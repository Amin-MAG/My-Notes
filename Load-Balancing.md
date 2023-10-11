# Load Balancing

Load balancing is an inevitable component for a large scale application. Load Balancing helps ensure high availability, responsiveness, and scalability. 

# Load Balancing Algorithms

Knowing the Load Balancing Algorithms helps us to better architect, troubleshoot, and optimized our application. There two main type of load balancing algorithms; static and dynamic.
## Static Algorithms

It distributes the request without taking into account real-time server status and performance metrics. The main advantage is simplicity, but the downside it less adaptability and precision. **Static algorithms like Round Robin work well for stateless applications.**

### Round Robin

The client requests are sent to different service instances in sequential order. It can potentially overload servers. The services are usually required to be stateless.

### Sticky Round Robin

This is an improvement of the round-robin algorithm. If Alice’s first request goes to service A, the following requests also go to service A. The goal is to improve the performance by having related data on the same server. It can cause uneven loads on the servers.

### Weighted Round Robin

The admin can specify the weight for each service. The ones with a higher weight handle more requests than others. The downside it that an admin should manually configure the weights for each server.

### IP/URL Hash

This algorithm applies a hash function on the incoming requests’ IP or URL. The requests are routed to relevant instances based on the hash function result. If the hash function is selected wisely, it can distribute the requests evenly. Keep in mind that choosing an optimal hash function could be challenging.

## Dynamic

These algorithms adapt in real-time by taking servers conditions into account. **Dynamic Algorithms help optimize response times and availability for large and complex applications.** 

### Least Connection

A new request is sent to the service instance with the least concurrent connections. This requires to actively keep track number of connection with the target servers.

### Least Time

A new request is sent to the service instance with the fastest response time. This approach is highly adaptive and reactive, however requires constant monitoring that can introduce complexity.

# Resources

- [Top 6 Load Balancing Algorithms Every Developer Should Know](https://www.youtube.com/watch?v=dBmxNsS3BGE)
- [EP47: Common Load-balancing Algorithms](https://blog.bytebytego.com/p/ep47-common-load-balancing-algorithms)
