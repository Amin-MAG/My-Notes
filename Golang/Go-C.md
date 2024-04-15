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

## Type Conversion

| C Type         | Go Equivalent       |
|----------------|---------------------|
| int            | C.int               |
| unsigned int   | C.uint              |
| long           | C.long              |
| unsigned long  | C.ulong             |
| long long      | C.longlong          |
| unsigned long long | C.ulonglong     |
| float          | C.float             |
| double         | C.double            |
| char           | C.char              |
| const char*    | *C.char             |
| void*          | unsafe.Pointer      |
| struct         | C.struct_*          |
| pointer        | *C.type             |
| array          | *C.type             |
| union          | C.union_*           |

- In Go, types from the "C" package are used to represent C types. For example, `C.int` represents an `int` in C.
- Pointers to C types are represented as `*C.type` in Go.
- C arrays are represented as pointers in Go. For example, `*C.int` represents a pointer to an `int` array in C.
- `unsafe.Pointer` in Go is used to represent `void*` in C. It is typically used for memory addresses and raw memory manipulation, and its use should be handled with caution due to potential safety issues.
- Structs and unions from C can be represented using their respective types prefixed with `C.struct_` or `C.union_` in Go.

## C Functions with Callback 

In some cases, you may need to pass a Go function as a callback to a C function. This involves using function pointers in C and the `unsafe` package in Go. Here's the `callback.h` file

```c
#include <stdio.h>  

// The function type which is the argument of call_from_c
typedef int (*callback)(int);  
  
// Forward declaration of the C function that accepts the callback  
void call_from_c(callback cb);  
  
// Exporting the Go callback function  
// We can not pass an anonymous/inline function.
// The function should be defined in C.
extern int goCallback(int value);
```

The function `call_from_c` has an argument of type function that receive an integer and returns an integer.

Here is the `callback.c` file which runs the callback with the value of 5

```c
#include "callback.h"  
#include <stdio.h>  
  
void call_from_c(callback cb) {  
  int r = cb(5);  
  printf("The return value of go function in C is %d\n", r);  
}
```

Now to pass the callback in Go 

```go
package main  
  
/*  
#include "callback.h"  
*/  
import "C"  
import (  
    "fmt"  
    "unsafe"
)  
  
//export goCallback  
func goCallback(value C.int) C.int {  
    v := int(value)  
    fmt.Println("Go callback called with result:", v*v*v)  
    res := v * v * v  
    return C.int(res)  
}  
  
func main() {  
    C.call_from_c((C.callback)(unsafe.Pointer(C.goCallback)))  
}
```

- Importing `callback.h` contains all of necessary informations
- The `export` keyword on top of goCallback register the function in C.
- Keep in mind to use `C.types` like `C.int` in the implementation of the function.
- Also `go run main.go` might not work, because you need all of Go and C files. So you should run the `go build .` or `go run .`
## Using C Libraries and Header Files Without WrapperYou can directly import the C file and use it in Go. For the previous `math` example,```gopackage main/*#include <math.h>#cgo LDFLAGS: -lm*/import "C"import (	"fmt")func main() {	x := 16.0	result := C.sqrt(C.double(x))	fmt.Printf("Square root of %.1f is %.1f\n", x, result)}```## Memory Management and Pointers

## Memory Management and Pointers

When interfacing with C code, you might need to manage memory explicitly, especially when dealing with pointers and dynamically allocated memory

## Using C Libraries and Header Files with Wrapper

If you're working with existing C libraries, you may need to include their header files and link against their compiled binaries. Let's work on the math package, here is the `math.h` file

```c
double c_sqrt(double x);
```

We can define functions to wrap and connect the C library to the Go code. Here is the `math.c`

```c
#include <math.h>

double c_sqrt(double x) { return sqrt(x); }
```

Here is the Go code to use the library through the C function that we defined

```go
package main

/*
#cgo LDFLAGS: -lm
#include "math.h"
*/
import "C"
import (
	"fmt"
)

func main() {
	x := 16.0
	result := C.c_sqrt(C.double(x))
	fmt.Printf("Square root of %.1f is %.1f\n", x, result)
}
```
