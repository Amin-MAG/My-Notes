# CDN - Content Delivery Network

It was developed to deliver static HTML content for users all around the world. It has evolved since 1998. The main duty is to bring content closer to the user. CDN deploys servers at hundreds of locations worldwide, known as PoP, Point of Presence, or edge servers.

Each edge server acts as a [Reverse Proxy]() with a huge content cache. If a piece of content is in the cache, it could be quickly returned to the user. This can effectively reduce the amount of bandwidth and load.

## Type of CDN technologies

### DNS-based routing

PoPs have different IP addresses. When the user looks up the CDN IP address the DNS returns the closest PoP.

### Anycast

PoPs have the same IP address. When the user sends a request the Network chooses the PoP which is closer to the requester.

## Converting static content

A modern CDN could also transform static content into more optimized formats. For example, It minifies javascript bundles or converts images to modern formats like WEBP or AVIF.

## SSL Encryption

All TLS connections terminate at the edge server. TLS handshakes are expensive. It significantly reduces the latency for the user to establish an encrypted TCP connection.

## Security 

All modern CDNs have huge network capacity at the edge which is the key to providing effective DDoS protection against large scale attacks.

## Availability

It increases the availability of the services because of its distribution.

# Resources

- [What Is A CDN? How Does It Work?](https://www.youtube.com/watch?v=RI9np1LWzqw)