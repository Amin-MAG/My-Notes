# Go Arena

In Go, the "arena" refers to the memory allocation strategy used by the Go runtime to manage memory for small objects. It's a term closely associated with the way Go handles memory allocation and garbage collection.

## Stack vs Heap

The stack is used for **local variables** and is managed efficiently because it's a **fixed-size memory** allocated per goroutine. The heap is used for **dynamically allocated memory**, such as when you create variables with `new()` or `make()`.

## Arena

The Go runtime divides the heap into several "arenas." Each arena is a contiguous block of memory dedicated to small object allocations.

When you allocate small objects (typically smaller than 32 bytes), Go doesn't use the general-purpose heap allocator. Instead, it has a dedicated allocator called the "tiny allocator," which allocates memory from the arenas.

Arenas are typically around **64KB in size**. The Go runtime manages these arenas efficiently to minimize fragmentation and overhead.

```go
func main() {
	// Example 1: Small object allocation
	// Since the slice is relatively small,
	// it's likely to be allocated from an
	// arena using the tiny allocator
	smallObject := make([]int, 10)
	fmt.Println("Small object:", smallObject)

	// Example 2: Allocation of large object
	// This allocation is likely to bypass the
	// tiny allocator and use the general-purpose
	// heap allocator
	largeObject := make([]int, 1000000)
	fmt.Println("Large object:", largeObject)
}
```

## Main Functions and methods

- **NewArena**: Creates a new arena area.
- **New**: Allocate a new pointer to the type provided.
- **NewSlice**: Allocate a new slice having the length and capacity
- **Free**: Frees all the data in the arena

```go
func main() {
	// Allocates the new arena memory onto Golang's unsafe pointer.
	mem := arena.NewArena()

	// New[T](arena.Arena) Creates a new pointer to the
	// type provided to the generic function in arena memory.
	intObject := arena.New[int](mem)
	stringObject := arena.New[string](mem)
	float64Slice := arena.MakeSlice[float64](mem, 100, 200)

	*intObject = 10
	*stringObject = "Hello, World!"

	fmt.Println("Int object:", *intObject)
	fmt.Println("String object:", *stringObject)
	fmt.Println("Float64 slice:", float64Slice)

	// Frees the arena memory and all objects
	// allocated from the arena.
	mem.Free()

	// Accessing the value after free may result in a fault,
	// but this fault is also not guaranteed.
	fmt.Println("Int object:", *intObject)
	fmt.Println("String object:", *stringObject)
	fmt.Println("Float64 slice:", float64Slice)
}
```

- **Clone**: Makes a shallow copy of the input value to the heap

```go
func main() {
	mem := arena.NewArena()

	o1 := arena.New[T](mem) // Arena memory
	o2 := arena.Clone(o1)   // Heap memory

	fmt.Println(o1 == o2) // false: Not the same object
	mem.Free()
}
```