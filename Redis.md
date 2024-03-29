# Redis

A remote dictionary server is an In-Memory database. Often used to improve performance. It is also a fully-fledged primary database. You can extend the Redis core with the Redis modules. Because it is an In-Memory database, then it is super fast and performant. It also makes the running test much faster. 

## Persist Redis data

If you have many replicas of Redis, then it doesn't cause any problems. Always we should consider the worst case (What if all of our replicas fail), Then we need to persist data.

There are 2 options here:

1. Use snapshotting (RDB): This is faster, but you lose your latest data. (You can use `SAVE`, `BGSAVE`, or `DUMP` to create a new snapshot) 
2. Use Append only file (AOF): This is slower, but you restore the latest state.

Or a combination of these methods.

## Basic commands

To see all available keys

```bash
keys *
```

To set a new key-value data

```bash
set test_key test_value

# Set a value with a TTL (seconds)
set test_10s value_10s EX 10
```

To get a key

```bash
get test_key
```

To delete a key

```bash
del test_key
```

To get the size of the database

```bash
dbsize
```

### exists

To check whether a key exists or not

```bash
exists test_key
```

### flush

To flush the whole instance or a single database

```sql
-- The whole instance
FLUSHALL

-- A single database
FLUSHDB
```

### monitor

To monitor the instance you can use 

```sql
MONITOR
```

### save

To perform a background saving of Redis on disk. The snapshot file is usually named `dump.rdb` and is stored in the Redis data directory.

```bash
bgsave
```

To perform a synchronous blocking save of the Redis database to disk.

```bash
save
```

`DUMP` returns a binary serialized version of the value associated with the key

```bash
dump mykey
```

### restore

The `RESTORE` command in Redis is used to restore data from a backup created with the `DUMP` command or obtained from a snapshot file (RDB file).

```bash
RESTORE mykey 0 <serialized-value>
```

The 0 in the command represents the key's TTL (Time to Live) value. If you want to restore the key without any expiration, use 0; otherwise, you can specify the desired TTL in seconds.

### scan

# Data Types

## Hash Map

### hmset

### hgetall

### hget, hdel

## Set

### SADD

To add an item to the set

```bash
SADD usernames amin
```

### SISMEMBER

Check if it is a member of the set

```bash
SISMEMBER usernames amin
```

## Sorted Set

### ZADD

```bash
ZADD userlogins 10 myuserID
```

### ZSCORE

```bash
ZSCORE userlogins myuserID
```

### ZINCRBY 

```bash
# If it is not in the sorted set, it will add it
ZINCRBY userlogins 7 foor
# It will increase it, when it is present.
ZINCRBY userlogins 1 myuserID
```

### ZRANGE, ZREVRANGE

To sort the entities ascending or descending.

```bash
# Sort from the first one to last one
ZRANGE userlogins 0 -1
```

### ZCARD

To get the number of the entities.

```bash
ZCARD userlogins
```

### ZRANGEBYSCORE, ZREVRANGEBYSCORE

To filter some of the results from redis.

```bash
# Filter the userlogins that are more than 10
# Sort them in ascending order
ZRANGEBYSCORE userlogins 10 +inf

# You can use WITHSCORES to get the value alongside
# with the name
ZRANGEBYSCORE userlogins -inf 5 WITHSCORES
```

# Use cases

## Caching Objects

It stores frequently requested data in memory. It facilitates the webservers to respond very quickly to the most frequent requests. It will decrease the load on the database and increase the response time.

## Session Store

The web server uses Redis to track the user's session.

## Distributed Lock

Redis is used as a distributed lock when multiple instances need to coordinate access to some shared resource.

## Rate Limiter

It can count the number of requests of a user and help us in rate limiting some specific users.

## Gaming Leader board

It can be used for game leader boards.

# Redis vs Memcached

## Redis

1. **Data Persistence**: Supports data persistence, meaning it can store data to disk.
2. **Data Types**: Supports a wide variety of data types, including strings, hashes, lists, sets, and more. This makes Redis more versatile for use cases beyond simple caching.
3. **Complexity**: More feature-rich and complex compared to Memcached. Redis can be used as a cache, database, and message broker, among other things.
4. **Atomic Operations**: Supports atomic operations on various data types, making it suitable for scenarios where you need to perform complex operations atomically.
5. **Replication and High Availability**: Provides built-in support for master-slave replication, which allows for data redundancy and high availability.
6. **Community and Ecosystem**: Has a larger community and a broader ecosystem.
7. **Use Cases**: Suitable for use cases where data persistence, complex data structures, and advanced features are required. It can be used as a caching layer, database, and more.

## Memcached

1. **Data Persistence**: Does not support data persistence.
2. **Data Types**: Primarily designed for caching key-value pairs with a focus on simplicity. It has a limited set of data types compared to Redis.
3. **Complexity**: Simpler and focused specifically on caching. It is lightweight and easy to set up.
4. **Atomic Operations**: Primarily supports atomic operations on simple data types. Complex operations may need to be implemented in the application logic.
5. **Replication and High Availability**: Does not have built-in support for replication.
6. **Community and Ecosystem**: Has been around longer and has a mature ecosystem, but it is more specialized for caching.
7. **Use Cases**: Ideal for simple caching scenarios where speed is critical, and data persistence is not a requirement.

# Read More 

hll hyper log log 

# Resources

- [Top 5 Redis Use Cases](https://www.youtube.com/watch?v=a4yX7RUgTxI)