# Prometheus

Prometheus is for auto monitoring and alerting. 

- It is reliable and standalone.
- It’s hard to scale it.

# Architecture

### Prometheus server

It is the main component and consists of

1. Time-Series Database
2. Data Retrieval Worker (Pulls data from targets and saves them in the database)
3. Web server that accepts queries (Connected to Prometheus UI or Grafana)

### Alert Manager

It will notify using email, slack, or...

# Metrics

Getting started with Prometheus is not a complex task, but you need to understand how it works and what type of data you can use to monitor and alert.

What I included here is a simple use case; you can do more with Prometheus.

For example, you can configure alerts using external services like Pagerduy. Or you can receive metrics from short-lived applications like batch jobs. But keep in mind that Prometheus focuses only on one of the [critical pillars of observability](https://www.scalyr.com/blog/three-pillars-of-observability/): metrics. You’ll need to use other tools for the rest of the pillars like [Jaeger for traces](https://www.scalyr.com/blog/jaeger-tracing-tutorial/).

Prometheus is a good fit for collecting metrics from servers or distributed systems like micro-services. You can diagnose problems by querying data or creating graphs. And that means you’ll get a better understanding of your workloads’ health.

## Metric Types

### Counters

> How many times?
> 

Counters are a simple metric type that can only be incremented or be reset to zero on restart. It is often used to count primitive data like the total number of requests to a services or number of tasks completed. Most counters are therefore named using the *_total* suffix e.g. *http_requests_total*.

```
# Total number of HTTP request
http_requests_total

# Total number of completed jobs
jobs_completed_total
```

The absolute value of these counters is often irrelevant and does not give you much information about the applications state. The real information can be gathered by their evolution over time which can be obtained using the *rate()* function.

### Gauges

> What is the value of x now?
> 

Gauges also represent a single numerical value but different to counters the value can go up as well as down. Therefore gauges are often used for measured values like temperature, humidy or current memory usage.

Unlike with counters the current value of a gauge is meaningful and can be directly used in graphs and tests.

### Histograms

> How long or How big?
> 

Histograms are used to measure the frequency of value observations that fall into specific predefined buckets. This means that they will provide information about the distribution of a metric like response time and signal outliers.

By default Prometheus provides the following buckets: .005, .01, .025, .05, .075, .1, .25, .5, .75, 1, 2.5, 5, 7.5, 10. These buckets are not suitable for every measurement and can therefore easily be changed.

### Summaries

Summaries are very similar to Histograms because they both expose the distribution of a given data set. The one major difference is that a Histogram estimate quantiles on the Prometheus server while Summaries are calculated on the client side.

Summaries are more accurate for some pre-defined quantiles but can be a lot more resource expensive because of the client-side calculations. That is why it is recommended to use Histograms for most use-cases.

# Query Language

## Data types

- **Instant vector** - a set of time series containing a single sample for each time series, all sharing the same timestamp
- **Range vector** - a set of time series containing a range of data points over time for each time series
- **Scalar** - a simple numeric floating point value -
- **String** - a simple string value; currently unused -

## Literal

- Float like `3.4e-9`
- String like `"this is a string"`

## ****Time series Selectors****

- Instant vector selectors like `http_requests_total`
- Range vector selectors like `http_requests_total{job="prometheus"}[5m]`
- Time Durations like `1h30m`
- Offset Modifier like `http_requests_total offset 5m`

## Operations

### Sum

```groovy
// To aggregate the status of the volumes by their phases
sum by (phase) (kube_persistentvolumeclaim_status_phase)

// Calculate the percentage of something 
(
sum by (persistentvolumeclaim) (kubelet_volume_stats_used_bytes{persistentvolumeclaim=~"stalin-.*"})
/
sum by (persistentvolumeclaim) (kube_persistentvolumeclaim_resource_requests_storage_bytes{persistentvolumeclaim=~"stalin-.*"})
) * 100
```

## Querying

To compare values

- `=`: Select labels that are exactly equal to the provided string.
- `!=`: Select labels that are not equal to the provided string.
- `=~`: Select labels that regex-match the provided string.
- `!~`: Select labels that do not regex-match the provided string.

```groovy
// To select a label
scrape_duration_seconds{container="exporter"}
// To use regex in these labels you can use
scrape_duration_seconds{container=~".*ter"}
// Query all HTTP status codes except the 4XX status codes
http_requests_total{status!~"4.."}
// Multiple filters
gin_ride_recommender_requests_total{pod="ride-recommender-40-np2dx", code!=200}
```

## Functions

```groovy
// The following example expression returns the per-second 
// rate of HTTP requests as measured over the last 5 minutes, 
// per time series in the range vector:
rate(http_requests_total{job="api-server"}[5m])

// Number of times that it has been changed
changes(http_requests_total{job="api-server"}[5m])

// To round the numbers
ceil(http_requests_total)

// To find the delta for Gauges
delta(prometheus_engine_query_duration_seconds[40s])
```

# Add a Custom Metric

```go
package main

import (
	"fmt"
	"github.com/gorilla/mux"
	"github.com/prometheus/client_golang/prometheus"
	"github.com/prometheus/client_golang/prometheus/promauto"
	"github.com/prometheus/client_golang/prometheus/promhttp"
	"log"
	"net/http"
	"strconv"
)

var totalRequests = prometheus.NewCounterVec(
	prometheus.CounterOpts{
		Name: "http_requests_total",
		Help: "Number of get requests.",
	},
	[]string{"path"},
)

func prometheusMiddleware(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		rw := NewResponseWriter(w)
		next.ServeHTTP(rw, r)

		totalRequests.WithLabelValues(path).Inc()
	})
}

func init() {
	prometheus.Register(totalRequests)
}

func main() {
	router := mux.NewRouter()
	router.Use(prometheusMiddleware)

	// Prometheus endpoint
	router.Path("/prometheus").Handler(promhttp.Handler())

	// Serving static files
	router.PathPrefix("/").Handler(http.FileServer(http.Dir("./static/")))

	fmt.Println("Serving requests on port 9000")
	err := http.ListenAndServe(":9000", router)
	log.Fatal(err)
}
```

Another example for using timer in response time

```go
package main

import (
	"fmt"
	"github.com/gorilla/mux"
	"github.com/prometheus/client_golang/prometheus"
	"github.com/prometheus/client_golang/prometheus/promauto"
	"github.com/prometheus/client_golang/prometheus/promhttp"
	"log"
	"net/http"
	"strconv"
)

type responseWriter struct {
	http.ResponseWriter
	statusCode int
}

func NewResponseWriter(w http.ResponseWriter) *responseWriter {
	return &responseWriter{w, http.StatusOK}
}

func (rw *responseWriter) WriteHeader(code int) {
	rw.statusCode = code
	rw.ResponseWriter.WriteHeader(code)
}

var totalRequests = prometheus.NewCounterVec(
	prometheus.CounterOpts{
		Name: "http_requests_total",
		Help: "Number of get requests.",
	},
	[]string{"path"},
)

var responseStatus = prometheus.NewCounterVec(
	prometheus.CounterOpts{
		Name: "response_status",
		Help: "Status of HTTP response",
	},
	[]string{"status"},
)

var httpDuration = promauto.NewHistogramVec(prometheus.HistogramOpts{
	Name: "http_response_time_seconds",
	Help: "Duration of HTTP requests.",
}, []string{"path"})

func prometheusMiddleware(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		route := mux.CurrentRoute(r)
		path, _ := route.GetPathTemplate()

		timer := prometheus.NewTimer(httpDuration.WithLabelValues(path))
		rw := NewResponseWriter(w)
		next.ServeHTTP(rw, r)

		statusCode := rw.statusCode

		responseStatus.WithLabelValues(strconv.Itoa(statusCode)).Inc()
		totalRequests.WithLabelValues(path).Inc()

		timer.ObserveDuration()
	})
}

func init() {
	prometheus.Register(totalRequests)
	prometheus.Register(responseStatus)
	prometheus.Register(httpDuration)
}

func main() {
	router := mux.NewRouter()
	router.Use(prometheusMiddleware)

	// Prometheus endpoint
	router.Path("/prometheus").Handler(promhttp.Handler())

	// Serving static files
	router.PathPrefix("/").Handler(http.FileServer(http.Dir("./static/")))

	fmt.Println("Serving requests on port 9000")
	err := http.ListenAndServe(":9000", router)
	log.Fatal(err)
}
```

# Run-on docker

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

## Add Prometheus to Grafana

You can create nice dashboards with Grafana beside the prometheus.

```yaml
grafana:
    image: grafana/grafana
    user: "472"
    depends_on:
      - prometheus
    ports:
      - 3000:3000
    restart: always
```

# Configuration

Configuration

`scrape_configs` tell Prometheus where your applications are.

`rule_files` tells Prometheus where to search for the alert rules. We come to this in a moment.

`scrape_interval` defines how often to check for new metric values.

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

# Alerting

## RED Strategy

1. Rate
2. Error
3. Duration

There are some other use cases:

- Queue sizes
- Comparing the current state of today with 7 days before.
- Monitoring and alerting for dependencies

# Resources

[Prometheus Tutorial: A Detailed Guide to Getting Started | Scalyr](https://www.sentinelone.com/blog/prometheus-tutorial-detailed-guide-to-getting-started/)

[Golang Application monitoring using Prometheus](https://gabrieltanner.org/blog/collecting-prometheus-metrics-in-golang)

[How Prometheus Monitoring works | Prometheus Architecture explained](https://www.youtube.com/watch?v=h4Sl21AKiDg)

[](https://prometheus.io/docs/prometheus/latest/querying)