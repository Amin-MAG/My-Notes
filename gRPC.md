# gRPC

gRPC (gRPC Remote Procedure Calls) is a high-performance, open-source framework developed by Google that allows you to define and invoke remote services using a protocol buffer interface.

## Use cases

- **Microservices Architecture:** gRPC is well-suited for communication between microservices in a distributed system. 
- **Internal APIs:** When both the client and server are developed in-house and can use the same communication protocol, gRPC can provide efficiency and performance benefits.

## REST API vs. gRPC

- **Advantages of gRPC:**
    1. **Performance:** gRPC generally has better performance compared to REST API, primarily because it uses a binary protocol (Protocol Buffers) and supports features like multiplexing.
    2. **Strong Typing:** Protocol Buffers provide a more structured and strongly typed approach compared to JSON in REST APIs.
- **Advantages of REST API:**
    1. **Simplicity:** REST is simpler to understand and implement. It relies on standard HTTP methods and is easy to work with using tools like cURL or browser-based requests.
    2. **Widely Adopted:** REST has been the traditional choice for many web applications and is widely supported by various tools and frameworks.

# See More

- [gRPC in Golang](Golang/gRPC.md)
