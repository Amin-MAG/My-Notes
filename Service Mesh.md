# Service Mesh

Service Mesh is a pattern or paradigm.

Service Mesh manages communication between Microservices. Each Microservice has its business logic and is deployed on a platform like the K8s cluster.

Migrating from Monolithic to Microservice challenges:

- Communication: Each of these services needs to communicate with some other Microservices.
- Security: Once an attacker reaches the cluster, It can do anything because there is no security restriction inside the cluster. (Although we have the firewall and some other restrictions for securing the cluster from the outside world)

Every single Microservice should have its:

- Own business logic
- Communication configurations
- Security logics
- Retry logic
- Metric & Tracing logic

Let’s just put all of these concerns into a sidecar proxy section except the business logic. The Control Plane of Service Mesh will inject these configurations into the proxy area.

## Traffic Splitting

Even though it passed the tests for new release versions, it’s better to dedicate a small amount of traffic to this new version to make sure it works. For example, first, release it for 10% of the users. (Canary Deployment)

With Service Mesh you can define WebServer Service, which can do traffic splitting.

# Istio

Istio is a service mesh implementation.

- Configuration Discovery
- Generate Certificates for TLS communication, Acts like Certificate Authority
- Gathers metrics, Handle tracing

## Architecture

### Istiod

The control plane is also called istiod.

### Envoy Proxy

The proxy section name is called envoy proxy. 

### Ingress Gateway

Actually acts like a load balancer and balance the traffic between the envoy proxies.

# References

[Istio & Service Mesh - simply explained in 15 mins](https://www.youtube.com/watch?v=16fgzklcF7Y&t=1s)