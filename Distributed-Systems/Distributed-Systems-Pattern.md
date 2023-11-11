# Distributed Systems Patterns

## Ambassador

![Untitle](Distributed-Systems/ambassador.png)

It acts like go-between for your apps and the services it communicates with (offloading tasks like logging, monitoring, or handling retries). For example, Kubenetes uses Ambassador design pattern to simplify the communication between services. It reduces the latency, enhances the security, and imporves the overall architecture.

## Circuit Breaker

This architecture prevents cascading failures. When a service become unavailable, the circuit breaker stops requests, allowing it to recover.

## CQRS (Command Query Responsibility Segregation)

By sparating the command or write operations from query or read operations, you can scale and optimized each independently. This kind of architecture can be considered for applications that have different performance characteristics, with different latency or resource requirements. 

## Event-Sourcing

Instead of updating a record directly, it is possible to store events representing the changes. This architecture provides a full history of changes. This enables better logging and auditing. Git version controller is a good example of this architecture.

## Leader Election

In a distributed system, the leader election pattern ensures only one node is responsible for a specific task or resource. When a leader fails, the remaining nodes elect a new leader. Zookeeper and etcd use this pattern. With this pattern we can ensure consistent decision making and avoid conflicts.

## PubSub

In this architecture, communication is based on publishing and consuming messages. This pattern enables better scalability. It is also useful in context that we want to propagate an update across a bunch of subscribers.

## Sharding

It is a technique for distributing data across multiple nodes in system. It improves performance and scalability. Each one of these shards include a subset of whole data that reduces load on any single node. Databases like MongoDB or Cassandra use sharding.

## Strangler Fig

It is a method for gradually replacing a legacy service with new implementations. It is usefull for system migrations.

# Resources 

- [Top 7 Most-Used Distributed System Patterns](https://www.youtube.com/watch?v=nH4qjmP2KEE&t=10s)