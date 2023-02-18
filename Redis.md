# Redis

A remote dictionary server is an In-Memory database. Often used to improve performance. It is also a fully-fledged primary database. You can extend the Redis core with the Redis modules. Because it is an In-Memory database, then it is super fast and performant. It also makes the running test much faster. 

## Persist Redis data

If you have many replicas of Redis, then it doesn't cause any problems. Always we should consider the worst case (What if all of our replicas fail), Then we need to persist data.

There are 2 options here:

1. Use snapshotting (RDB): This is faster, but you lose your latest data.
2. Use Append only file (AOF): This is slower, but you restore the latest state.

Or a combination of these methods.

## Basic commands

To flush the whole instance or a single database

```sql
-- The whole instance
FLUSHALL

-- A single database
FLUSHDB
```

To monitor the instance you can use 

```sql
MONITOR
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

# Resources

- [Top 5 Redis Use Cases](https://www.youtube.com/watch?v=a4yX7RUgTxI)