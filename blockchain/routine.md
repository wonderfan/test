# Golang Routine

All of go routines are keeped recorde by allgs slice in the runtime. Some goroutines are active, some are IO waiting, channel blocking, scheduling or locking. The goroutine has a lot of properties which can help debug the application. The gid, atomic status and gopc are the basic properties. The waiting reason can help us to know why the goroutine is waiting. 

The g, m, p data model are defined in the runtime source file. The fields of g are the goroutine properties. Go has implemented different strategies on verticals like concurrency, system calls, task scheduling, and memory modeling, among others. A Goroutine is defined as a lightweight thread managed by the Go runtime. Different Goroutines (G) can be executed on different OS threads (M), but at any given time, only one OS thread can be run on a CPU (P). In the user space, you achieve concurrency as the Goroutines work cooperatively. In the presence of a blocking operation (network, I/O or system call), another Goroutine can be assigned to the OS thread.

Goroutines live within the user thread space. In comparison to OS threads, their operations cost less: The overhead for assigning them, suspending them, and resuming them is lower than the overhead required by OS threads. Goroutines and channels are two of the most important primitives Go offers for concurrency. One important aspect of Goroutines is that expressing them in terms of code is fairly easy. You simply put the keyword go before the function you want to schedule to be run outside of the main thread.

Go comes with its own runtime scheduler. The language does not rely on the native OS thread/process scheduler, but it cooperates with it. Because the scheduler is an independent component, it has the flexibility for implementing optimizations. All these optimizations aim for one thing: to avoid too much preemption of the OS Goroutines, which would result in suspending and resuming the functions’ execution, an expensive operation.

In order to properly prioritize your performance effort, the most valuable strategy is to identify your bottlenecks and focus on them. To achieve this, use profiling tools! Pprof and Trace are your friends. Goroutines is the feature that makes concurrency easy to implement in Golang. Though programmers choose to run the routines on a single core in most cases, there is a way to utilize more than one core to run the Goroutines. You can use “`GOMAXPROCS“` environment variable to set the number of cores that Goroutines uses. You need to choose this number wisely. If your Goroutines communicate a lot, it is better to be on one core. Else, using multiple cores can be advantageous.

Profilers do dynamic analysis of the program to measure the performance in many aspects, like CPU utilization and memory allocation. Profilers also point out the piece of code that is misbehaving: consuming a lot of the CPU’s capacity or making too many calls to a method. Library ants(https://github.com/panjf2000/ants) implements a goroutine pool with fixed capacity, managing and recycling a massive number of goroutines, allowing developers to limit the number of goroutines in your concurrent programs.

Go’s scheduler has three basic concepts. G is goroutine. M is an OS thread. P is a processor. P handles multiplexing some goroutines onto some OS threads. OS thread behaves like a worker for goroutine using a run queue. What is important is P uses a smart scheduling strategy called a work-stealing algorithm. Therefore P efficiently schedules available goroutines onto OS threads. 

## Networking

The network event has kqueue and epool mechanism for IO events. Go has a powerful built-in profiler that supports CPU, memory, goroutine and block (contention) profiling.Goroutine profile dumps the goroutine call stack and the number of running goroutines. Blocking profile shows function calls that led to blocking on synchronization primitives like mutexes and channels. A “Stop the World” (STW) is a crucial phase in some garbage collector algorithms to get track of the memory. It suspends the execution of the program to scan the memory roots and add write barriers. Stopping the program means stopping the running goroutines. The test, benchmark, profile and trace are important tools in the golang world. 

In many popular programming environments the stack usually refers to the call stack of a thread. A call stack is a LIFO stack data structure that stores arguments, local variables, and other data tracked as a thread executes functions. Each function call adds (pushes) a new frame to the stack, and each returning function removes (pops) from the stack.

Since threads are managed by the OS, the amount of memory available to a thread stack is typically fixed, e.g. a default of 8MB in many Linux environments. This means we also need to be mindful of how much data ends up on the stack, particularly in the case of deeply-nested recursive functions. If the stack pointer in the diagram above passes the stack guard, the program will crash with a stack overflow error.

The heap is a more complex area of memory that has no relation to the data structure of the same name. We can use the heap on demand to store data needed in our program. Memory allocated here can’t simply be freed when a function returns, and needs to be carefully managed to avoid leaks and fragmentation. The heap will generally grow many times larger than any thread stack, and the bulk of any optimization efforts will be spent investigating heap use.

Threads managed by the OS are completely abstracted away from us by the Go runtime, and we instead work with a new abstraction: goroutines. Goroutines are conceptually very similar to threads, but they exist within user space. This means the runtime, and not the OS, sets the rules of how stacks behave.

Go compilers will allocate variables that are local to a function in that function’s stack frame. However, if the compiler cannot prove that the variable is not referenced after the function returns, then the compiler must allocate the variable on the garbage-collected heap to avoid dangling pointer errors. Also, if a local variable is very large, it might make more sense to store it on the heap rather than the stack.

If a variable has its address taken, that variable is a candidate for allocation on the heap. However, a basic escape analysis recognizes some cases when such variables will not live past the return from the function and can reside on the stack. Stack traces play a critical role in Go profiling. Unwinding (or stack walking) is the process of collecting all the return addresses (elements in Stack Layout) from the stack. Frame pointer unwinding is the simple process of following the base pointer register (rbp) to the first frame pointer on the stack which points to the next frame pointer and so on. Symbolization is the process of taking one or more program counter (pc) address and turning them into human readable symbols such a function names, file names and line numbers. This article provides more details (https://github.com/DataDog/go-profiler-notes/blob/main/stack-traces.md).

## Perf command




