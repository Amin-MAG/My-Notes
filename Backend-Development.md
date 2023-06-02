# Backend Development

## API Architecture Styles

APIs or Application Programming Interfaces are the backbone of our interconnected digital world. They act as bridges, allowing distinct software components to communicate and interact. There several architectural style that can be used.

### SOAP

It's a veteran in the field, mature, comprehensive, and XML-based. SOAP is heavily used in financial service and payment gateways where security and reliability are key.

### RESTful

They are like the Internet's backbone. popular, easy to implement, and use HTTP methods. If you need real-time data or operate with a highly connected data model, This might not be the right choice.

### GraphQL

It is not only a API architecture style, but also a query language. The client can specify the exact required fields by the query language.  This means no more over-fetching or under-fetching of data. It leads to more efficient network communication and faster responses.

### gRPC

It's modern, high-performance, and uses Protocol Buffers. It is a favorite choice for microservices architectures.

### WebSocket

WebSocket is all about real-time, bidirectional, and persistent connections. It is perfect for live chat applications and real-time gaming, where low-latency is a key.

### Webhook

It is all about event-driven, HTTP callbacks, and asynchronous operation.

## User Aspect in HTTP connection

Users enters an URL to reach the website.

```bash
# Schema://Domain/Path/Resource
http://example.com/product/electric/phone
```

First, the browser needs to know how to reach the domain. This process is DNS lookup. DNS, Domain Name Server, is like a phone book that translate the domain names to IP addresses.

1. The browser caches these information for short period.
2. The operating system also caches these information.
3. At last, browser should make a request to a DNS resolver to retreive these information.

Secondly, the browser establishes a TCP connection with the web server using the IP address.

Finally, the browser sends HTTP requests to the server and receives the response from the server. Then, It renders the details on the browser. 

# Resource

- [What happens when you type a URL into your browser?](https://www.youtube.com/watch?v=AlkDbnbv7dk)
- [Top 6 Most Popular API Architecture Styles](https://www.youtube.com/watch?v=4vLxWqE94l4)