# Goroutines

## Channels

Very simple code for channels

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

## Some challanging scenarios

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

It should be in another goroutine although.

```go
// This causes error
c := make(chan int)  
  
c <- 3  
  
time.Sleep(time.Second * 4)  
fmt.Println(<-c)
```

## Buffer channel

You can specify how much you want to buffer for your channel.

In this example, We can continue to execute the code by having `res <- 3` in our buffer. By default, there is no buffer. It usually means we block the sender code, and it should wait for the receiver to receive it.

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

## Common errors

### Deadlock

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

## Mutex

# Snippets

A pattern that you can execute recursive calls and gather the results back.

```go
package main

import "fmt"

func Hack(start string) []string {
	finalResult := []string{}

	tempResult := Retrieve(start)
	fmt.Println(tempResult)
	finalResult = append(finalResult, tempResult...)

	c := make(chan []string)
	lenOfItems := len(tempResult)
	for _, v := range tempResult {
		go func(v string, c chan []string) {
			subPath := Hack(v)
			c <- subPath
		}(v, c)
	}

	counter := 0
	for {
		if counter == lenOfItems {
			break
		}
		subPath := <-c
		counter++
		finalResult = append(finalResult, subPath...)
	}

	return finalResult
}
```