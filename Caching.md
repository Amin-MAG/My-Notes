---
title: Caching
draft: true
tags: [system-design, backend, reference]
---
# Caching

## CPU

### L1, L2 and L3 cache

### Translation Lookaside Buffer

## Operating System

### Page Cache

### File System Cache like INode

## System Architecture

### Client-Side Cache 

Browsers, for example, cache the response of the server in themselves.

### CDN

The CDN first looks at its cache to see whether the request is available or not. It will send the request to the origin if the request does not exist in the cache.

### Load Balancer

Some kinds of load balancers cache the response of requests, which leads to less load on the origin server and less response time for users.

### Databases

Relational, Non-Relational, and any databases, in general, have a mechanism for caching.

# See More

- [Tips on Caching](https://www.youtube.com/watch?v=wh98s0XhMmQ)

# See Also

- [Redis](Redis.md)
- [CDN](CDN.md)
- [System Design Concepts](System-Design-Concepts.md)