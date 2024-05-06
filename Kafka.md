# Kafka

It’s a Distributed Steaming platform for building data pipelines and streaming application at massive scale. It is written in Scala and Java.

Event streams are organized into topics that are distributed across multiple servers called brokers. This ensure that data is easily accessible and resilient to system crashes.

Kafka combines three key capabilities so you can implement your use cases for event streaming end-to-end with a single battle-tested solution:

1. To **publish** (write) and **subscribe to** (read) streams of events, including continuous import/export of your data from other systems.
2. To **store** streams of events durably and reliably for as long as you want.
3. To **process** streams of events as they occur or retrospectively.

Kafka has its own limitations. It is quite complicated and it has steep learning curve. It not suitable for startups and ultra low latency products like trading platforms.

# Kafka Architecture

### Kafka server - Kafka broker

Producers and Consumers establish a TCP connection to the Kafka server or broker to subscribe or publish topics. 

Unlike RabitMQ, the consumers pull topics from the broker.

### Partitions

Kafka uses the concept of sharding to distribute the database.

![Untitled](Kafka/Untitled.png)

### Consumer Groups

Do parallel processing on partitions.

### Using GO SDK

There are two libraries for communicating to Kafka in Go

- https://github.com/segmentio/kafka-go
- https://github.com/Shopify/sarama

### Kafka Go


# Kafka Usecases

## Message Queue

It serves as a highly reliable and scalable message queue.

## Data streaming for Recommendation (Activity Tracking) 

Kafka is ideal for ingesting and storing real-time events like clicks, views, and purchases from high traffic websites.

It processes user click streams and aggregates data lake for deep analysis. [Flink](Flink.md) can be used with Kafka to perform real-time analytics and machine learning on the streaming data.

## Change Data Capture (CDC)

Imagine the challenge of keeping data in sync across multiple databases and applications. Kafka comes to the rescue by capturing changes made to source databases, such as transaction logs, in real time.

With the help of [ElasticSearch](ElasticSearch.md) and [Redis](Redis.md), Kafka propagates these changes to target system reliably.

CDC ensures **Data Consistency** and **Data Synchronization**. It reduces **Discrepancies** and enables **Real-time Data Integration**.

## System Migration

Upgrading or Migrating systems can be a daunting task. Kafka acts as a data bridge between the old and new versions of the services during system migration.

Kafka ensures data consistency and availability throughout the migration process by replicating data between the old and new version of each service. This allows for a seamless transition without any data loss or disruption to the overall system.

## Gathering Data

Kafka can consolidate disparate streams into unified real-time pipelines for analytics and storage.

## Microservice Architecture

Kafka can be used in multiple patterns in microservice architectures. It can serves as a real-time data bus that allows different services to communicate to each other.

## Monitoring, Alerting, and Obervability

Kafka integrated with ELK stack can collect metrics, logs, and network data in real-time that can be aggregated and analyze to monitor the overall system health and performance. [Flink](Flink.md) Can be used to analyze real-time data for alerting.
 
## Log Processing and Analysis

Kafka's ability to handle massive volumes of log data makes it an ideal choice for this use case. Kafka integrates seamlessly with the ELK Stack providing powerful log analysis and visualization capabilities.

## Big Data

 Since it use a distributed architecture it can handle massive volumes of real-time data streams. 

# References

- [Apache Kafka in 6 minutes](https://www.youtube.com/watch?v=Ch5VhJzaoaI)
- [What is Kafka and How does it work?](https://www.youtube.com/watch?v=LN_HcJVbySw)
- [Apache Kafka](https://kafka.apache.org/intro)
- [System Design: Apache Kafka In 3 Minutes](https://www.youtube.com/watch?v=HZklgPkboro)