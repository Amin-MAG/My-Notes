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

The exchange must know exactly what to do with a message it receives.

There are a few exchange types available: direct, topic, headers, and fanout.

```go
err = ch.ExchangeDeclare(
  "logs",   // name
  "fanout", // type
  true,     // durable
  false,    // auto-deleted
  false,    // internal
  false,    // no-wait
  nil,      // arguments
)
```

The fanout exchange is very simple. As you can probably guess from the name, it just broadcasts all the messages it receives to all the queues it knows. And that's exactly what we need for our logger.

### Direct Exchange

The routing key should be exactly as same as the direct binding key.

```go
err = ch.QueueBind(
	q.Name,        // queue name
	"info",             // routing key
	"logs_direct", // exchange
	false,
	nil)
helper.FailOnError(err, "Failed to bind a queue")
```

### Topic Exchange

Messages sent to a topic exchange can't have an arbitrary `routing_key` - it must be a list of words, delimited by dots.

- `*` Star - can substitute for exactly one word.
- `#` Hash - can substitute for zero or more words.

```go
err = ch.QueueBind(
	q.Name,       // queue name
	"kernel.#",            // routing key
	"logs_topic", // exchange
	false,
	nil)
helper.FailOnError(err, "Failed to bind a queue")
```

### Binding

That relationship between exchange and a queue is called *binding*.

```go
err = ch.QueueBind(
  q.Name, // queue name
  "",     // routing key
  "logs", // exchange
  false,
  nil,
)
```

Bindings can take an extra routing_key parameter. To avoid confusion with a `Channel.Publish` parameter we're going to call it a binding key. This is how we could create a binding with a key

```go
err = ch.QueueBind(
  q.Name,    // queue name
  "black",   // routing key
  "logs",    // exchange
  false,
  nil)
```

The meaning of a binding key depends on the exchange type. The `fanout` exchanges, which we used previously, simply ignored their value.

To extend the logs application example, We need to filter some of these logs to be published for some consumers.

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

- Durable and Non-Auto-Deleted queues will survive server restarts and remain when there are no remaining consumers or bindings.  Persistent publishings will be restored in this queue on server restart.  These queues are only able to be bound to durable exchanges.
- Non-Durable and Auto-Deleted queues will not be redeclared on server restart and will be deleted by the server after a short time when the last consumer is canceled or the last consumer's channel is closed. Queues with this life-time can also be deleted normally with QueueDelete.  These durable queues can only be bound to non-durable exchanges.
- Non-Durable and Non-Auto-Deleted queues will remain declared as long as the server is running regardless of how many consumers.  This lifetime is useful for temporary topologies that may have long delays between consumer activity. These queues can only be bound to non-durable exchanges.
- Exclusive queues are only accessible by the connection that declares them and will be deleted when the connection closes.
- When noWait is true, the queue will assume to be declared on the server.

> Empty string for the name indicates that we want to generate a random name for our queue. when the connection closes, the queue will be deleted because it is declared as exclusive.
> 

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

## Dispatching

RabbitMQ just dispatches a message when the message enters the queue. It doesn't look at the number of unacknowledged messages for a consumer.

This tells RabbitMQ not to give more than one message to a worker at a time.

```go
err = ch.Qos(
  1,     // prefetch count
  0,     // prefetch size
  false, // global
)
failOnError(err, "Failed to set QoS")
```

## RPC

We need to run a function on a remote computer and wait for the result. Well, that's a different story. This pattern is commonly known as *Remote Procedure Call* or *RPC*.

A client sends a request message and a server replies with a response message. In order to receive a response, we need to send a 'callback' queue address with the request.

To publish an RPC event or task

```go
err = ch.Publish(
  "",          // exchange
  "rpc_queue", // routing key
  false,       // mandatory
  false,       // immediate
  amqp.Publishing{
    ContentType:   "text/plain",
    CorrelationId: corrId,
    ReplyTo:       q.Name,
    Body:          []byte(strconv.Itoa(n)),
})
```

- `reply_to`: Commonly used to name a callback queue.
- `correlation_id`: Useful to correlate RPC responses with requests.

### Overview of RPC

- When the Client starts up, it creates an anonymous exclusive callback queue.
- For an RPC request, the Client sends a message with two properties: `reply_to`, which is set to the callback queue, and `correlation_id`, which is set to a unique value for every request.
- The request is sent to a `rpc_queue`.
- The RPC worker (aka server) is waiting for requests on that queue. When a request appears, it does the job and sends a message with the result back to the Client, using the queue from the `reply_to` field.
- The client waits for data on the callback queue. When a message appears, it checks the `correlation_id` property. If it matches the value from the request it returns the response to the application.

# Commands

To list the components of RabbitMQ

```bash
rabbitmqctl list_queues
rabbitmqctl list_exchanges
rabbitmqctl list_bindings
```

To delete a queue

```bash
rabbitmqctl delete_queue task_queue
```

To enable the management ui

```bash
rabbitmq-plugins enable rabbitmq_management # http://localhost:15672/#/
```

# Examples

Check out [this repository](https://github.com/Amin-MAG/RabbitMQ) (Actually inspired by https://github.com/rabbitmq/rabbitmq-tutorials)

# References

[RabbitMQ tutorial - "Hello World!" - RabbitMQ](https://www.rabbitmq.com/tutorials/tutorial-one-go.html)

[https://github.com/rabbitmq/rabbitmq-tutorials](https://github.com/rabbitmq/rabbitmq-tutorials)