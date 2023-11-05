# Golang

## To Assembly

To see the assembly code. 

```bash
go tool compile -S file.go > file.s
```

## Data Race

Data races are among the most common and hardest-to-debug types of bugs in concurrent systems. A data race occurs when two goroutines access the same variable concurrently and at least one of the accesses is a write. See the [The Go Memory Model](https://go.dev/ref/mem/) for details.

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

## Upload endpoint

First of all, we usually consider the size of the file:

```go
func upload(w http.ResponseWriter, r *http.Request){
	// Set max upload size
	if err := r.ParseMultipartForm(2 * 1024 * 1024); err != nil {
		fmt.Printf("Could not parse multipart form: %v\n", err)
		renderError(w, "CANT_PARSE_FORM", http.StatusInternalServerError)
		return
	}

	// ...
}
```

To grab the file and validate the uploaded file:

```go
func upload(w http.ResponseWriter, r *http.Request){
	// ...

	// uploadFile is the name of the file in request
	file, fileHeader, err := r.FormFile("uploadFile")
	if err != nil {
		renderError(w, "INVALID_FILE", http.StatusBadRequest)
		return
	}
	defer file.Close()
	
	// Grab the file size in bytes 
	fileSize := fileHeader.Size
	if fileSize > 2 * 1024 * 1024 {
		renderError(w, "FILE_TOO_BIG", http.StatusBadRequest)
		return
	}

	// Read the file
	fileBytes, err := ioutil.ReadAll(file)
	if err != nil {
		renderError(w, "INVALID_FILE", http.StatusBadRequest)
		return
	}

	// check file type, detectcontenttype only needs the first 512 bytes
	detectedFileType := http.DetectContentType(fileBytes)
	switch detectedFileType {
	case "image/jpeg", "image/jpg":
	case "image/gif", "image/png":
	case "application/pdf":
		break
	default:
		renderError(w, "INVALID_FILE_TYPE", http.StatusBadRequest)
		return
	}

	// ...
}

```

To save the file:

```go
func upload(w http.ResponseWriter, r *http.Request){
	// ...

	// Choose the file name and path
	fileName := randToken(12)
	fileEndings, err := mime.ExtensionsByType(detectedFileType)
	if err != nil {
		renderError(w, "CANT_READ_FILE_TYPE", http.StatusInternalServerError)
		return
	}
	newPath := filepath.Join("./tmp", fileName+fileEndings[0])

	// Write file
	newFile, err := os.Create(newPath)
	if err != nil {
		renderError(w, "CANT_WRITE_FILE", http.StatusInternalServerError)
		log.Warnf("error: %s", err.Error())
		return
	}
	defer newFile.Close()

	if _, err := newFile.Write(fileBytes); err != nil || newFile.Close() != nil {
		renderError(w, "CANT_WRITE_FILE", http.StatusInternalServerError)
		return
	}
	w.Write([]byte("SUCCESS"))
}

```

[https://github.com/zupzup/golang-http-file-upload-download](https://github.com/zupzup/golang-http-file-upload-download)

## Serve a directory

You need to create `http.FileServer()`

```go
fs := http.FileServer(http.Dir("./tmp"))
http.Handle("/files/", http.StripPrefix("/files", fs))
log.Errorln(http.ListenAndServe(":8080", nil))
```

## Upload a File

Consider this application that has a path and an endpoint to upload the file.

```bash
func main() {
	file := flag.String("file", "", "path of the file you want to upload")
	flag.Parse()

	err := postFile(*file, "http://localhost:8080/upload")
	if err != nil {
		log.Warnln(err.Error())
	}
}
```

## Read Files

Get statistics about a file.

```go
f, err := os.Open(name)  
if err != nil {  
   log.Println(err)  
   return  
}  
defer f.Close()  
  
stats, err := f.Stat()  
if err != nil {  
   log.Println(err)  
   return  
}  
log.Printf("%+v", stats)
```

Read the contents of a file

```go
contents, err := os.ReadFile(name)  
if err != nil {  
   log.Println(err)  
   return  
}  
  
fmt.Println(string(contents))
```

Read the content of a file line by line

```go
f, err := os.Open(name)  
if err != nil {  
   log.Println(err)  
   return  
}
defer f.Close()

s := bufio.NewScanner(f)  
for s.Scan() {  
   fmt.Printf("- %s \n", s.Text())  
}
```

Read the content of a file word by word

```go
f, err := os.Open(name)  
if err != nil {  
   log.Println(err)  
   return  
}
defer f.Close()
  
s := bufio.NewScanner(f)  
s.Split(bufio.ScanWords)  
for s.Scan() {  
   fmt.Println(s.Text())  
}
```

Read the content of a file bytes by bytes

```go
f, err := os.Open(name)  
if err != nil {  
   log.Println(err)  
   return  
}  
defer f.Close()  
  
buf := make([]byte, size)  
for {  
   totalRead, err := f.Read(buf)  
   if err != nil && err != io.EOF {  
      log.Println(err.Error())  
      return  
   }  
   if err != nil && err == io.EOF {  
      fmt.Println(string(buf[:totalRead]))  
      return  
   }  
   fmt.Println(string(buf[:totalRead]))  
}
```

## Check the variable type

```go
package main  
  
import (  
   "fmt"  
)  
  
func main() {  
   var t interface{}  
   t = functionOfSomeType()  
   switch t := t.(type) {  
   default:  
      fmt.Printf("unexpected type %T\n", t) // %T prints whatever type t has  
   case bool:  
      fmt.Printf("boolean %t\n", t) // t has type bool  
   case int:  
      fmt.Printf("integer %d\n", t) // t has type int  
   case *bool:  
      fmt.Printf("pointer to boolean %t\n", *t) // t has type *bool  
   case *int:  
      fmt.Printf("pointer to integer %d\n", *t) // t has type *int  
   }  
}
```

## `defer` 

Deferred functions are executed in LIFO order, so this code will cause `4 3 2 1 0` to be printed when the function returns. A more plausible example is a simple way to trace function execution through the program.

```go
for i := 0; i < 5; i++ {
    defer fmt.Printf("%d ", i)
}
```

## `new` VS `make`

The `make` function is used to create slices, maps, and channels. These data structures need to be initialized with specific internal data structures, and `make` ensures that the underlying data is properly initialized and ready for use. It returns a reference to the initialized data structure.

```go
package main

import "fmt"

func main() {
    // Create a slice of integers with length 3 and capacity 5
    mySlice := make([]int, 3, 5)

    // Add elements to the slice
    mySlice[0] = 10
    mySlice[1] = 20
    mySlice[2] = 30

    // Output the slice
    fmt.Println(mySlice) // Output: [10 20 30]
}
```

The `new` function is used to allocate memory for a new value of a specified type and returns a pointer to the newly allocated memory. It doesn't initialize the memory, but it zeroes out the allocated memory, which means that the new value will have the default zero value of its type.

```go
package main

import "fmt"

type Person struct {
    Name string
    Age  int
}

func main() {
    // Create a new instance of the Person struct using new
    personPtr := new(Person)

    // As new allocates memory, we can directly assign values to the struct fields through the pointer
    personPtr.Name = "John"
    personPtr.Age = 30

    // Output the person object (note the dereferencing with *)
    fmt.Println(*personPtr) // Output: {John 30}
}
```

In summary, `make` is used for creating and initializing slices, maps, and channels, while `new` is used for allocating memory for a new value of a specified type (usually for structs) and returns a pointer to that memory.

## `make(MyStruct)` VS `&MyStruct{}`

- `new(MyStruct)` returns a pointer to a zero-initialized instance of `MyStruct`.
- `&MyStruct{}` returns a pointer to a new `MyStruct` instance with specified initial values for its fields.

As a limiting case, if a composite literal contains no fields at all, it creates a zero value for the type. The expressions `new(File)` and `&File{}` are equivalent.



# Topics

- [Build Executive Binaries](Build.md)
- [ENT Database](ENT.md)
- [Benchmark](GoBenchmark.md)
- [Graph QL](Go%20GraphQL.md)
- [Go Bot](GoBot.md)
- [Go Cron](GoCron.md)
- [Golang, Real-Time Techs](Go%20Real-time.md)
- [gRPC](gRPC.md)
- [Golang Mock](Mock.md)
- [Go Generic](GoGenerics.md)
- [Goroutines](Goroutines.md)
- [Go patterns](GoPatterns.md)
- [Go Design Patterns](Golang/Go-Design-Patterns.md)
- [Go Refactorings](GoRefactorings.md)
- [Gzip](gzip.md)
- [Hugo](Hugo.md)
- [Issues](Issues.md)
- [Memory management](Go%20Memory%20Management.md)
- [Prometheus in Go](Go%20Prometheus.md)
- [Rate](Rate.md)
- [Serverless in Go](Serverless-in-go.md)
- [Go Swagger](Go%20Swagger.md)
- [Testify](Testify.md)
- [Go Testing](Go%20Testing.md)
- [Go Effective](Go-Effective.md)
- [Go Networking](Go-Networking.md)

- [Close Chan](Golang/Closed-Chan-Golang.md)

# Libraries

Progress bar:

[https://github.com/cheggaaa/pb](https://github.com/cheggaaa/pb)