# Go Internals

## Go Trace

The trace file can be viewed by go tool trace command. 

```go
package main

import (
	"os"
	"runtime/trace"
    "net/http"
	"net/http/pprof"
)

func main() {
	f, err := os.Create("trace.out")
	if err != nil {
		panic(err)
	}
	defer f.Close()

	err = trace.Start(f)
	if err != nil {
		panic(err)
	}
	defer trace.Stop()

// second styles
    trace.Start(os.Stderr)
    defer trace.Stop()

// http trace
    r.HandleFunc("/debug/pprof/trace", pprof.Trace)
}
```

## Go Runtime

Go is a bit different from other languages in that it generates binaries that are fully self-contained. A system that executes a compiled Go binary doesn’t require a runtime or additional dependencies to be installed. This contrasts with languages such as Java or .NET that require a user to install a runtime before binaries will execute correctly. With Go's approach, the compiler embeds runtime code for various language features (e.g., garbage collection, stack traces, type reflection) into each compiled program. This is a major reason why Go binaries are larger than an equivalent program written in a language such as C. In addition to the runtime code, the compiler also embeds metadata about the source code and its binary layout to support language features, the runtime, and debug tooling.

Go binaries without debug symbols, also referred to as stripped binaries, provide a unique challenge to reverse engineers. Without symbols, analyzing a binary can be extremely complex and time consuming. With symbols restored, a reverse engineer can begin to map disassembled code back to its original source. 

The go keyword starts a new thread of execution that cooperatively interlaces execution with the rest of the application. This is not an operating system thread; instead, multiple Go routines take turns executing on one thread. Communication across Go routines is done via "channels" in a message passing fashion. When a channel is allocated, an interface type is passed to the runtime routine runtime.makechan, defining the type of data flowing across the channel. Data can be sent and received across a channel using the <- operator. Based on the direction, the routine runtime.chansend or runtime.chanrecv will be present in the disassembly. Channel logic is often adjacent to Go routine code, which begins execution by passing a function pointer to the runtime routine runtime.newproc.

The pclntab structure is short for “Program Counter Line Table”. The design for this table is documented on this page, but has evolved in more recent Go versions. The table is used to map a virtual memory address back to the nearest symbol name to generate stack traces with function and file names. The original specification states this information can be used for language features such as garbage collection, but this doesn’t appear to be true in modern runtime versions. For symbol recovery purposes, the pclntab is important because it stores function names, function start and end addresses, file names, and more.

Locating the pclntab works differently depending on the file format. For ELF and Mach-O files, the pclntab is located in a named section  within the binary. ELF files contain a section named .gopclntab that stores the pclntab while Mach-O files use a section named __gopclntab. Locating the pclntab in PE files is more complex and begins with identifying a symbol table referred to in the Go source code as .symtab.

For PE files, the .symtab symbol table is pointed to by the FileHeader.PointerToSymbolTable field. A symbol in this table named runtime.pclntab contains the address of the pclntab. ELF and Mach-O files also have a .symtab symbol table that contains a runtime.pclntab symbol but do not rely on it to locate the pclntab. To locate the .symtab in ELF files, look for a section named .symtab of type SH_SYMTAB. In Mach-O files, the .symtab is referenced by an LC_SYMTAB load command. The following is a list of relevant symbols present in the .symtab:

## Task

From the kernel’s point of view there is no such concept as threads. Linux implements all threads as processes, and the kernel has no special scheduling algorithm to handle threads. A thread is simply seen as a process that shares some resources with other processes. Like processes, each thread has its own task_struct, so in the kernel, a thread appears to be a normal process. Threads are also called lightweight processes. A process can have multiple threads, which have their own independent stack and are scheduled by the operating system to switch. On Linux they can be created with the pthread_create() method or the clone() system call.

Standard processes that run independently in kernel space. The difference between kernel threads and normal threads is that kernel threads do not have a separate address space.

When a Golang program starts, it first creates a process, then a main thread, which executes some code for runtime initialization, including scheduler initialization, and then starts the scheduler, which keeps looking for a goroutine that needs to be run to bind to a kernel thread.

A process can be associated with multiple threads, and the threads will share some resources of the process, such as memory address space, open files, process base information, etc. Each thread will also have its own stack and register information, etc. Threads are lighter than processes, while goroutines are lighter than threads, and multiple goroutines will be associated with one thread. Each goroutine will have its own stack space, so it will also be more lightweight. From process to thread to goroutine, it is actually a process of continuous sharing to reduce switching costs.

A goroutine is essentially a function that can be suspended and resumed. When a goroutine is created, a section of space is allocated in the heap of the process, which is used to store the goroutine stack area, and then copied from the heap when the goroutine needs to be resumed to restore the runtime state of the function.

GPM model in the scheduler, starting with an understanding of the roles and connections of the three components of the GPM model.

G: Goroutine, the function we run in a Go program using the go keyword.
M: Machine, or worker thread, which stands for system thread, and M is an object in runtime that creates a system thread and binds to that M at the same time as each M is created.
P: Processor, similar to the concept of a CPU core, which can execute Go code only when M is associated with a P.

When a Golang program starts, the main thread creates the first goroutine to execute the main function, and a new goroutine is created if the user uses the go keyword in the main function, and the user can continue to create new goroutines in the goroutine using the go keyword. goroutines are created by calling the newproc() function in the golang runtime. The number of goroutines is limited by system resources (CPU, memory, file descriptors, etc.). If there is only simple logic in a goroutine, it is theoretically fine to have as many goroutines as you want, but if there are operations such as creating network connections or opening files in a goroutine, too many goroutines may result in reports such as too many files open or Resource temporarily unavailable and so on, causing the program to execute abnormally.

When G exits, the goexit() function is executed and the state of G changes from _Grunning to _Gdead. However, the G object is not freed directly, but is put into the local or global idle list gFree of the associated P for reuse via gfput(). Priority is given to the P local queue, and if there are more than 64 gFree in the P local queue, only 32 will be kept in the P local queue, and any more than that will be put into the global idle queue sched.gFree.

## Memory Allocation

The Go language has a built-in runtime (that is, runtime) that abandons the traditional way of allocating memory in favor of autonomous management. This allows for autonomous implementation of better memory usage patterns, such as memory pooling, pre-allocation, etc. This way, you don’t need to make a system call for every memory allocation. 

The Golang runtime memory allocation algorithm is derived from Google’s TCMalloc algorithm for C. The core idea is to reduce the granularity of locks by dividing memory into multiple levels of management. It manages the available heap memory in a two-level allocation: each thread maintains a separate memory pool of its own and allocates memory from that pool first, and only applies to the global memory pool when the pool is insufficient to avoid frequent competition between different threads for the global memory pool.

Whether a variable is allocated on the stack or on the heap is determined by the result of the escape analysis. Usually, the compiler prefers to allocate variables on the stack because it has less overhead, and the most extreme is “zero garbage”, where all variables are allocated on the stack so that there is no memory fragmentation, garbage collection, or anything like that.

## Profiling

There are multiple way to identify the opportunity area

runtime/pprof is a tool for visualization and analysis of profiling data.
It’s useful for identifying where your application is spending its time (CPU and memory).
net/http/pprof serves via its HTTP server runtime profiling data in the format expected by the runtime/pprof visualization tool.
pkg/profile provides a simple way to manage runtime/pprof profiling of your Go application
runtime/trace contains facilities for programs to generate traces for the Go execution tracer
runtime/debug contains facilities for programs to debug themselves while they are running

Profiling and benchmarking should be part of earlier stages when the code is still in baking stage and not merged into production. 


