# Golang Routine

All of go routines are keeped recorde by allgs slice in the runtime. Some goroutines are active, some are IO waiting, channel blocking, scheduling or locking. The goroutine has a lot of properties which can help debug the application. The gid, atomic status and gopc are the basic properties. The waiting reason can help us to know why the goroutine is waiting. 