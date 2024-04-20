# System Design Concepts

## Vertical Scaling

When you have more users one way is to upgrade your RAM or CPU. This option is very limited and it is also a single point of failure. This method is called vertical scaling.

## Horizontal Scaling

When there are more users for a service, a better way is to deploy replicas of your service. Each replica handles a subset of users. By using horizontal scaling, we can increase our system's redundancy and fault tolerance. This method is called horizontal scaling.

## Load Balancer

How can we ensure that one server will not be overloaded when we have multiple replicas? If we use a reverse proxy to dispatch the request to the servers, we can be sure that none of them will be overloaded. There are multiple algorithms and mechanisms for load balancing like round-robin or hashing. You can read more about load balancers on the [Load Balancing](Load-Balancer.md) page.

## Content Deliver Network (CDN)

If our servers are located all around the world, we can use a technique to send each request to the closest server. The CDNs do not handle any kind of logic. They serve static files like images, videos, HTML, JS, and CSS files. CDNs receive the data from an origin server and copy it. It can be both pull-based or pushed-based. You can read more about CDNs on the [CDN](CDN.md) page.

## Caching

Caching is a big component in system design. A browser for example caches its data in an HDD disk. To be faster it can use the memory to cache it. It can be also in the CPU (L1, L2, and L3 cache). You can see more about caching on the [Caching](Caching.md) page.

## TCP/IP

## DNS

DNS is a component of the network which translates the domain names to a specific IP address. You can read more about load balancers on [this](Network.md##DNS) page.

## HTTP

TCP is too low level. HTTP protocol is an application layer protocol which is much easier to use for developers.

## API Paradigm

### REST

It is a standardazation around HTTP protocol that make it stateless and consistent.

### GraphQL

Instead of multiple request, By using Graphql, you can request a `query` and fetch the fields that you want to receive. You can recieve multiple resources by a single request. [Here](Golang/Go%20GraphQL.md) is an initial setup for GraphQL in Go language.

### gRPC

Other paradaigms uses JSON to request and respond, but gRPC uses protocol buffers which boosts the performance. The down side is that JSON is much more human readable than protocol buffers. [Here](Golang/gRPC.md) is an initial setup for GraphQL in Go language.

## Web Socket

For realtime applications like chat apps, you can use websocket which pushes and receives the data immediately.

## Databases

You can read more about database in the [database](Databases.md) page.

## Sharding

If we don't have to enforce any foreign key constraints (Consistancy from ACID), we can use sharding to have horizontal scaling. But sharding can get complicated. You can read more about sharding in the [sharding](Sharding.md) page.

## Replication

### Leader-Follower Replication

If we want to scale our database read we can create read-only copies of our database.

### Leader-Leader Replication

It can cause to inconsistant databases.

## CAP Theorem

It is stated in the theorem that we can choose 2 of three for our design. C for consistancy, A for availability, and P for Partion (Network). For example, for database partitioning we can either availability or consistancy.

### ELC

### PAC

## Message Queues

They are kinda like databases because they have durable storage and can be replicated or sharded. If our system receives more request than it can process it is good to use a message queue.

# Resources

- [20 System Design Concepts Explained in 10 Minutes](youtube.com/watch?v=i53Gi_K3o7I)