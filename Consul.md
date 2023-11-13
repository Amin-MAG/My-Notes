# [Consul](https://github.com/hashicorp/consul)

Consul is an implementation of [Service Mesh](Service-Mesh.md). Consul is a distributed, highly available, and data center aware solution to connect and configure applications across dynamic, distributed infrastructure.

> Istio is another implementation of Service Mesh.

There is a sidecar agent [Envoy Proxy](https://github.com/envoyproxy/envoy) in each pod as an assistant to help the containers and applications to send to or receive requests from services.

## Advantages 

- **Dynamic Service Discovery**: 
- **Health and Fault Tolerance**: Consul has the information about healthiness and readiness of each pod. When a request is going to be sent, the platform makes sure to send the request to the healthy pods.
- **Secure Networking**: If you want to have Secure networking you have to change all code of your application. With Consul, all communication between services is going to be in TLS.
- **Authenticated Communication**: Each proxy in pods has its own certificate. When a request is received by the proxy, It can validate the senders key (asking the central registry) to see whether the request is real or fake.

# Resources

- [Consul Service Mesh Tutorial for Beginners Crash Course](https://youtube.com/watch?v=s3I1kKKfjtQ)