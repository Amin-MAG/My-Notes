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

## NoSQL vs SQL Databases

- **SQL:** MySQL, PostgreSQL, Oracle, SQL Server.
- **NoSQL:** MongoDB (document-oriented), Cassandra (column-family), Redis (key-value), Neo4j (graph).

### SQL

1. **Data Model**: Data is structured in tables with predefined schemas
2. **Schema**: Requires a predefined schema where the structure of the data is established before inserting data.
3. **Scalability**: Typically scales vertically by adding more powerful hardwares. Scaling horizontally can be challenging.
4. **Transactions**: Supports ACID.
5. **Query Language**: Uses a standardized query language (SQL) for defining and manipulating the data.
6. **Use Cases**: Well-suited for applications with complex queries and structured, relational data, such as financial systems, ERP (Enterprise Resource Planning), and transactional systems.

### NoSQL

1. **Data Model**: It supports various data models including document-oriented, key-values, column-family, and graphs.
2. **Schema**: Data can be inserted without a predefined schema, allowing for dynamic and evolving data structures.
3. **Scalability**: Designed to scale horizontally, allowing for distributed and parallel processing across nodes. Good for large amount of data and traffic.
4. **Transactions**: May sacrifice some aspects of ACID properties for performance and scalability. NoSQL databases often provide eventual consistency, where data consistency is guaranteed over time rather than immediately.
5. **Query Language**: No standardized query language across all NoSQL databases. Each type of NoSQL database may have its own query language or API.
6. **Use Cases**: Ideal for scenarios with rapidly changing data, unstructured or semi-structured data, and high scalability requirements, such as content management systems, real-time big data applications, and mobile app backends.

## Why to use Time-series Databases

1. **Efficient Time-Ordered Data Storage**: Time series databases are specifically optimized for storing and retrieving time-ordered data. They employ efficient data structures and storage mechanisms that excel at managing sequences of timestamped records, making them more suitable for time-series data than generic databases.
2. **High Write Throughput**: Time series data is often generated at a high frequency, especially in applications like IoT, monitoring systems, and financial markets. TSDBs are designed to handle high write throughput efficiently, ensuring that incoming data can be ingested and processed in real-time without sacrificing performance.
3. **Compression Techniques**: Many time series databases use specialized compression techniques to minimize storage space requirements for large volumes of timestamped data.
4. **Time-Based Queries and Aggregation and Time-Series Features**: TSDBs provide built-in functionalities for handling time-based queries and aggregations. This includes features like downsampling, filtering based on time intervals, and easily retrieving data within specific time ranges. TSDBs typically include features specifically designed for time-series use cases, such as continuous queries, moving averages, and statistical aggregations.
5. **Retention Policies**: Time series databases often include features for defining retention policies. These policies allow users to specify how long data should be retained before it is automatically purged, helping manage storage resources effectively and ensuring that only relevant historical data is kept.
6. **Specialized Query Language or APIs**
7. **Scalability for Time-Series Workloads**: TSDBs are designed to scale efficiently to handle growing volumes of time-series data.
8. **Handling Irregular Sampling Intervals**: Time series data often comes with irregular sampling intervals. TSDBs are adept at handling such irregularities, making them more flexible and accommodating for real-world scenarios where data may not be uniformly collected at regular intervals.

# Indexing

Indexing in databases is a technique used to improve the speed of data retrieval operations on a database table. Indexes provide a quick way to look up data based on the values in one or more columns.

## Types

- **Single-Column Index:** An index on a single column.
- **Composite Index:** An index on multiple columns.
- **Unique Index:** Ensures that the indexed columns do not contain duplicate values.
- **Clustered Index:** Determines the physical order of data rows in a table.
- **Non-clustered Index:** Keeps a separate structure to store the index data, pointing back to the actual data rows.

> **Note**: When you define a column as `UNIQUE`, the database system often automatically creates an index to enforce the uniqueness constraint efficiently. This index helps the database system quickly check if a new value being inserted or updated violates the uniqueness rule. However, not all indexes imply uniqueness. You can create indexes on columns without enforcing uniqueness. Multiple rows may have the same indexed value in such cases.

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