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