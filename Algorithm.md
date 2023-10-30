# Algorithm


- [DA - Shortest path](Algorithm/DA%20-%20Short%20248bc.md)
- [Dynamic Programming](Algorithm/Dynamic%20Programming.md)
- [Graphs](Algorithm/Graphs.md)
- [NetworkX](Algorithm/NetworkX.md)

## Algorithms for System Design

## Consistent Hashing

It plays an important role in distributed systems. One example of this algorithm is Cassandra distributing data across multiple servers. In this algorithm, nodes are on a single rings. When you want to remove or add a node, it will be better than naive hashing, because just a range of that rings need remapping. 

### See more

- Rendezvous Hashing
- Jump Consistent Hashing

## Quad Trees

Quad Trees is very useful for map related platforms. It divides a 2 dimension space like a map to a sequence of number. At first, It split the map into 4 regions and continue splitting each one of them to 4 smaller regions. This algorithm enables fast location-based insertion and search. You can also points in a radius by traversing the tree.

### See more

- R-Tree
- KD-Tree

## Leaky Bucket

Leaky Bucket algorithm is for rate limiting. These kind of algorithms are simple algorithms with each making tradeoffs between accuracy and performance.

### See more

- Token Bucket
- Sliding Window Counter

## Tries

For better performance in operations like autocomplete, search engines or text editors, Tries can help. It is a tree optimized for storing strings and prefixes. The power of Tries is in fast lookup speed. Keep in mind that this kind of data structure is very greedy with memory.

## Bloom Filter

Bloom Filters shine in caching, deduplication, and analytics. It is a probabilistic data structure for set membership check. In this algorithm, we recognize firm no for set membership check, by hashing the item. You can be more accurate and better, but keep in mind that using more accurate one use more memory.

## Consensus Algorithms

In distributed systems, reaching consensus an ensuring that all nodes agree on a share state is tricky.

## Raft

Kafka and etcd use Raft algorithm in their replication, fail over, and leader election.

### See more

- Paxos
- Raft

# Resources

- [Algorithms You Should Know Before System Design Interviews](https://www.youtube.com/watch?v=xbgzl2maQUU)
