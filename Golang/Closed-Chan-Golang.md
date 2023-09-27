# Tricky closed channel in Golang

When you close a channel in Go, it's essential to understand how receiving data from the channel works after it has been closed. If you attempt to receive data from a closed channel, the receiver will still receive the remaining values in the channel buffer, if any, without blocking. However, if the channel is empty, subsequent receives will immediately return a zero-value for the channel's type.

Here's an example to illustrate this behavior:

```go
package main

import "fmt"

func main() {
    ch := make(chan int, 3)

    // Send some data into the channel
    ch <- 1
    ch <- 2
    ch <- 3

    // Close the channel
    close(ch)

    // Receiving data from the closed channel
    fmt.Println(<-ch) // 1
    fmt.Println(<-ch) // 2
    fmt.Println(<-ch) // 3

    // Receiving from an empty channel after it's closed
    fmt.Println(<-ch) // 0
    fmt.Println(<-ch) // 0
}
```

In this example, we create a buffered channel of capacity 3 and send three values into it. After closing the channel, we can still receive the three values from the channel without blocking. However, when we try to receive additional values from the closed channel, they will return the zero-value for integers (which is 0) because the channel is now empty.

To avoid this, you can use the second value returned from the receive operation to detect whether the channel has been closed, like this:

```go
package main  
  
import (  
   "fmt"  
)  
  
func Value(c chan int) (val int, ok bool) {  
   val, ok = <-c  
   return  
}  
  
func main() {  
   bc := make(chan int, 3)  
  
   bc <- 1  
   bc <- 2  
   bc <- 3  
  
   close(bc)  
  
   fmt.Println(Value(bc))  
   fmt.Println(Value(bc))  
   fmt.Println(Value(bc))  
  
   fmt.Println(Value(bc))  
   fmt.Println(Value(bc))  
}
```

By using the `ok` variable, you can differentiate between a valid received value and the zero-value that occurs when reading from a closed, empty channel.