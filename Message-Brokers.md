# 💬 Message Brokers

Message brokers play a crucial role in distributed systems by facilitating communication and coordination between different components or services.

- [RabbitMQ](RabbitMQ.md)
- [Kafka](Kafka.md)
- [EMQX](EMQX.md)
- [KubeMQ](KubeMQ.md)

# Why to Use Message Brokers

## Asynchronous Communication

When you have services that don't need an immediate response from each other and can continue processing independently.

**Example:** In an e-commerce system, after a customer places an order, the order processing service can publish a message to a broker indicating that an order is ready for processing. Other services, such as inventory management or shipping, can then subscribe to these messages and process them at their own pace.

## Decoupling

When you want to decouple the components of your system, allowing them to evolve independently without directly depending on each other.

**Example:** In a microservices architecture, different microservices can communicate via a message broker. If a service needs to be updated or replaced, as long as it continues to produce and consume messages with the same format, it can be changed without affecting other services.

## Load Balancing

When you want to distribute the workload among multiple instances of a service or multiple services to improve scalability.

**Example:** A task queue managed by a message broker can be used to distribute processing tasks among multiple worker instances. Each worker subscribes to the queue and picks up a task to process, ensuring a balanced distribution of workload.

## Fault Tolerance

When you want to build a system that is resilient to failures and can recover from errors without losing messages or data.

**Example:** If a service producing messages goes down temporarily, the messages can be buffered in the message broker until the service is back online. Consumers can also be designed to handle intermittent failures gracefully.

## Event-Driven Architecture

When you want to build an event-driven system where components react to events and trigger actions based on those events.

**Example:** In a smart home system, various devices can publish events (e.g., motion detection, door opening) to a message broker. Other components, such as security systems or automation controllers, can subscribe to these events and respond accordingly.

## Interoperability

When you have services implemented in different languages or technologies, and you need a common communication layer.

**Example:** A message broker can serve as an intermediary between services written in Java and Python. Each service communicates with the message broker using a common messaging protocol, allowing for language-agnostic communication.

## Using  Message Queue vs Threads

Using message brokers versus direct communication between services using threads depends on various factors, and both approaches have their advantages and drawbacks.

### Using Message Brokers

- **Decoupling**: Introduce decoupling between the services. It just publishes an order event, and other services subscribe to those events. **Although it adds complexity to our system.**
- **Scalability**: Enables easy scalability, but requires additional resources to manage infrastructure.
- **Fault Tolerance**: Offers better fault tolerance, but also, **Introduces a single point of failure if the message broker is not designed for high availability.**
- **Order**: Message brokers often provide guarantees on message ordering and delivery, ensuring that messages are processed in the order they are received. **In the meanwhile, it increases the latency.**

### Using Threads

- **Decoupling**: It is more simple in implementation, but **tighter coupling between services. If one service fails, it might impact the others directly. Maintenance and updates to each service may require coordination.**
- **Scalability**: While you can still achieve parallelism with threads, **scaling may be more manual and require careful coordination to avoid bottlenecks.**
- **Fault Tolerance**: Fault tolerance may require additional mechanisms such as retries and error handling
- **Order**: Depending on the implementation, you might need to implement additional logic to ensure the ordering and reliability of messages.

