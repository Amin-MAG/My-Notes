# Backend Development

## API Architecture Styles

APIs or Application Programming Interfaces are the backbone of our interconnected digital world. They act as bridges, allowing distinct software components to communicate and interact. There several architectural style that can be used.

### SOAP

It's a veteran in the field, mature, comprehensive, and XML-based. SOAP is heavily used in financial service and payment gateways where security and reliability are key. 

IBM and Microsoft define a new way to define the APIs in 2007 which is called WSDL. WSDL contains all definitions about the API such as Elements, Operations, Messages, etc.. You can use SoapUI to see the API definition.

### RESTful 

They are like the Internet's backbone. popular, easy to implement, and use HTTP methods. If you need real-time data or operate with a highly connected data model, This might not be the right choice.

### GraphQL

It is not only a API architecture style, but also a query language. The client can specify the exact required fields by the query language.  This means no more over-fetching or under-fetching of data. It leads to more efficient network communication and faster responses.

### gRPC

It's modern, high-performance, and uses Protocol Buffers. It is a favorite choice for microservices architectures.

- [gRPC](gRPC.md)
- [gRPC in Golang](Golang/gRPC.md)

### WebSocket

WebSocket is all about real-time, bidirectional, and persistent connections. It is perfect for live chat applications and real-time gaming, where low-latency is a key.

### Webhook

It is all about event-driven, HTTP callbacks, and asynchronous operation. [Read more about Webhook](Webhook.md)

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

# Ways to improve the performance of API

The optimization should not be the first step in the procedure of improving the performance of the API. Optimization is powerful, but it can lead to complexity. Actually, the first step is to find bottlenecks using load testing and profiling.

## Caching

It is one of the most effective ways. We can store the result of an expensive computation and reuse it again. You can use `memcached` or `Redis`.

## Connection Pool

You can maintain a pool of connection instead of creating a new connection for each request. Creating a new connection to each time (For instance, a connection to the database) involves a lot of handshake protocols that can slow down your API.

In serverless, we need to create a new connection for each request. In this case, there are some solutions like AWS RDS proxy and Azure's SQL database serverless that handle this situation.

## N+1 Query Problem

Consider a scenario that you have an API for fetching blog posts and first comments of each post. If you are using more than 2 queries to fetch these data, you are slowing down your API, because you 1 request for fetching post and n requests for fetching comments of each post. 

```sql
-- Do not use 
select id, content from posts;
select author_name, comment_data from comments where id = 1;
select author_name, comment_data from comments where id = 2;
select author_name, comment_data from comments where id = 3;
select author_name, comment_data from comments where id = 4;
select author_name, comment_data from comments where id = 5;
select author_name, comment_data from comments where id = 6;
```

In this case, reducing the number of Round-trip to the database can make your API faster.

```sql
select posts.id as post_id, post.content, comments.author, comments.comment_data
from posts
left join comments on comments.post_id = posts.id;
```

## Use pagination

If your API is responsible for returning a huge amount of data, you can slow things down. Break and change the response of API to smaller pages. You can use `limit` and `offset` parameters to manage these pages.

## Lightweight JSON Serialization

When the API is returning JSON responses, consider evaluating the performance of the JSON serialization library that you use.

## Compression

By compressing request and response's payload, you can reduce the amount of data transferred over the network. You can see Brotli. Additionally, some cloud providers like cloudflare can handle this compression for you offloading this tasks from your server.

## Asynchronous Logging

By asynchronous sending logs to another service or saving them in files, you can enhance the performance of a service with a high throughput. In this case, there is a chance that you loose some of your logs when the service crashes. 


# Top Tips for API Security

## HTTPS

Use HTTPS for having encrypted connection between client and server. It prevents Eavesdropping, Man-in-the-Middle, and other security threats.

## OAuth2

It is a way for authenticating the user by a third-party software or account. Since we do not keep track of credentials of the user, it is more safer.

## WebAuthn or Web Authentication

It uses methods that make it much harder for attackers to perform phishing or credential stuffing. It usually uses Facial Recognition or Fingerprints for authenticating.

## API Keys

API Keys are a common way for authenticating and authorizing service to service communications.

Having a Single API key is not a good practice. Instead it is much better to use Leveled API Keys having various access levels.

## Authorization

Authenticating and Authorizing is different. Authorizing is about what actions each subject (for instance, each user) can perform on a specific resource. There are couple of different [Access Control Models](Access-Control-Models.md) that can be used for implementing the Authorization system.

## Rate Limiting

It is about controlling how many request each client can perform during a specific period of time. Rate Limiting help us protect our APIs from being overwhelmed by malicious actors.

Rate Limiting can be based on different factors such as IP address, user ID, or API Key. It is possible to have different rate limits on different types of resources.

## API Versioning

Implementing API Versioning is a best practice that allow us to evolve the API over time while maintaining backward compatibility.

## Allow Listing

It is a mechanism that explicitly allow access to a list of trusted IP Addresses, User IDs, or API Keys. Allow Listing follows the principle of deny all. 

## OWASP

Open Web Application Security Project provides valuable resources and guidelines for web application security. 

## [API-Gateway](API-Gateway.md)

API Gateway is an access entry point for clients that sits between clients and backend services. It provides a centralized layer for enforcing security policies, rate limiting, authentication, etc. They can also provide traffic management, caching, logging, monitoring, etc .

## Error Handling

Proper Error handling is crucial for API Security and User Experience. 

## Input Validation

Implement robust input validation for all data received from clients. It can prevent attacks such as SQL Injection, XSS (Cross-Site Scripting), etc. The input validation should be done in both client and server sides.

# Resource

- [What happens when you type a URL into your browser?](https://www.youtube.com/watch?v=AlkDbnbv7dk)
- [Top 6 Most Popular API Architecture Styles](https://www.youtube.com/watch?v=4vLxWqE94l4)
- [Top 7 Ways to 10x Your API Performance](https://www.youtube.com/watch?v=zvWKqUiovAM)