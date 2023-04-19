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

Some Esstential features of a DBMS System:

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

# Read more

- [Logging Databases](Logging.md#Databases)
- [SQL Injection](SQL-Injection.md)