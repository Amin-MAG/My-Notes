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
- Consistancy: It ensures the data remains consistent and valid throughout the database.
- Security: It controls authentication through users and permissions.
- Reliability: It is easy to backup databases and roll them back.
- Structure Query Language: They have a language to simplifies user interaction.

## Relation Database Management Systems

They are usually apply the rule of ACID.

## Non-Relation Database Management Systems

This DMS drops the concept of consistancy from ACID and whole idea of relations.

# ACID 

## Atomicity 

All transactions are all or nothing.

## Consistancy 

It means foreign key and other constrains will always be enforced. Consistancy make databases harder to scale.

## Isolation

Different kind of transactions will not interfere with each other.

## Durability

They offer durability because the data is stored on the disk.

# How to choose database

> **Note**: If you want to change the current database to another type of database, You should be sure that there is not any other way or solution to fix the problem that you've faced.

Here are some casual trade-offs:

1. The eliminate or limit transactional guarantees like ACID.
2. The can limit data modeling flexibility.

After you chose database candidates, create a realistic test bench using your own data.
# Read more

- tuning the working of set memory size
- choosing compaction strategy
- change the garbage collection strategy
- Using cache to fix the problem?
- Add Read replicas
- partitioning or shardin (maybe the data is naturally siloed)
- I should read more about the benefits and limitation of each kind of database
- 

# Read more

- [Logging Databases](Logging.md#Databases)
- [SQL Injection](SQL-Injection.md)
- [How to choose the right database](https://www.youtube.com/watch?v=kkeFE6iRfMM)