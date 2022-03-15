# gRPC

## Installation

First install the compiler `protoc` then install the go lang plugin

```bash
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go get -u google.golang.org/grpc
```

# Simple gRPC

Here we will implement a very basic server and clients. This server has a service responsible for calculating the majority of a list.

## Proto Files

```protobuf
syntax = "proto3";

option go_package = "bale_interview/apis";

package bale_interview;

service Majority {
  rpc CalculateListMajority(ListMajorityRequest) returns (ListMajorityReply) {}
}

// The request message containing the user's name.
message ListMajorityRequest {
  repeated int32 numbers = 1;
}

// The response message containing the greetings
message ListMajorityReply {
  string message = 1;
  int32 majorityNumber = 2;
}
```

`repeated` is for defining an array in proto files.

### Generating the Go files

You need to use `protoc` command to generate go files:

```bash
$ protoc --go_out=. --go_opt=paths=source_relative \
      --go-grpc_out=. --go-grpc_opt=paths=source_relative \
      apis/majority.proto
```

You specify the source, the output directory, and the proto file.  After executing this command some Go files will be generated.

## Server-side code

First you should create listener.

```go
lis, err := net.Listen("tcp", port)
if err != nil {
		log.Fatalf("failed to listen: %v", err)
}
```

Create the gRPC server. You can pass on some options if you want.

```go
s := grpc.NewServer()
```

Then you should register your services defined in proto files. First, You should pass on a type that implements that interface.

```go
type TestServer struct {
	*grpc.Server
	apis.UnimplementedMajorityServer
}

// CalculateListMajority to find most repeated number
// that repeated more than size / 2 times
func (s *TestServer) CalculateListMajority(ctx context.Context, in *apis.ListMajorityRequest) (*apis.ListMajorityReply, error) {
	integerCounter := make(map[int32]int32)
	listSize := int32(len(in.Numbers))

	// fill the integer counter
	fillIntegerCounter(in, integerCounter)

	// find the most repeated item
	exists, maxim := findMostRepeatedItemGreaterHalf(integerCounter, listSize)

	if exists == false {
		return &apis.ListMajorityReply{
			Message:        "Not found any majority",
			MajorityNumber: 0,
		}, nil
	} else {
		return &apis.ListMajorityReply{
			Message:        "majority is Founded",
			MajorityNumber: maxim,
		}, nil
	}
}
```

Here `TestServer` implements the interface. So we can use this struct to register.

```go
s := grpc.NewServer()
apis.RegisterMajorityServer(s, &TestServer{})
```

That's it.

## Client-Side code

It depends on what your client is. Here we assume we use Go language for clients too. This client uses the majority function to calculate the most repeated number.

```go
func main() {
	// Set up a connection to the server.
	conn, err := grpc.Dial(address, grpc.WithInsecure(), grpc.WithBlock())
	if err != nil {
		log.Fatalf("did not connect: %v", err)
	}
	defer conn.Close()

	c := apis.NewMajorityClient(conn)

	ctx, cancel := context.WithTimeout(context.Background(), time.Second)
	defer cancel()

	r, err := c.CalculateListMajority(ctx, &apis.ListMajorityRequest{Numbers: []int32{
		0, 0, 1, 1, 1, 1,
	}})
	if err != nil {
		log.Println(err.Error())
	}

	if r != nil {
		log.Printf("%s", r.GetMessage())
		log.Printf("%+v", r.GetMajorityNumber())
	}
}
```

# Simple gRPC (With TLS)

Generate the certificates

```bash
# CA
openssl req -x509 -newkey rsa:4096 -days 365 -keyout ca-key.pem -out ca-cert.pem

# Server Key
openssl req -newkey rsa:4096 -keyout server-key.pem -out server-req.pem

# Sigining and Generating the server certificate
openssl x509 -req -in server-req.pem -CA ca-cert.pem -CAkey ca-key.pem -CAcreateserial -out server-cert.pem

# Verify
openssl verify -CAfile ca-cert.pem server-cert.pem

# To remove the passphrase
openssl rsa -in key.pem -out key.unencrypted.pem -passin pass:<PHRASE>

# Here is the original
# There is an issue if you want to run it locally in 0.0.0.0
# 1. Generate CA's private key and self-signed certificate
openssl req -x509 -newkey rsa:4096 -days 365 -nodes -keyout ca-key.pem -out ca-cert.pem -subj "/C=FR/ST=Occitanie/L=Toulouse/O=Tech School/OU=Education/CN=*.techschool.guru/emailAddress=techschool.guru@gmail.com"

echo "CA's self-signed certificate"
openssl x509 -in ca-cert.pem -noout -text

# 2. Generate web server's private key and certificate signing request (CSR)
openssl req -newkey rsa:4096 -nodes -keyout server-key.pem -out server-req.pem -subj "/C=FR/ST=Ile de France/L=Paris/O=PC Book/OU=Computer/CN=*.pcbook.com/emailAddress=pcbook@gmail.com"

# 3. Use CA's private key to sign web server's CSR and get back the signed certificate
openssl x509 -req -in server-req.pem -days 60 -CA ca-cert.pem -CAkey ca-key.pem -CAcreateserial -out server-cert.pem -extfile server-ext.cnf

echo "Server's signed certificate"
openssl x509 -in server-cert.pem -noout -text
```

# Simple gRPC bi-directional streaming

Here I want to create a simple server that calculates the prime factors of the given number. This connection is bi-directional streaming

## Defining the proto file

To specify that we want to use streaming we use `stream` keyword before the definition. In this example, I want to use bi-directional streaming. So I use the keyword before both the request and response.

```protobuf
syntax = "proto3";

option go_package = "prime/api";
package prime;

service Factors {
  // The stream keyword is specified before both the request type and response
  // type to make it as bidirectional streaming RPC method.
  rpc CalculatePrimeFactors (stream CalculatePrimeFactorsRequest) returns (stream CalculatePrimeFactorsResponse) {}
}

message CalculatePrimeFactorsRequest {
  int64 num = 1;
}

message CalculatePrimeFactorsResponse {
  int64 result = 1;
}
```

### Using a make file to generate the protofile

You can create a `Makefile`, when you want to generate the code in multiple languages.

```makefile
all: client server

client:
	@echo "--> Generating Kotlin client files"
	python3 -m grpc_tools.protoc -I protobuf/ --python_out=. --grpc_python_out=. protobuf/primefactor.proto
	@echo ""

server:
	@echo "--> Generating Go files"
	protoc -I protobuf/ --go_out=plugins=grpc:protobuf/ protobuf/primefactor.proto
	@echo ""

install:
	@echo "--> Installing Python grpcio tools"
	pip3 install -U grpcio grpcio-tools
	@echo ""
```

And then execute make command:

```bash
$ make
```

But for this example we use go clients.

```bash
$ protoc --go_out=. --go_opt=paths=source_relative \
      --go-grpc_out=. --go-grpc_opt=paths=source_relative \
      api/prime_factor.proto
```

## Server-Side code

First create a listener:

```go
lis, err := net.Listen("tcp", fmt.Sprintf(":%d", port))
	if err != nil {
	log.Fatalln("ERROR:", err.Error())
}
```

Create the gRPC server. You can pass on some options if you want.

```go
s := grpc.NewServer()
```

For registering `CalculatePrimeFactors` we need to pass a struct that implements the interface.

```go
type primeService struct {
	*grpc.Server
	api.UnimplementedFactorsServer
}

func NewPrimeService(s *grpc.Server) *primeService {
	return &primeService{
		Server: s,
	}
}

func (ps *primeService) CalculatePrimeFactors(stream api.Factors_CalculatePrimeFactorsServer) error {
	log.Println("Entering CalculatePrimeFactors")

	for {
		log.Println("waiting for new receive")
		req, err := stream.Recv()
		if err == io.EOF {
			break
		}
		if err != nil {
			return err
		}
		log.Println("Received:", req.Num)

		// time.Sleep(time.Second)

		c := make(chan int64)
		go findPrimeFactors(c, req.Num)
		for n := range c {
			resp := &api.CalculatePrimeFactorsResponse{
				Result: n,
				Done:   false,
			}
			_ = stream.Send(resp)
		}
		_ = stream.Send(&api.CalculatePrimeFactorsResponse{
			Done: true,
		})
	}
	log.Println("Leaves CalculatePrimeFactors")

	return nil
}

// findPrimeFactors
func findPrimeFactors(c chan int64, num int64) {
	var i int64 = 2
	var numSqrt = int64(math.Sqrt(float64(num)))

	for ; i <= numSqrt; {
		if num%i == 0 {
			c <- i
			for num%i == 0 {
				num /= i
			}
		}

		i++
	}

	if num > 1 {
		c <- num
	}

	close(c)
}
```

About stream functions:

- `stream.Recv()` will wait until some message comes from client.
- `io.EOF` will check if the stream has been closed or not.
- `stream.Send()` will send values to the client.
- When stream is closed, we should get out of this function.

## Client-Side code

Setup the connection:

```go
// Set up a connection to the server.
conn, err := grpc.Dial(address, grpc.WithInsecure(), grpc.WithBlock())
if err != nil {
	log.Fatalln("cannot connect:", err)
}
defer func() {
	_ = conn.Close()
}()
```

Connect to the stream:

```go
// Setup the stream
c := api.NewFactorsClient(conn)
stream, err := c.CalculatePrimeFactors(context.Background())
if err != nil {
	log.Fatalln(err)
}
```

This stream is bi-directional so we should create a separate goroutine for receiving from server.

```go
{
	// In main function after creating stream
	wc := make(chan struct{})
	go receiveFromServer(wc, stream)
}

func receiveFromServer(wc chan struct{}, stream api.Factors_CalculatePrimeFactorsClient) {
	for {
		in, err := stream.Recv()
		if err == io.EOF {
			fmt.Println("closing the stream from client")
			return
		}
		if err != nil {
			fmt.Println("Failed to receive a note :", err)
		}

		if in.Done {
			wc <- struct{}{}
		} else {
			fmt.Println("Got new factor:", in.Result)
		}
	}
}
```

This function will receive each time something comes from server stream.

After this goroutine we implement an input mechanism to get number from user. Then send the numbers to the server using the `stream.Send()`.

```go
for {
		reader := bufio.NewReader(os.Stdin)
		fmt.Print("Enter a number: ")
		text, _ := reader.ReadString('\n')
		text = strings.Replace(text, "\n", "", -1)

		if text == "q" {
			break
		}

		num, err := strconv.Atoi(text)
		if err != nil {
			fmt.Println("bad input")
		}

		_ = stream.Send(&api.CalculatePrimeFactorsRequest{
			// Input new number
			Num: int64(num),
	})

		<-wc
}
```

It will wait for the last receive and then iterate again. At the end we should close the stream:

```go
_ = stream.CloseSend()
time.Sleep(2 * time.Second)
```

> **Note: I don't know the reason but you don't use the `time.sleep()` the server is still waiting!**
> 

# gRPC Tools

## `grpcurl`

Use `grpcurl` to test your server

```bash
brew install grpcurl

# Download image
docker pull fullstorydev/grpcurl:latest
# Run the tool
docker run fullstorydev/grpcurl api.grpc.me:443 list
```

To get the list of services

```bash
grpcurl -plaintext localhost:8787 list
```

To get the list of methods inside a service

```bash
grpcurl -plaintext localhost:8787 list models.Majority
```

Then to call the method run this command

```bash
grpcurl -plaintext -d '{"numbers": [1,2,3,5,5,5,5]}' localhost:8787 models.Majority/CalculateListMajority
```

## `grpcui`

Install the tool

```bash
go install github.com/fullstorydev/grpcui/cmd/grpcui@latest
```

To interact with the user interface run

```bash
grpcui -plaintext localhost:8787
```

# Load balancing

You can use some services that support the gRPC load balancing.

There are bunch of ways for load balancing these kind of services.

## Server Side Load balancing

As you can see clients are not aware of the gRPC servers. The LB component is responsible for dispatching and load balancing these connections.

A Network Load Balancer operates at the layer-4 of [OSI (Open Systems Interconnection) model](https://en.wikipedia.org/wiki/OSI_model).

Remember now that gRPC connections are **sticky** and **persistent**, so it will hold on to the **same connection** between the client and same server instance behind the load balancer, as long as it can.

![Untitled](gRPC%20db5d8/Untitled.png)

**⚠️**  Issues:

- **Sticky connections and Auto Scaling (Read more about this from link later)**

## Client Side Load Balancing

Instead of separate component in our system we can have client-side load balancing and based on the state of the server we make a new connection to the appropriate server.

This is a viable option, under one condition: **If you have full control over all the clients**.

![Untitled](gRPC%20db5d8/Untitled%201.png)

For an example, we can use Round-Robin in this situation. Go has this algorithm built-in.

```go
var riderAddresses []resolver.Address
for i := 0; i < 10; i++ {
	riderAddresses = append(riderAddresses, resolver.Address{Addr: c.AddrWithCustomPort(8080 + i)})
	log.InfoS("Add new connection", "address", c.AddrWithCustomPort(8080 + i))
}

// Create resolver
rb := manual.NewBuilderWithScheme("rider")
rb.InitialState(resolver.State{
	Addresses: []resolver.Address{
		// Here should be the servers' IP addresses
	},
})

// Set up a connection to the server.
log.DebugS(
	"going to create new connection to",
	"host", c.Host, "port", c.Port,
)
conn, err := grpc.Dial(
	"rider:///balancer",
	grpc.WithTransportCredentials(insecure.NewCredentials()),
	grpc.WithDefaultServiceConfig(fmt.Sprintf(`{"loadBalancingConfig": [{"%s":{}}]}`, roundrobin.Name)),
	grpc.WithResolvers(rb),
)
```

## Look Aside

[As recommended by the official gRPC load balancing](https://grpc.io/blog/grpc-load-balancing/#lookaside-load-balancing), this method is using an external load balancer or **one-arm load balancer** for distributing the traffic between server instances.

Client reaches out to an external service and it will return a list of available servers, service discovery and all other required information.

![Untitled](gRPC%20db5d8/Untitled%202.png)

## Resource

[GitHub - ridha/grpc-streaming-demo: A quick demo of bi-directional streaming RPC's using grpc, go and python3](https://github.com/ridha/grpc-streaming-demo)

[Why load balancing gRPC is tricky?](https://majidfn.com/blog/grpc-load-balancing/)