# Redis

Remote dictionary server is a In-Memory database. Often used to improve performance. It is also a fully-fledged primary database.

You can extend the Redis core with the Redis modules.

Because it is In-Memory database, then it is super fast and performant. It also make the running test much faster. 

## Persist Redis data

If you have many replicas of Redis, then it doesn't cause any problem. Always we should consider the worst case (What if all of our replicas fails), Then we need to persist data.

There are 2 options here:

1. Use snapshotting (RDB): This is faster, but you loose your latest data.
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