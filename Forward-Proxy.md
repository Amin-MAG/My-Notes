# Forward Proxy

A forward proxy, often called a proxy, proxy server, or web proxy is a server that sits in front of a group of client machines. When those computers make requests to sites and services on the Internet, the proxy server intercepts those requests and then communicates with web servers on behalf of those clients, like a middleman.

![](Attachments/Forward-Proxy/image-20240416210918917.svg)

## Why use a Forward Proxy?

### Protecting the client's identity

IP addresses of users are hidden from the server. It is complex to trace back the identity of the user because the server is just interacting with forward proxy. Only the IP address of the proxy server will be visible.

### Bypassing browsing restriction

Some governments, schools, and other organizations use firewalls to give their users access to a limited version of the Internet. A forward proxy can be used to get around these restrictions, as they let the user connect to the proxy rather than directly to the sites they are visiting.

### Blocking access to certain content

Conversely, proxies can also be set up to block a group of users from accessing certain sites. For example, a school network might be configured to connect to the web through a proxy which enables content filtering rules, refusing to forward responses from Facebook and other social media sites.