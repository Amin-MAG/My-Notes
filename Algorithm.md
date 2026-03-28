---
title: Algorithm
draft: true
tags: [computer-science, system-design, reference]
---
# Algorithm


- [DA - Shortest path](Algorithm/DA%20-%20Short%20248bc.md)
- [Dynamic Programming](Algorithm/Dynamic%20Programming.md)
- [Graphs](Algorithm/Graphs.md)
- [NetworkX](Algorithm/NetworkX.md)

## Algorithms for System Design

## Consistent Hashing

It plays an important role in distributed systems. One example of this algorithm is Cassandra distributing data across multiple servers. In this algorithm, nodes are placed on a single ring. When you want to remove or add a node, it performs better than naive hashing, because only a range of that ring needs remapping.

### See more

- Rendezvous Hashing
- Jump Consistent Hashing

## Quad Trees

Quad Trees are very useful for map-related platforms. They divide a 2-dimensional space like a map into a sequence of regions. First, the map is split into 4 regions and each one is further split into 4 smaller regions. This algorithm enables fast location-based insertion and search. You can also find points within a radius by traversing the tree.

### See more

- R-Tree
- KD-Tree

## Leaky Bucket

The Leaky Bucket algorithm is used for rate limiting. These kinds of algorithms are simple, with each making tradeoffs between accuracy and performance.

### See more

- Token Bucket
- Sliding Window Counter

## Tries

For better performance in operations like autocomplete, search engines or text editors, Tries can help. It is a tree optimized for storing strings and prefixes. The power of Tries is in fast lookup speed. Keep in mind that this kind of data structure is very greedy with memory.

## Bloom Filter

Bloom Filters shine in caching, deduplication, and analytics. It is a probabilistic data structure for set membership check. In this algorithm, we recognize firm no for set membership check, by hashing the item. You can be more accurate and better, but keep in mind that using more accurate one use more memory.

## Consensus Algorithms

In distributed systems, reaching consensus and ensuring that all nodes agree on a shared state is tricky.

## Raft

Kafka and etcd use Raft algorithm in their replication, fail over, and leader election.

### See more

- Paxos
- Raft

# Resources

- [Algorithms You Should Know Before System Design Interviews](https://www.youtube.com/watch?v=xbgzl2maQUU)

# See Also

- [Data Structures](DataStructure.md)
- [System Design Concepts](System-Design-Concepts.md)
- [Distributed Systems](Distributed-Systems.md)
