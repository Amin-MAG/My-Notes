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

Another Simple example to sum 2 number

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

### Channels will wait until receive a value

The first point is that when we send something through the pipe it does not matter how much it takes to receive that data. It even does not matter that we receive it or not.

```go
c := make(chan int)  
  
go func(res chan int) {  
   time.Sleep(3 * time.Second)  
   res <- 3  
}(c)  
  
time.Sleep(time.Second * 15)  
// If you remove the line bellow nothing will happen,
// We just ignore the channel
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
    // Ensure that the section does not exceed its capacity    // Use the result channel to signal whether the operation was successful    inv.mu.Lock()  
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
    // Use the result channel to signal whether the operation was successful    inv.mu.Lock()  
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


When we are waiting to receive data from a channel, and at one point, we discover that there is no goroutine!

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


