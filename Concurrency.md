# Concurrency

## Classic Problems

### The Dining Philosopher

The Dining Philosophers problem is a classic OS problem that's usually stated in very non-OS terms: There are N philosophers sitting around a circular table eating spaghetti and discussing philosophy. The problem is that each philosopher needs 2 forks to eat, and there are only N forks, one between each 2 philosophers.

```go
// Implementation of 5 philosopher with 5 fork 

package main

import (
	"fmt"
	"sync"
)

var philosophers = []string{"Plato", "Jack", "Max", "Pascal", "Locke"}
var forks = []*sync.Mutex{&sync.Mutex{}, &sync.Mutex{}, &sync.Mutex{}, &sync.Mutex{}, &sync.Mutex{}}
var orderFinished []string
var hunger = 3

func diningProblem(philosopher string, left, right *sync.Mutex, rc chan string) {
	fmt.Println(philosopher, "is seated.")

	for i := hunger; i > 0; i-- {
		fmt.Println(philosopher, "is hungry.")

		left.Lock()
		fmt.Printf("\t%s picked up the fork to his left.\n", philosopher)
		right.Lock()
		fmt.Printf("\t%s picked up the fork to his right.\n", philosopher)

		fmt.Printf("\t%s picked up both of forks.\n", philosopher)

		fmt.Println(philosopher, "is eating.")

		left.Unlock()
		fmt.Println(philosopher, "put down the fork on his left.")
		right.Unlock()
		fmt.Println(philosopher, "put down the fork on his right.")
	}

	fmt.Println(philosopher, "eating is done.")

	fmt.Println(philosopher, "has left the table.")
	rc <- philosopher
}

// Keep in mind that picking left fork simultaneously
// is one of the possibilities. (Deadlock)
func main() {
	rc := make(chan string)
	for i := 0; i < len(philosophers); i++ {
		rightForkIdx := i + 1
		if i+1 >= len(philosophers) {
			rightForkIdx = 0
		}

		go diningProblem(philosophers[i], forks[i], forks[rightForkIdx], rc)
	}

	for i := 0; i < len(philosophers); i++ {
		orderFinished = append(orderFinished, <-rc)
	}
	close(rc)
	fmt.Println(orderFinished)
}
```

### The Sleeping Barber
## Concurrency Patterns

### Worker Pool

limit the total CPU usage

We can create as many thread or routines as we want, but We should consider the bottleneck in the real world. For instance, in computing, 

- When a network can only handle a certain number of outbound connection
- When a website can only handle a certain number of simultaneous connection

And that's why we use worker pools.

### Pipeline