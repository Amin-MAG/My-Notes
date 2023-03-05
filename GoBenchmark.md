# Go Benchmark

Let's assume this is your function:

```go
func MyFunctionName(a, b int) int {
	return a + b
}
```

You need to create a test file for your benchmark function. Then add this function:

```go
func BenchmarkMyFunctionName(b *testing.B) {  
   for i := 0; i < b.N; i++ {  
      MyFunctionName(1, 2)  
   }
}
```

To run benchmark

```bash
# Just run a benchmark
go test -bench=BenchmarkMyFunctionName

# Run all of the benchmarks (You can set some custom RegEx here)
go test -bench=.

# Run all of the benchmarks with memory statistics
go test -bench=. -benchmem

# Specify how long each benchmark should be run
go test -bench=. -benchtime=10s
```