# Kafka

It’s a Distributed stream processing system written in Scala and Java.

Kafka combines three key capabilities so you can implement [your use cases](https://kafka.apache.org/powered-by) for event streaming end-to-end with a single battle-tested solution:

1. To **publish** (write) and **subscribe to** (read) streams of events, including continuous import/export of your data from other systems.
2. To **store** streams of events durably and reliably for as long as you want.
3. To **process** streams of events as they occur or retrospectively.

## Architecture

### Kafka server - Kafka broker

Producers and Consumers establish a TCP connection to the Kafka server or broker to subscribe or publish topics. 

Unlike RabitMq, the consumers pull topics from the broker.

### Partitions

Kafka uses the concept of sharding to distribute the database.

![Untitled](Kafka%206f548/Untitled.png)

### Consumer Groups

Do parallel processing on partitions.

# Using GO SDK

There are two libraries for communicating to Kafka in Go

- https://github.com/segmentio/kafka-go
- https://github.com/Shopify/sarama

## Kafka Go

# References

[Apache Kafka in 6 minutes](https://www.youtube.com/watch?v=Ch5VhJzaoaI)

[What is Kafka and How does it work?](https://www.youtube.com/watch?v=LN_HcJVbySw)

[Apache Kafka](https://kafka.apache.org/intro)