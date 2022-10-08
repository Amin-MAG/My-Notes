# Backend Development

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