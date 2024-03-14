# 100 Go Mistakes and How to Avoid Them

## #1 Unintended Variable Shadowing

In Go, a variable declared in a block can be redeclared in an inner block. This concept is called variable shadowing. You should be careful because it is going to change the value in the inner scope. It is not going to change the outer one.

```go
package main

import (
	"fmt"
	"net/http"
)

func main() {
	tracing := true

	var client *http.Client
	if tracing {
		client := http.Client{
			Transport: nil,
			CheckRedirect: func(req *http.Request, via []*http.Request) error {
				return nil
			},
			Timeout: 30,
		}
		fmt.Printf("%+v %+v\n", client, client.Timeout)
	} else {
		client := http.Client{
			Transport: nil,
			CheckRedirect: func(req *http.Request, via []*http.Request) error {
				return nil
			},
			Timeout: 10,
		}
		fmt.Printf("%+v %+v\n", client, client.Timeout)
	}

	// This will print the zero value of http.Client because of the shadowing
	fmt.Printf("%+v\n", client)
}
```
