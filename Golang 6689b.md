# Golang

## To Assembly

To see the assembly code. 

```bash
go tool compile -S file.go > file.s
```

## Data Race

Data races are among the most common and hardest to debug types of bugs in concurrent systems. A data race occurs when two goroutines access the same variable concurrently and at least one of the accesses is a write. See the [The Go Memory Model](https://go.dev/ref/mem/) for details.

Here is an example of a data race that can lead to crashes and memory corruption:

```go
func main() {
	c := make(chan bool)
	m := make(map[string]string)
	go func() {
		m["1"] = "a" // First conflicting access.
		c <- true
	}()
	m["2"] = "b" // Second conflicting access.
	<-c
	for k, v := range m {
		fmt.Println(k, v)
	}
}
```

## Pointers

A kind of code that shows how pointers work

```go
func pointers() {
	log.Infoln("Simple variables")
	var a string
	var b string
	a = "amin"
	b = "ghasvari"
	log.Infoln(a, b)
	log.Infoln(&a, &b)

	log.Infoln("Pointers")
	var as *string
	var bs *string
	as = &a
	bs = &b
	log.Infoln(as, bs)
	log.Infoln(&as, &bs)
	log.Infoln(*as, *bs)

	log.Infoln("Assignments")
	changeThePointer(&a)
	log.Infoln(a)
	log.Infoln(&a)
	//changeThePointer(bs)
	//log.Infoln(bs)
	//log.Infoln(&bs)
	//log.Infoln(*bs)

	log.Infoln("Using temp variable for simple variable")
	temp := b
	log.Infoln(temp)
	log.Infoln(&temp)
	changeThePointer(&temp)
	log.Infoln(temp)
	log.Infoln(b)
	log.Infoln(&temp)

	log.Infoln("Using temp variable for pointers")
	temps := bs
	log.Infoln(temps)
	log.Infoln(&temps)
	log.Infoln(*temps)
	changeThePointer(bs)
	log.Infoln(*temps)
}

func changeThePointer(s *string) {
	if s == nil {
		log.Infoln("It is empty!")
		return
	}
	*s = "changed"
}
```

## Context

The main purpose of context is cancelation propagation. It also can pass values.

```go
type Context interface {
	Deadline() (deadline time.Time, ok bool)
	Done() <-chan struct{}
	Err() error
	Value(key interface{}) interface{}
}
```

Use `Background()` when you’re not reacting to anything and you don’t need the cancelation.

```go
// To create one 
ctx := context.Background()

ctx, cancel := context.WithTimeout(ctx, time.Second)
ctx, cancel := context.WithCancel(ctx)
ctx, cancel := context.WithDeadline(ctx, time.Now().Add(time.Second))

// To receive the data
c := <- ctx.Done()
```

## Read from console

To read a single string

```go
// Read a single input
var first string
fmt.Print("Enter Your First Name: ")
_, _ = fmt.Scanln(&first)
fmt.Printf("Your name is %s\n", first)
```

To read a line

```go
// Read a line
fmt.Print("Enter text: ")
reader := bufio.NewReader(os.Stdin)
inp, _ := reader.ReadString('\n')
inp = strings.TrimSuffix(inp, "\n")
fmt.Printf("Your text is: %s\n", inp)
fmt.Printf("Your text splited by spaces: %+v\n", strings.Split(inp, " "))
```

To read multiple lines

```go
// Read multiple lines of text
scanner := bufio.NewScanner(os.Stdin)
for {
	fmt.Print("Enter Text: ")
	scanner.Scan()
	text := scanner.Text()
	if len(text) != 0 {
		fmt.Printf("Your text is: %s\n", text)
		fmt.Printf("Your text splited by spaces: %+v\n", strings.Split(text, " "))
	} else {
		break
	}
}
```




# Topics

[Build Executive Binaries](Golang%206689b/Build%20Exec%2068159.md)

[ENT Database](Golang%206689b/ENT%20Databa%201f959.md)

[Graph QL](Golang%206689b/Graph%20QL%207ba83.md)

[Go Bot](Golang%206689b/Go%20Bot%20ad18f.md)

[Go Cron](Golang%206689b/Go%20Cron%2081e75.md)

[Golang, Real-Time Techs](Golang%206689b/Golang,%20Re%2067a69.md)

[Golang Mock](Golang%206689b/Golang%20Moc%20c1533.md)

[Goroutines](Golang%206689b/Goroutines%2001dc2.md)

[Issues](Golang%206689b/Issues%20f1a09.md)

[Memory management](Golang%206689b/Memory%20man%20079a0.md)

[Prometheus in Go](Golang%206689b/Prometheus%20962c0.md)

[Rate](Golang%206689b/Rate%20ff6fe.md)

[Swagger](Golang%206689b/Swagger%208fac4.md)

[Testify](Golang%206689b/Testify%203f1e8.md)

[Testing](Golang%206689b/Testing%2016c6f.md)

# Libraries

Progress bar:

[https://github.com/cheggaaa/pb](https://github.com/cheggaaa/pb)