# Testing

## Unit testing

By adding `_test` to the end of another file, you can create tests for that code. You could use the built-in package named `testing` for testing!

The functions actually should start with the word `Test` just like below:

```go
func TestSpcuLogger(t *testing.T) {
	logger, err := NewLogger(SpcuLoggerConfig{})
	if err != nil {
		t.Fatalf(err.Error())
	}
	logger.Traceln("This is a trace log.")
	logger.Println("This is a print mode.")
	logger.Infoln("This is an information log.")
	logger.Warningln("This is a warning log.")
	logger.Warnln("This is a warn log.")
	logger.Debugln("This is a debug log.")
	logger.Errorln("This is an error log.")
}
```

As you can see, there is a `testing.T` as an argument.

### To run the tests

You can run all of your tests by using:

```bash
go test ./...
```

`./...` actually looks for `.go` files recursively.

If you want to see the results, you can use the `-v` flag:

```bash
go test -v ./...
```

### Showing test coverage

I am not sure but I think `-race` make the test to work in parallel.

```bash
go test -race ./... -v -coverprofile=coverage.out && go tool cover -func=coverage.out
```

# Benchmark

Here is how to run a benchmark. The code looks like this

```go
package main

import (
   "sync"
   "sync/atomic"
   "testing"
)

func BenchmarkAtomic(b *testing.B) {
   var counter uint32 = 0
   for i := 0; i < b.N; i++ {
      atomic.AddUint32(&counter, 1)
   }
}

func BenchmarkMutex(b *testing.B) {
   var counter uint32 = 0
   mu := sync.Mutex{}
   for i := 0; i < b.N; i++ {
      mu.Lock()
      counter++
      mu.Unlock()
   }
}
```

With this command you can run the benchmark

```bash
go test -bench=.
```