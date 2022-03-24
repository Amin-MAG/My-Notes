# RabbitMQ

RabbitMQ is open-source message broker software: it accepts and forwards messages. You can think about it as a post office.

- *Producing* means nothing more than sending. A program that sends messages is a *producer.*
- The q*ueue* is the name for a post box that lives inside RabbitMQ.  A *queue* is only bound by the host's memory & disk limits, it's essentially a large message buffer.
- *Consuming* has a similar meaning to receiving. A *consumer* is a program that mostly waits to receive messages.

## Installation

### Docker

```yaml
version: '3'
services:

  rabbitmq:
    image: rabbitmq:3.9.13
    container_name: rabbitmq_test
    ports:
      - "5672:5672"
      - "15672:15672"
```

## Exchanges

Exchanges are message routing agents, defined by the virtual host within RabbitMQ. An exchange is responsible for routing the messages to different queues with the help of header attributes, bindings, and routing keys.

To go further: [https://www.cloudamqp.com/blog/part4-rabbitmq-for-beginners-exchanges-routing-keys-bindings.html](https://www.cloudamqp.com/blog/part4-rabbitmq-for-beginners-exchanges-routing-keys-bindings.html#:~:text=Exchanges%20are%20message%20routing%20agents,a%20queue%20to%20an%20exchange)

## Queue

To send or receive events, we must declare a queue.

```go
// Declare a Queue
q, err := ch.QueueDeclare(
	"hello", // name
	false,   // durable
	false,   // delete when unused
	false,   // exclusive
	false,   // no-wait
	nil,     // arguments
)
helper.FailOnError(err, "Failed to declare a queue")
```

Queue arguments & options

- Durable and Non-Auto-Deleted queues will survive server restarts and remain
when there are no remaining consumers or bindings.  Persistent publishings will
be restored in this queue on server restart.  These queues are only able to be
bound to durable exchanges.
- Non-Durable and Auto-Deleted queues will not be redeclared on server restart
and will be deleted by the server after a short time when the last consumer is
canceled or the last consumer's channel is closed.  Queues with this life-time
can also be deleted normally with QueueDelete.  These durable queues can only
be bound to non-durable exchanges.
- Non-Durable and Non-Auto-Deleted queues will remain declared as long as the
server is running regardless of how many consumers.  This lifetime is useful
for temporary topologies that may have long delays between consumer activity.
These queues can only be bound to non-durable exchanges.
- Exclusive queues are only accessible by the connection that declares them and
will be deleted when the connection closes.
- When noWait is true, the queue will assume to be declared on the server.

## Publishing

To publish events to the queue.

```go
// Publish a new event to the queue
body := "Hello World!"
err = ch.Publish(
	"",     // exchange
	q.Name, // routing key
	false,  // mandatory
	false,  // immediate
	amqp.Publishing{
		ContentType: "text/plain",
		Body:        []byte(body),
	},
)
```

Publishing arguments & options

- Publishings can be undeliverable when the mandatory flag is true and no queue is bound that matches the routing key
- when the immediate flag is true and no consumer on the matched queue is ready to accept the delivery.

> Transient messages will not be restored to durable queues, persistent messages will be restored to durable queues and lost on non-durable queues during server restart.
> 

## Consuming

To consume from the queue.

```go
// Consume the channel
messages, err := ch.Consume(
	q.Name,  // queue
	"hello", // consumer
	true,    // auto-ack
	false,   // exclusive
	false,   // no-local
	false,   // no-wait
	nil,     // args
)
helper.FailOnError(err, "Failed to register a consumer")

// Start consuming
go func() {
	for d := range messages {
		log.Printf("Received a message: %+v, string body: %s", d, string(d.Body))
	}
}()
```

Consuming arguments & options

- The consumer is identified by a string that is unique and scoped for all
consumers on this channel.
- When `autoAck` (also known as `noAck`) is true, the server will acknowledge deliveries to this consumer prior to writing the delivery to the network.
- When exclusive is true, the server will ensure that this is the sole consumer from this queue.
- When noWait is true, do not wait for the server to confirm the request and immediately begin deliveries.

# Commands

To list all queues

```bash
rabbitmqctl list_queues
```

# Examples

Check out [this repository](https://github.com/Amin-MAG/RabbitMQ) (Actually inspired by https://github.com/rabbitmq/rabbitmq-tutorials)

# References

[RabbitMQ tutorial - "Hello World!" - RabbitMQ](https://www.rabbitmq.com/tutorials/tutorial-one-go.html)

[https://github.com/rabbitmq/rabbitmq-tutorials](https://github.com/rabbitmq/rabbitmq-tutorials)