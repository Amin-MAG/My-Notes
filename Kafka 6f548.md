# Kafka

It’s a Distributed stream processing system written in Scala and Java.

## Architecture

### Kafka server - Kafka broker

Producers and Consumers establish a TCP connection to the Kafka server or broker to subscribe or publish topics. 

Unlike RabitMq, the consumers pull topics from the broker.

### Partitions

Kafka uses the concept of sharding to distribute the database.

![Untitled](Kafka%206f548/Untitled.png)

### Consumer Groups

Do parellel processing on partitions.

# References

[Apache Kafka in 6 minutes](https://www.youtube.com/watch?v=Ch5VhJzaoaI)

[What is Kafka and How does it work?](https://www.youtube.com/watch?v=LN_HcJVbySw)