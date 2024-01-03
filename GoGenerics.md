# Go Generics

Here is a basic example of `sum` function.

```go
package main

import (
    "fmt"
)

func Sum[V int64 | float64](m ...V) V {
    var s V
    for _, v := range m {
        s += v
    }

    return s
}

func main() {
    fmt.Println(Sum([]int64{1, 2, 3, 4, 5}...))
    fmt.Println(Sum(1.2, 1.42, 15.231))
}
```

You can make these definition easy most of the time. For example, if you want numeric type:

```go
package main

import (
    "golang.org/x/exp/constraints"
)

func Sum[V constraints.Float | constraints.Intger](m ...V) V {
    var s V
    for _, v := range m {
        s += v
    }

    return s
}

func main() {
    fmt.Println(Sum([]int64{1, 2, 3, 4, 5}...))
    fmt.Println(Sum(1.2, 1.42, 15.231))
}
```

The expression `~string` means the set of all types whose underlying type is `string`. This includes the type `string` itself as well as all types declared with definitions such as `type MyString string`.

For example, with this feature you can have such a function for all of your type

```go
func Contains[V comparable](slice []V, value V) bool {  
    for _, item := range slice {  
       if item == value {  
          return true  
       }  
    }  
    return false  
}
```


