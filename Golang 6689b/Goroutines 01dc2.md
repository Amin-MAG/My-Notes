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