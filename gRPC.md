---
title: gRPC
draft: true
tags: [backend, networking, system-design, reference]
---
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


# grpcurl

[grpcurl](https://github.com/fullstorydev/grpcurl) is a command-line tool that lets you interact with gRPC servers, similar to how `curl` works for REST APIs. It supports server reflection, making it easy to explore and test gRPC services without needing the original `.proto` files.

---

## Prerequisites

For most commands to work, the gRPC server must have **reflection enabled** (see [Enabling Reflection](##Enabling%20Reflection) below). Without it, you'll need to pass the `.proto` files manually using the `-proto` flag.

---

## List Available Services

Discover all services exposed by the server.

```bash
# -plaintext disables TLS (use for local/dev servers only)
grpcurl -plaintext 127.0.0.1:50051 list
```

---

## List Methods in a Service

Inspect all RPC methods available on a specific service.

```bash
grpcurl -plaintext 127.0.0.1:50051 list grpc.health.v1.Health
```

---

## Call a Method

Invoke an RPC method on the server.

```bash
# Without a request body (e.g. for methods that take google.protobuf.Empty)
grpcurl -plaintext 127.0.0.1:50051 grpc.health.v1.Health/Check

# With a JSON request body
grpcurl -d '{"service": "my.Service"}' -plaintext 127.0.0.1:50051 grpc.health.v1.Health/Check
```

> The `-d` flag accepts a JSON string that maps to the request message fields.

---

## Enabling Reflection

Server reflection allows grpcurl to discover your services and their message schemas at runtime, without needing `.proto` files on the client side.

Add the following to your Go gRPC server setup:

```go
import "google.golang.org/grpc/reflection"

// Register reflection service on the gRPC server.
// This should be called after registering your own services.
reflection.Register(grpcServer)
```

> ⚠️ It is recommended to enable reflection only in **development or staging** environments, as it exposes your full API surface.
# See More

- [gRPC in Golang](Golang/gRPC.md)
