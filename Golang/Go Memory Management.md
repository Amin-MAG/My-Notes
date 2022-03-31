# Go Memory management

# Memory leak

When programming in a language supporting auto garbage collection, generally we don't need care about memory leaking problems, for the runtime will collect unused memory regularly. However, we do need to be aware of some special scenarios which may cause kind-of or real memory leaking. The remaining of the current article will list several such scenarios.

You can use `unsafe` package to measure the object size.

For example, to find out the boolean size

```go
package main

import (
	"fmt"
	"time"
	"unsafe"
)

func main() {
	b := true
	fmt.Println(unsafe.Sizeof(b))
}
```

The output is `1` Byte

Consider This struct, It should have `1+1+8+8=18` Bytes.

```go
package main

import (
   "fmt"
   "unsafe"
)

type Foo struct {
   A bool // 1
   B int64 // 8
   C bool // 1
   D float64 // 8
}

func main() {
   f :=  Foo{}
   fmt.Println(unsafe.Sizeof(f)) // prints 32!
}
```

It’s wired at first, but the answer is `32` Bytes.

There are bunch of tips here:

- Minimum struct size is 1 byte
- The struct size should be a coefficient of maximum field size in struct.
    - For this example is `8`: So now it should be `24` or bigger.
- The starting address point for each field in struct should be a coefficient of the size of that field.

So The first field is 1 byte then byte 0 is for A. We miss the 1-7 blocks and then 8-16 is for B. Again we use one byte that make the next 7 block useless. The 24-32 is for the last D.

The obvious solution to optimize this is to sort the fields based on their size descending. There is no rule for this memory optimization. In some cases, It’s not important to think about these things because we don’t care about the memory usage that much.

There are also some linters that sort these fields automatically.

# References

[An overview of memory management in Go](https://medium.com/safetycultureengineering/an-overview-of-memory-management-in-go-9a72ec7c76a8)

[آهای گولنگ، مموری اضافه‌ی من کجاست؟](https://virgool.io/golangpub/hey-golang-where-is-my-memory-oky2gqaxd5ly)