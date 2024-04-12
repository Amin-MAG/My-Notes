# C library in Go

C library refers to the ability to interact with C code directly from Go programs. This functionality is primarily provided through the `cgo` tool, which allows Go code to call C functions and use C types.

## `cgo` Tool

The `cgo` tool allows Go code to import and use C libraries and call C functions directly. It also enables Go code to be called from C code. `cgo` generates Go code that interfaces with the C code and handles the necessary conversions between Go and C types.

## Simple Usage

Let's consider the following C code.

```c
#include <stdio.h>

int add(int a, int b) { return a + b; }

int main() {
  // Get the input
  int num1, num2;
  printf("Enter two numbers: ");
  scanf("%d %d", &num1, &num2);

  // Call the add function
  int sum = add(num1, num2);
  printf("the sum of the number %d and %d is %d\n", num1, num2, sum);

  return 0;
}
```

This code contains `add` and `main` functions. In the `main` function sum of the two number is going to be calculated using `add` function. To run the C code, as you might know

```bash
# Compile the code
gcc addition.c -o run

# Execute
./run
```

Since we can not have two main functions, I'm going to change the name `main` to `exec_cli`. We import the `"C"` package, which allows us to call C functions and use C types.

```go
package main

/*
#include "addition.c"
*/
import "C"
import "fmt"

func main() {
	fmt.Println("Hello, World!")

	result := C.add(12, 13)
	_ = C.exec_cli()
	fmt.Println("Result: ", result)
}
```
