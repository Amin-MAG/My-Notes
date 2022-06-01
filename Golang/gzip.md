# Gzip

Using `gzip` compression to upload and download a file.

## Send using a gzip compression

To send a file to a `gzip` handler, I create an entry that you can pass the file's name, and it will send this file to the `http://localhost:8080/gzip`. (I will explain this handler later.)

```go
func main() {
	// Get file path from the command line flag
	file := flag.String("file", "", "path of the file you want to upload")
	flag.Parse()

	// Call upload API to upload that file
	err := postFile(*file, "http://localhost:8080/gzip")
	if err != nil {
		log.Warnln(err.Error())
	}
}
```

To post this file

```go
func postFile(filename string, targetUrl string) error {
	// Open the file
	in, err := os.Open(filename)
	if err != nil {
		log.Fatal(err)
	}

	// gzip writes to pipe, http reads from pipe
	pr, pw := io.Pipe()

	// buffer readers from file, writes to pipe
	buf := bufio.NewReader(in)

	// gzip wraps buffer writer and wr
	gw := gzip.NewWriter(pw)

	// Actually start reading from the file and writing to gzip
	go func() {
		log.Printf("Start writing")
		n, wErr := buf.WriteTo(gw)
		if wErr != nil {
			log.Fatal(wErr)
		}
		gw.Close()
		pw.Close()
		log.Printf("Done writing: %d", n)
	}()

	// Create new request
	req, err := http.NewRequest("POST", targetUrl, pr)
	if err != nil {
		return err
	}

	// Do the HTTP Request
	resp, err := http.DefaultClient.Do(req)
	defer resp.Body.Close()
	respBody, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		return err
	}
	fmt.Println(resp.Status)
	fmt.Println(string(respBody))

	return nil
}
```

## Create a gzip Handler

```go
func gzipHandler(rw http.ResponseWriter, r *http.Request) {
	log.Printf("Recv'd request")
	defer r.Body.Close()

	// Read gzip file
	gr, err := gzip.NewReader(r.Body)
	if err != nil {
		log.Fatal(err)
	}
	defer gr.Close()

	data, err := ioutil.ReadAll(gr)
	if err != nil {
		log.Fatal(err)
	}
    // ...
}


func randToken(len int) string {
	b := make([]byte, len)
	rand.Read(b)
	return fmt.Sprintf("%x", b)
} 
```

## Save a gzip file from a byte array

I defined some utility functions.

```go
type F struct {
	f  *os.File
	gf *gzip.Writer
	fw *bufio.Writer
}

func CreateGZ(s string) (f F) {
	fi, err := os.OpenFile(s, os.O_WRONLY|os.O_APPEND|os.O_CREATE, 0660)
	if err != nil {
		log.Printf("Error in Create\n")
		panic(err)
	}
	gf := gzip.NewWriter(fi)
	fw := bufio.NewWriter(gf)
	f = F{fi, gf, fw}
	return
}

func WriteGZ(f F, data []byte) {
	(f.fw).Write(data)
}

func CloseGZ(f F) {
	_ = f.fw.Flush()
	// Close the gzip first.
	_ = f.gf.Close()
	_ = f.f.Close()
}


func randToken(len int) string {
	b := make([]byte, len)
	rand.Read(b)
	return fmt.Sprintf("%x", b)
}
```

I passed the data to the `WriteGZ()` to create gzip file.

```go
func gzipHandler(rw http.ResponseWriter, r *http.Request) {
    // ...
    data, err := ioutil.ReadAll(gr)
	if err != nil {
		log.Fatal(err)
	}
	_, err = rw.Write([]byte(fmt.Sprintf("Recv'd %d plaintext bytes", len(data))))

	fileName := fmt.Sprintf("%s.gz", randToken(12))
	newPath := filepath.Join(uploadPath, fileName)

    // Save the gzip file
	f := CreateGZ(newPath)
	WriteGZ(f, data)
	CloseGZ(f)

	_, err = rw.Write([]byte("SUCCESS"))
}
```


