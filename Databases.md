# 💾 Databases

## Key-Value

- [Redis](Redis.md)
- [etcd](etcd.md)

## Wide-Column

- Cassandra
- Hbase

## Document

- [MongoDB](MongoDB.md)
- Fire Store

## Relational

- [PostgreSQL](PostgreSQL.md)
- [SQLite3](SQLite3.md)
- [MySQL](MySQL.md)

## Time-Series

- [Prometheus](Prometheus.md)
- [InfluxDB](InfluxDB.md)

## Graph

- Neo4j
- DGraph
- [ENT](ENT.md)

## Search

- [ElasticSearch](ElasticSearch.md)
- Solr
- MEILISearch

## Vector

- Milvus


# Database Management Systems

Some essential features of a DBMS System:

- Concurrency: It makes sure that these concurrent interactions succeed without corrupting or loosing any data.
- Consistency: It ensures the data remains consistent and valid throughout the database.
- Security: It controls authentication through users and permissions.
- Reliability: It is easy to backup databases and roll them back.
- Structure Query Language: They have a language to simplifies user interaction.

## Relation Database Management Systems

They are usually apply the rule of ACID.

## Non-Relation Database Management Systems

This DMS drops the concept of consistency from ACID and whole idea of relations.

# ACID 

## Atomicity 

All transactions are all or nothing.

## Consistency 

It means foreign key and other constrains will always be enforced. Consistency make databases harder to scale.

## Isolation

Different kind of transactions will not interfere with each other.

## Durability

They offer durability because the data is stored on the disk.

# How to choose database

> **Note**: If you want to change the current database to another type of database, You should be sure that there is not any other way or solution to fix the problem that you've faced.

First let's take a look at some steps you should do before replacing the legacy database by another one.

- **Memory Configuration**: Properly configuring memory settings is crucial because it directly impacts the database's ability to efficiently store and retrieve data.
- **Compaction Strategy**: Compaction is the process of reclaiming space and organizing data to reduce storage requirements and improve access efficiency.
- **Garbage Collector Strategy**: Garbage collection is a process that automatically identifies and frees up memory occupied by objects that are no longer in use by the application.
- **Cache**: Consider the case that you can use caching to enhance the performance of the database.
- **Read Replicas**: Consider adding the read replicas to your database system so that it can reduce the amount of pressure on it.
- **Partitioning/Sharding**: Use sharding method can increase the complexity of your system, but in some cases it can be good solution. 

Here are some casual trade-offs:

1. The eliminate or limit transactional guarantees like ACID.
2. The can limit data modeling flexibility.

After you chose database candidates, create a realistic test bench using your own data.


## Benefits of Indexing:

1. **Improved Query Performance**
2. **Faster Sorting and Grouping**
3. **Enhanced Joins**
4. **Unique Constraints**
5. **Covering Indexes**

## When to Use Indexing:

- Use indexing when dealing with large datasets and frequently queried columns.
- Consider indexing columns involved in WHERE clauses, JOIN conditions, and ORDER BY clauses.
- Be cautious not to over-index, as it can impact the performance of insert, update, and delete operations.

# Read more

- [Logging Databases](Logging.md#Databases)
- [SQL Injection](SQL-Injection.md)
- [How to choose the right database](https://www.youtube.com/watch?v=kkeFE6iRfMM)