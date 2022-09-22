# Golang Routine

All of go routines are keeped recorde by allgs slice in the runtime. Some goroutines are active, some are IO waiting, channel blocking, scheduling or locking. The goroutine has a lot of properties which can help debug the application. The gid, atomic status and gopc are the basic properties. The waiting reason can help us to know why the goroutine is waiting. 

The g, m, p data model are defined in the runtime source file. The fields of g are the goroutine properties. Go has implemented different strategies on verticals like concurrency, system calls, task scheduling, and memory modeling, among others. A Goroutine is defined as a lightweight thread managed by the Go runtime. Different Goroutines (G) can be executed on different OS threads (M), but at any given time, only one OS thread can be run on a CPU (P). In the user space, you achieve concurrency as the Goroutines work cooperatively. In the presence of a blocking operation (network, I/O or system call), another Goroutine can be assigned to the OS thread.

Goroutines live within the user thread space. In comparison to OS threads, their operations cost less: The overhead for assigning them, suspending them, and resuming them is lower than the overhead required by OS threads. Goroutines and channels are two of the most important primitives Go offers for concurrency. One important aspect of Goroutines is that expressing them in terms of code is fairly easy. You simply put the keyword go before the function you want to schedule to be run outside of the main thread.

Go comes with its own runtime scheduler. The language does not rely on the native OS thread/process scheduler, but it cooperates with it. Because the scheduler is an independent component, it has the flexibility for implementing optimizations. All these optimizations aim for one thing: to avoid too much preemption of the OS Goroutines, which would result in suspending and resuming the functions’ execution, an expensive operation.

