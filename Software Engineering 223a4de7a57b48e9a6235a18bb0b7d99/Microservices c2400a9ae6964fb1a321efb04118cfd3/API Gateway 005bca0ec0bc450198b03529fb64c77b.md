# API Gateway

# What is the API Gateway?

An API gateway is an important concept in a microservices architecture. It forms an entry point for external clients(anything that is not part of the microservice system). It is a component that acts as an entry point for an application.

In other words, an API gateway is an API management server that has information about endpoints. It is also capable of performing authentication, rate limiting, load balancing, and more.

To get a better picture of an API gateway, we need to know why it is essential to have an API gateway.

# Why do we need to use API Gateway?

To understand the need for an API gateway, let’s discuss a use case of an e-commerce application.

## **Case Study**

Consider a case study of a complex page( let’s say product page) of an e-commerce application. If we look at the below page of the Amazon product listing, we can see a lot of information needed to be rendered by this specific page.

![API%20Gateway%20005bca0ec0bc450198b03529fb64c77b/Untitled.png](API%20Gateway%20005bca0ec0bc450198b03529fb64c77b/Untitled.png)

For illustration purposes, let’s list all the microservices that we might need to render the above particular page.

Consider **Search Product, Inventory, Shipping, Rating and Reviews, Recommendation Engine, Merchants, and Finance and Insurance** are the different seven(7) microservices being used for rendering the above page.

P.S: Above seven(7) microservices are just an assumption to explain the API gateway concept. In reality, Amazon could have a different number of microservices.

### What is the problem?

Since these microservices have been deployed separately on a different server if a client wants to access these services, at least seven(7) calls have to be requested for a single page.

![API%20Gateway%20005bca0ec0bc450198b03529fb64c77b/Untitled%201.png](API%20Gateway%20005bca0ec0bc450198b03529fb64c77b/Untitled%201.png)

I don’t think it’s a recommended approach because we have to make seven different calls, which would definitely impact performance, resource consumption, load time, etc. The client is also tightly coupled with all of the services, and suppose if we have to separate the Reviews and Rating microservices into two different services, we have to update the client code. The client has to make one call to get reviews, and one call to get ratings, which is really not the best way to deal with it.

### So what is the solution?

It is an API gateway.

In this approach, we have a layer between the client and microservices called an API gateway. It is a front-facing service for all of the microservices. Now any client who wants to access the microservices, the client has to call the API gateway. Now API gateway, in turn, makes a call to all of the microservices and gets whatever response we might need. This process is called API composition.

> In a nutshell, An API gateway sits in between the client and microservices and it acts as a gateway for all of the microservices.

Not only this but using an API Gateway benefits us in many ways.

## API Gateway Benefits

API gateways benefit us in implementing A/B testing, caching, managing access quotas, API health monitoring, API versioning, Chaos monkey testing, monetization, and a lot more. Let’s touch on some of the following benefits.

### Security

Every time an API call is performed, it has to access the services using public IP addresses. This exposes risks.

By switching on to API Gateways, these microservices can be accessed using private IP addresses only. This results in a more secure way of the transaction of data. Additionally, the usage of API Gateway also protects the data from malicious and DDoS attacks.

To ensure security, a [TLS certificate](https://geekflare.com/free-ssl-tls-certificate/) is necessary, API Gateway handles all of them by keeping all our APIs behind a single static IP or domain and helping protect them with keys, tokens, and IP filtering.

### Authentication, Authorization, and Fault Tolerance 🔐

It is important to ensure authentication and authorization of the user who logs into applications. The API Gateway makes it easier by being a single entry point and satisfies all the requirements easily. Thus, it allows only authorized users to log in, and authenticated users to make changes, so fault tolerance is gained.

### Load Balancing and Routing 🚏

In the case of multiple requests coming in and increasing traffic, API Gateway helps take care of it. It is done by creating multiple services and calling them on like Round-Robin. It can manage and routes the client requests based on user segmentation. Thus, different quality or rate of speed of content is provided for different users.

Consider a use case where two microservices are defined for returning low-quality images/videos and high-quality images/videos for a desktop and mobile, respectively. In this case, we can configure an API gateway in such a way that it acts as a router and if the request is coming from a mobile it will route that request to the low-quality images/videos service, and if the request is coming from the desktop, it can route to high-quality images/videos service. This routing can be done based on headers, paths, and params, etc.

### Insulation

If one or more microservices have been added to the application or removed, we will not update the client code. In this case, we need to perform some changes in the API gateway itself to make a call according to updated microservices.

### Reverse Proxy and Caching

Serving a static file (HTML, JS, CSS, fonts) by a microservice is not the best use, In this case, we can move these files to the API gateway.

An API gateway can keep hold of all the static contents and can directly serve the client. Similarly, consider a service that evaluates the trending products, and these trends are being calculated hourly or daily. So once the trend is calculated for the rest of the time, the service will return the same response repeatedly. In this case, an API gateway has a feature called **response cache,** where we can mention a URL and threshold time for which it needs to cache the responses.

### Protocol Adaptor

If we want to take advantage of protocol like web socket or a newer version of HTTP, i.e., HTTP/2, and even if our backend services are not ready or not compatible with HTTP/2 or web socket, an API gateway can take the responsibility of converting a newer to an older protocol. It can act as a protocol adaptor.

# API Gateways

## Kong Gateway

Kong Gateway is the most popular open-source cloud-native API gateway built on top of a lightweight proxy. It is written in Lua running with the help of the Nginx. It is a template engine that helps to accelerate the event time. It guarantees to deliver unparalleled latency performance and scalability for all our microservice applications regardless of where they run.

Companies like Nasdaq, Honeywell, Cisco, FAB, Expedia, Samsung, Siemens, and Yahoo Japan extensively use the Kong API gateway.

Some of the features offered by Kong are:

- Authentication
- Traffic Control
- Analytics
- Transformations
- Logging
- Serverless
- Extendable using Plugin architecture

Kong got very good [documentation](https://docs.konghq.com/) and [integration](https://docs.konghq.com/hub/).

You can run Kong on your preferred [cloud platform](https://geekflare.com/cloud-hosting-platform/).

## Apache APISIX

[Apache APISIX](https://apisix.apache.org/) was initially born at China’s **ZhiLiu** technology and a later stage, it entered the apache incubator and was made open-source. The vice president of the project, **Ming Wen**, states that this API gateway solves various challenges brought by cloud-native & microservices.

Apache ApiSix is being used by companies like **360, HelloTalk, NetEase, TravelSky**, and many more.

Apache APISIX is based on Nginx and etcd, and it has dynamic routing and plug-in hot loading, which is especially suitable for API management under the microservice system.

## Tyk

Tyk is an enterprise-ready open-source API gateway. You have an option to either go for self-hosted or managed.

The following are some of the out-of-the-box features offered by TYK.

- Authentication
- Quotas & Rate Limiting
- Version Control
- Notifications and Events
- Mock out APIs
- Detailed Monitoring and Analytics
- Committed to backward compatibility
- GraphQL Out of the Box

TYK is also available on the [AWS marketplace](https://aws.amazon.com/marketplace/seller-profile?id=432b7859-4299-4278-8eb2-f7bbe7739ec6). A good choice if your application stack is on AWS.

## Goku

Goku API Gateway is an umbrella project of EOLINK Inc. It is a Golang-based microservice gateway that enables high-performance dynamic routing, service orchestration, multi-tenancy management, API access control, etc.

Goku provides a graphic interface and a plug-in system to make configuration easier and expand more conveniently. Apart from standard features, Goku offers clustering, hot updates, alerting, logging, etc.

more services on this page [https://geekflare.com/api-gateway/](https://geekflare.com/api-gateway/)

# **Conclusion**

Once your API is ready, don’t forget to [monitor](https://geekflare.com/api-monitoring-tools/) and [secure](https://geekflare.com/securing-api-endpoint/) them. If you are still under development, check out these [tools](https://geekflare.com/api-tools/) to expedite the API testing & development.

The above should give you an idea about available API Gateway and Management solutions. If you are under a tight budget, then you can try open-source. It the best to install some of them on your [cloud VM](https://geekflare.com/cloud-hosting-platform/) to see what works for you.

# Resources

[14 Open Source and Managed API Gateway for Modern Applications](https://geekflare.com/api-gateway/)