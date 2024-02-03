# HTTP/HTTPS

HTTP is the foundation of data communication for the World Wide Web.

- HTTP is an application layer protocol and It is TCP/IP-based protocol.
- HTTP allows the web-applications to communicate and exchange data.
- It is Stateless and connection-less.

## Stateless Protocol

HTTP is a stateless protocol. Each request from a client to a server is independent and contains all the information needed for the server to fulfill that request.

## HTTPS

HTTPS uses SSL/TLS protocols to encrypt data transmitted between the client and the server, ensuring secure communication.

## URL Structure

Components of a URL (Uniform Resource Locator): scheme, host, port, path, query parameters, and fragment.

## HTTP Methods

- **GET:** Retrieve data from the server.
- **POST:** Submit data to be processed to a specified resource.
- **PUT:** Update a resource on the server.
- **DELETE:** Remove a resource on the server.
- **HEAD:** Retrieve only the headers of a resource.
- **OPTIONS:** Describe communication options for the resource.

## HTTP Headers

- **Request Headers:** Sent by the client to provide additional information about the request.
- **Response Headers:** Sent by the server to provide additional information about the response.

## Status Codes

Status Codes are Three-digit codes sent by the server in response to a client's request. For example, 200 OK, 404 Not Found, 500 Internal Server Error.

## HTTP Requests

The HTTP request looks like

```
METHOD URI Version

Headers

Body
```

For instance, 

```
GET /products/index.html HTTP/1.0

Host: www.mysebsite.com
Accept: text/html
Accept-language: en-us
...
```

## HTTP Response

The HTTP response looks like

```
Version StatusCode

Headers

Data
```

For example,

```
HTTP/1.0 200:OK

Host: www.mysebsite.com
Accept: text/html
Accept-language: en-us

<The content of HTML File>
```

## Cookies and Sessions

Cookies are small pieces of data stored on the client's side, providing state management for HTTP transactions.

## Caching 

Mechanisms like ETags and cache-control headers to control caching behavior.

## Redirection

Status codes like 301 (Moved Permanently) and 302 (Found) for URL redirection.

## Authentication

- Basic Authentication and Digest Authentication mechanisms.
- OAuth and JWT for modern authentication.

## Cross-Origin Resource Sharing (CORS)

CORS is a security feature implemented by web browsers to control resource access from different origins.

## Content Types

MIME types specifying the type of data being sent (e.g., text/html, application/json).

## WebSockets

A protocol providing full-duplex communication channels over a single TCP connection.

## Server-Sent Events (SSE)

A standard allowing servers to push data to web clients over HTTP.

## HTTP/2 and HTTP/3

Protocols designed to improve the performance of HTTP by optimizing the way data is exchanged between the client and the server.

## Security Best Practices

- Avoiding mixed content issues.
- Implementing secure password practices.
- Regularly updating SSL/TLS certificates.

## Web Application Firewalls (WAF)

Protective barrier between a web application and the internet, filtering and monitoring HTTP traffic.

## Load Balancing

Distributing incoming network traffic across multiple servers to ensure no single server bears too much demand.

## Web Performance Optimization

Techniques like minification, compression, and asset caching to improve website performance.

## HTTP/HTTPS Libraries and Frameworks

Familiarity with libraries (e.g., requests in Python) and frameworks (e.g., Express.js in Node.js) for working with HTTP/HTTPS.

## Security Headers

Implementing headers like Content Security Policy (CSP) and Strict-Transport-Security (HSTS) for enhanced security.

## SSL/TLS Handshake

Understanding the process of establishing a secure connection between the client and the server.

## Web Security Vulnerabilities

Awareness of common vulnerabilities like Cross-Site Scripting (XSS), Cross-Site Request Forgery (CSRF), and SQL Injection.
