# Jaeger

Jaeger, inspired by **[Dapper](https://research.google.com/pubs/pub36356.html)** and **[OpenZipkin](http://zipkin.io/)**, is a distributed tracing system released as open source by **[Uber Technologies](http://uber.github.io/)**. It is used for monitoring and troubleshooting microservices-based distributed systems, including:

- Distributed context propagation
- Distributed transaction monitoring
- Root cause analysis
- Service dependency analysis
- Performance / latency optimization

Jaeger backend is designed to have no single points of failure and to scale with the business needs. For example, any given Jaeger installation at Uber is typically processing several billion **spans** per day.

## Get Started

Use docker

```bash
docker run -d --name jaeger \
  -e COLLECTOR_ZIPKIN_HOST_PORT=:9411 \
  -p 5775:5775/udp \
  -p 6831:6831/udp \
  -p 6832:6832/udp \
  -p 5778:5778 \
  -p 16686:16686 \
  -p 14268:14268 \
  -p 14250:14250 \
  -p 9411:9411 \
  jaegertracing/all-in-one:1.29s
```

The container exposes the following ports:

[Ports](Ports%2059e8c.csv)