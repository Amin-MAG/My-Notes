# Proxy

- [Forward-Proxy](Forward-Proxy.md)
- [Reverse-Proxy](Reverse-Proxy.md)
- [Load-Balancer](Load-Balancer.md)
- [API-Gateway](API-Gateway.md)

## Forward Proxy

- **Purpose** 
	1. Intermediary between clients on a local network and the internet
	2. Intercepts outbound requests from clients
	3. Forwards them to the internet on behalf of the clients
- **Features**
	1. Content Filtering
	2. Access Control
	3. Caching
	4. Anonymization
	5. Bandwidth control
	6. Policies to restrict access
- **Use cases**
	- Corporate networks
	- Educational Institutions
	- Public WIFI
- **Examples:**
	- Squid Proxy
	- CNTLM
	- Privoxy
	- TinyProxy
	- SquidGuard

You can read more about Forward proxy [here](Forward-Proxy.md).

## Reverse Proxy

- **Purpose** 
	1. Intermediary between clients and backend servers
	2. Receives requests from client on behalf of server
	3. Process those requests and Forward to the appropriate server.
- **Features**
	1. Load Balancing
	2. SSL Termination
	3. Caching
	4. Compression
	5. Request Routing
	6. Security Features such as DDosS protection, Web Application Firewall (WAF)
	7. Content Optimization
- **Use cases**
	- Mostly used in [Web Servers](Web-Servers.md) to enhance
		1. Performance
		2. Scalability
		3. Security
		4. Transparently Distributing client requests
- **Examples:**
	- [NGINX](NGINX.md)
	- [Apache](Apache.md)
	- HAProxy

You can read more about Reverse proxy [here](Reverse-Proxy.md).

## Load Balancer

- **Purpose** 
	1. Distributes incoming network traffic
- **Features**
	1. Health checks
	2. Session persistence
	3. Content-based routing
	4. SSL Termination
	5. Global server load balancing
- **Use cases**
	- Optimizing resource utilization, maximize throughput, etc, in
		- [Web Servers](Web-Servers.md)
		- Application servers
		- [Microservices](Microservices.md) environments
- **Examples:**
	- [NGINX](NGINX.md)
	- HAProxy
	- AWS Elastic Load Balancer

You can read more about Load Balancer [here](Load-Balancer.md)

## API Gateway

API Gateway is a specialized form of Reverse Proxy.

- **Purpose** 
	1. Manage requests for APIs
	2. Secure requests for APIs
	3. Optimize requests for APIs
- **Features**
	1. Authentication of APIs
	2. Rate Limiting of APIs
	3. Request/Response Transformation of APIs
	4. Logging of APIs
	5. Monitoring of APIs
	6. Versioning of APIs
- **Use cases**
	- [Microservices](Microservices.md) architecture 
	- [Distributed Systems](Distributed-Systems.md)
- **Examples:**
	- [NGINX](NGINX.md)
	- Kong

You can read more about API Gateway [here](API-Gateway.md).

# Resources

- [Proxy vs Reverse Proxy (Real-world Examples)](https://www.youtube.com/@ByteByteGo)
- [What is a reverse proxy?](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/)