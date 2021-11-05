# Prometheus

## Run-on docker

Simple docker-compose file for running Prometheus.

```yaml
version: "3"
services:
  prometheus:
    image: prom/prometheus:latest
    ports:
      - 9090:9090
    volumes:
      - "./prometheus.yml:/etc/prometheus/prometheus.yml"
      - "./prometheus.rules.yml:/etc/prometheus/prometheus.rules.yml"
```

## Configuration

In `scrape_configs`, It will search for `/metrics` endpoint to gather the metrics.

You can set the time interval to gather the metrics for each job.

```yaml
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.
  evaluation_interval: 15s # Evaluate rules every 15 seconds.

  # Attach these extra labels to all timeseries collected by this Prometheus instance.
  external_labels:
    monitor: 'scalyr-blog'

rule_files:
  - 'prometheus.rules.yml'

scrape_configs:
  - job_name: 'prometheus'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'golang-random'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['hashbrowns:8080']
        labels:
          group: 'production'
```

## Metrics

Getting started with Prometheus is not a complex task, but you need to understand how it works and what type of data you can use to monitor and alert.

What I included here is a simple use case; you can do more with Prometheus.

For example, you can configure alerts using external services like Pagerduy. Or you can receive metrics from short-lived applications like batch jobs. But keep in mind that Prometheus focuses only on one of the [critical pillars of observability](https://www.scalyr.com/blog/three-pillars-of-observability/): metrics. You’ll need to use other tools for the rest of the pillars like [Jaeger for traces](https://www.scalyr.com/blog/jaeger-tracing-tutorial/).

Prometheus is a good fit for collecting metrics from servers or distributed systems like micro-services. You can diagnose problems by querying data or creating graphs. And that means you’ll get a better understanding of your workloads’ health.

## Add Prometheus to Grafana

You can create nice dashboards with Grafana beside the prometheus.

```go
grafana:
    image: grafana/grafana
    user: "472"
    depends_on:
      - prometheus
    ports:
      - 3000:3000
    restart: always
```

# Resources

[Prometheus Tutorial: A Detailed Guide to Getting Started | Scalyr](https://www.sentinelone.com/blog/prometheus-tutorial-detailed-guide-to-getting-started/)