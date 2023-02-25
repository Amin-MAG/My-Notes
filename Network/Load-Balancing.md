# Load Balancing Algorithms

## Round Robin

The client requests are sent to different service instances in sequential order. The services are usually required to be stateless.

## Sticky Round Robin

This is an improvement of the round-robin algorithm. If Alice’s first request goes to service A, the following requests also go to service A.

## Weighted Round Robin

The admin can specify the weight for each service. The ones with a higher weight handle more requests than others.

## IP/URL Hash

This algorithm applies a hash function on the incoming requests’ IP or URL. The requests are routed to relevant instances based on the hash function result.

## Least Connection

A new request is sent to the service instance with the least concurrent connections.

## Least Time

A new request is sent to the service instance with the fastest response time.

# Resources

[EP47: Common Load-balancing Algorithms](https://blog.bytebytego.com/p/ep47-common-load-balancing-algorithms)
