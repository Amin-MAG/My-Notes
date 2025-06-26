# Goroutines


Goroutines and threads are both concurrency mechanisms used in programming, but they operate differently, especially in the context of languages like Go.

## Goroutines VS Threads

One important question is the difference between Goroutines and Threads.

|                                                                               Goroutines                                                                               |                                                                 Threads                                                                 |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------------------------------------: |
|                                              Goroutines are **lightweight user-space** threads managed by the Go runtime.                                              |                               Threads are the **smallest unit of execution** within an operating system.                                |
|                                                        They are designed to be **cheap** to create and manage.                                                         |                        Threads are managed by the OS kernel and are more **heavyweight** compared to goroutines.                        |
| Goroutines are **multiplexed onto a smaller number of OS threads**. This means that thousands or even millions of goroutines can run concurrently on a few OS threads. |                           Each thread **has its own stack and resources** allocated by the operating system.                            |
|       Communication between goroutines is typically done using **channels**, which are built-in constructs for safely passing data between concurrent processes.       | Threads can be created and managed directly using **threading libraries** provided by the programming language or the operating system. |
|                                 Goroutines are part of the Go language's concurrency model and are **specifically optimized** for it.                                  |     Threads are generally **more expensive to create and manage** compared to goroutines due to their **reliance on OS resources**.     |

## Trade-offs of using Goroutines

There are a couple of trade-offs in using goroutine that you should know

| Advantages        | Disadvantages        |
| ----------------- | -------------------- |
| Concurrency<br>   | Complexity           |
| Lightweight<br>   | Resource Management  |
| Simple Syntax<br> | Performance Overhead |
| Composition       |                      |

# Channels
  
In Go, channels are a powerful construct for facilitating communication and synchronization between goroutines, enabling safe concurrent operations. This is a simple code using channels:

```go
package main

import "fmt"

func main() {

    messages := make(chan string)

    go func() { messages <- "ping" }()

    msg := <-messages
    fmt.Println(msg)
}
```

Here is another example to sum 2 number

```go
package main

import "fmt"

func sum(s []int, c chan int) {
	sum := 0
	for _, v := range s {
		sum += v
	}
	c <- sum // send sum to c
}

func main() {
	s := []int{7, 2, 8, -9, 4, 0}

	c := make(chan int)
	go sum(s[:len(s)/2], c)
	go sum(s[len(s)/2:], c)
	x, y := <-c, <-c // receive from c

	fmt.Println(x, y, x+y)
}
```

Channels in GoLang can be categorized into two main types based on their behavior: Unbuffered and Buffered channels

|                                                     Unbuffered Channel                                                     |                                                           Buffered Channel                                                            |
| :------------------------------------------------------------------------------------------------------------------------: | :-----------------------------------------------------------------------------------------------------------------------------------: |
|                                   Unbuffered channels have no capacity for storing data                                    |                                       Buffered channels have a fixed capacity for storing data                                        |
|    When a goroutine sends data to an unbuffered channel, it blocks until another goroutine is ready to receive the data    |                                Sending data to a buffered channel blocks only when the buffer is full                                 |
|       This synchronous behavior ensures that the sender and receiver are synchronized at each point of communication       | Buffered channels allow a certain degree of decoupling between the sender and receiver, as they don't have to synchronize immediately |

Additionally, channels can be typed based on the type of data they can transmit.

## Channels will wait until receive a value

When sending data through a channel in Go, it doesn't matter how long it takes for the receiver to receive the data. Although, If you do not receive the data from channel, the channel will be blocked and can not send any new data.

```go
c := make(chan int)

go func(res chan int) {
	time.Sleep(3 * time.Second)
	res <- 3
}(c)

time.Sleep(time.Second * 5)
// If you remove this line below, It is just
// going to block the channel for sending new data
fmt.Println(<-c)
```

Although, It should be in another goroutine.

```go
// This causes error
c := make(chan int)  
  
c <- 3  
  
time.Sleep(time.Second * 4)  
fmt.Println(<-c)
```

## Using Mutex and Channels simultaneously

It's tricky and you should be careful when you are using the channels and mutex at the same time. If you want to send a new value through the channel, first the mutex should be unlocked. Here is an example of correct usage,

```go
type Inventory struct {  
    Sections map[string]int  
    mu       sync.Mutex  
}  
  
func NewInventory() *Inventory {  
    return &Inventory{  
       Sections: make(map[string]int),  
    }  
}  
  
func (inv *Inventory) addItem(section string, quantity int, result chan bool) {  
    // Implement the logic to add items to the inventory section  
    // Ensure that the section does not exceed its capacity    
    // Use the result channel to signal whether the operation was
    // successful
    inv.mu.Lock()  
    addedSuccessfully := true  
    defer func() {  
       inv.mu.Unlock()  
       result <- addedSuccessfully  
    }()  
  
    if _, ok := inv.Sections[section]; !ok {  
       inv.Sections[section] = quantity  
       return  
    }  
  
    inv.Sections[section] = inv.Sections[section] + quantity  
    return  
}  
  
func (inv *Inventory) removeItem(section string, quantity int, result chan bool) {  
    // Implement the logic to remove items from the inventory section  
    // Use the result channel to signal whether the operation was successful
    inv.mu.Lock()  
    removedSuccessfully := true  
    defer func() {  
       inv.mu.Unlock()  
       result <- removedSuccessfully  
    }()  
    if _, ok := inv.Sections[section]; !ok {  
       removedSuccessfully = false  
       return  
    }  
  
    availableQuantity := inv.Sections[section]  
    if availableQuantity-quantity < 0 {  
       removedSuccessfully = false  
       return  
    }  
  
    inv.Sections[section] = inv.Sections[section] - quantity  
    return  
}  
  
func (inv *Inventory) displayInventory() {  
    // Implement the logic to display the current state of the inventory  
    fmt.Println("Current Inventory:")  
    for section, quantity := range inv.Sections {  
       fmt.Printf("%s: %d\n", section, quantity)  
    }  
    fmt.Println("--------------------")  
}  
  
func main() {  
    // Create an instance of the Inventory  
    inventory := NewInventory()  
  
    addA := make(chan bool)  
    addB := make(chan bool)  
    removeA := make(chan bool)  
    removeB := make(chan bool)  
  
    // Example usage: Start multiple goroutines to add and remove items concurrently  
    go inventory.addItem("SectionA", 5, addA)  
    go inventory.addItem("SectionB", 3, addB)  
    go inventory.removeItem("SectionA", 2, removeA)  
    go inventory.removeItem("SectionB", 1, removeB)  
  
    fmt.Printf("%v\t%v\t%v\t%v\n", <-addA, <-addB, <-removeA, <-removeB)  
  
    // Display the final state of the inventory  
    inventory.displayInventory()  
}
```

## Buffer channel

You can specify how much you want to buffer for your channel. In this example, We can continue to execute the code by having `res <- 3` in our buffer. By default, there is no buffer. It usually means we block the sender code, and it should wait for the receiver to receive it.

```go
c := make(chan int, 1)  
  
go func(res chan int) {  
   time.Sleep(1 * time.Second)  
   fmt.Println("here we go")  
   res <- 3  
   fmt.Println("here we go")  
   res <- 4  
   fmt.Println("here we go")  
   res <- 5  
   fmt.Println("here we go")  
   res <- 6  
   fmt.Println("here we go")  
   res <- 7  
   fmt.Println("here we go")  
   res <- 8  
   fmt.Println("here we go")  
   res <- 9  
}(c)  
  
time.Sleep(time.Second * 3)  
fmt.Println(<-c)  
fmt.Println(<-c)  
  
time.Sleep(4 * time.Second)  
//fmt.Println(<-c)
```

## Closing channel

```go
package main

import "fmt"

func main() {
	// Describes our tasks
    jobs := make(chan int, 5)
	// To handle the join
    done := make(chan bool)

	// A goroutine to receive everything
    go func() {
        for {
			// Close channel returns false for more
            j, more := <-jobs
            if more {
                fmt.Println("received job", j)
            } else {
                fmt.Println("received all jobs")
                done <- true
                return
            }
        }
    }()

    for j := 1; j <= 3; j++ {
        jobs <- j
        fmt.Println("sent job", j)
    }
    close(jobs)
    fmt.Println("sent all jobs")

    <-done
}
```

## Range over channels

Take care of closing the channel. Remember that `close(chan)` is not something to be stored in the buffer.

```go
func main() {    
	queue := make(chan string, 2)  
	queue <- "one"  
	queue <- "two"  
	close(queue)  
	
	// It will print the both of values
	for elem := range queue {  
	  fmt.Println(elem)
	}
}
```

## Group Wait

```go
func main() {
    var wg sync.WaitGroup

    for i := 1; i <= 5; i++ {
        wg.Add(1)

        go func() {
            defer wg.Done()
			time.Sleep(time.Second)
        }()
    }

    wg.Wait()
}
```


# Mutex

Mutex helps in preventing data races and ensuring safe concurrent access to shared data. There are two kinds of Mutexes: `sync.Mutex` and `sync.RWMutex`.

Both `sync.Mutex` and `sync.RWMutex` are synchronization primitives provided by the Go standard library (`sync` package) to control access to shared resources in a concurrent environment. Here are the differences

|                                                                                             `sync.Mutex`                                                                                             |                                                                                                                  `sync.RWMutex`                                                                                                                   |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `sync.Mutex` is a mutual exclusion lock. It allows only one goroutine to acquire the lock at a time, ensuring that only one goroutine can access the critical section of code protected by the lock. |                                             `sync.RWMutex` is a reader-writer lock. It allows multiple readers to acquire the lock simultaneously or only one writer to acquire the lock exclusively.                                             |
|                      When a goroutine acquires the lock using `Lock()` method, it becomes the owner of the lock until it explicitly releases the lock using `Unlock()` method.                       | Multiple goroutines can acquire the lock in read mode (`RLock()`), allowing concurrent read access to the shared resource. However, only one goroutine can acquire the lock in write mode (`Lock()`), preventing concurrent reads and writes.<br> |
|                         If another goroutine attempts to acquire the lock while it's already locked, it will be blocked until the lock is released by the owning goroutine.                          |                                                      When a goroutine acquires the lock in write mode, it blocks all other goroutines (readers and writers) until it releases the lock.<br>                                                       |
|                           `Mutex` is suitable for scenarios where exclusive access to a resource is required, and there is a need to prevent concurrent reads and writes.                            |                                                             `RWMutex` is suitable for scenarios where reads are frequent and writes are infrequent, allowing for better parallelism.                                                              |

## `sync.Mutex`

```go
var (
	counter int
	mutex   sync.Mutex
)

func increment() {
	mutex.Lock()
	defer mutex.Unlock()
	counter++
}

func main() {
	wg := sync.WaitGroup{}
	wg.Add(10)

	for i := 0; i < 10; i++ {
		go func() {
			increment()
			wg.Done()
		}()
	}

	wg.Wait()
	fmt.Println("Counter:", counter) // Output: Counter: 10
}
```

## `sync.RWMutex`

```go
var (
	data    map[string]string
	rwMutex sync.RWMutex
)

func readData(key string) string {
	rwMutex.RLock()
	defer rwMutex.RUnlock()
	return data[key]
}

func writeData(key, value string) {
	rwMutex.Lock()
	defer rwMutex.Unlock()
	data[key] = value
}

func main() {
	data = make(map[string]string)

	// Writing data
	go writeData("1", "one")
	go writeData("2", "two")
	go writeData("3", "three")

	// Reading data
	go fmt.Println("Read:", readData("1"))
	go fmt.Println("Read:", readData("2"))
	go fmt.Println("Read:", readData("3"))

	time.Sleep(time.Second)
	fmt.Println("Data:", data)
}
```


# Pools

The `sync.Pool` package in Go provides a simple mechanism for managing a pool of temporary objects, such as frequently allocated but short-lived objects like buffers or goroutines. The primary purpose of `sync.Pool` is to reduce memory allocation overhead and improve performance by reusing objects from a pool instead of allocating new ones each time they are needed.

## Key Features

Here are some key features and purposes of the `sync.Pool` package:

1. **Object Pooling**:

	- `sync.Pool` maintains a pool of objects that can be temporarily borrowed and returned by goroutines.
    - Instead of allocating new objects every time, goroutines can borrow objects from the pool when needed and return them to the pool when they are no longer in use.
    - This helps reduce the frequency of memory allocations and deallocations, which can improve performance and reduce memory fragmentation.

2. **Garbage Collection Optimization**:
    
    - Reusing objects from a pool can help reduce pressure on the garbage collector by decreasing the number of objects that need to be garbage-collected.
    - By reusing objects from a pool, the overall memory footprint of the program may be reduced, leading to more efficient garbage collection and lower memory usage.

3. **Concurrency-Safe**:
    
    - The `sync.Pool` package is designed to be safe for concurrent use by multiple goroutines.
    - Goroutines can borrow and return objects from the pool concurrently without requiring external synchronization.
    - The pool internally manages synchronization to ensure safe access to pool objects across multiple goroutines.

4. **Automatic Eviction**:
    
    - The `sync.Pool` package automatically evicts objects from the pool when they have been unused for a certain period.
    - This helps prevent the pool from growing too large and consuming excessive memory, especially in scenarios where objects are rarely reused.

5. **Temporary Storage**:
    
    - `sync.Pool` is particularly useful for managing short-lived temporary objects, such as buffers or small data structures, that are frequently allocated and deallocated.
    - By reusing these temporary objects from a pool, the overhead of memory allocation and deallocation can be reduced, improving overall application performance.

## Usage

In this example, we'll create a pool of database connections. This scenario simulates a common use case where multiple goroutines need to interact with a database, and we want to reuse database connections to avoid the overhead of creating new connections each time.

> This might not be a good example, since database connections are not short-term objects. Use pool functionalities inside the database packages for production.
>  

```go
// Create a new sync.Pool
pool := sync.Pool{
	New: func() interface{} {
		// Create a new database connection
		conn, err := sql.Open("mysql", "user:password@tcp(localhost:3306)/database")
		if err != nil {
			panic(err)
		}
		return &DatabaseConnection{
			ID:   1, // Assign an ID to the connection (for demonstration)
			// Initialize other fields if needed
		}
	},
}

// Get a connection from the pool
dbConn := pool.Get().(*DatabaseConnection)
```

We can use database connections in the pool in different goroutines. If there is no free object in the pool, it will create a new connection using the `New()` function.

# Common errors

## Deadlock

When we are waiting to receive data from a channel, and at one point, we discover that there is no goroutines.

```go
func worker(done chan bool) {  
   fmt.Print("working...")  
   time.Sleep(time.Second)  
   fmt.Println("done")  
  
   //done <- true  
}

func main() {  
   done := make(chan bool, 1)  
   go worker(done)  
  
   <-done  
}
```


