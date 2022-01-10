# Open Telemetry

To add new span

```go
// New span
_, span := otel.Tracer(name).Start(ctx, "Poll")
defer span.End()

// To set some attributes
span.SetAttributes(attribute.Bool("isTrue", true), attribute.String("stringAttr", "hi!"))
var myKey = attribute.Key("myCoolAttribute")
span.SetAttributes(myKey.String("a value"))

```

You can create events in your span

```go
span.AddEvent("Acquiring lock")
mutex.Lock()
span.AddEvent("Got lock, doing work...")
// do stuff
span.AddEvent("Unlocking")
mutex.Unlock()
```

To create a new exporter

```go
// newExporter returns a console exporter.
func newExporter(w io.Writer) (trace.SpanExporter, error) {
	return stdouttrace.New(
		stdouttrace.WithWriter(w),
		// Use human-readable output.
		stdouttrace.WithPrettyPrint(),
		// Do not print timestamps for the demo.
		stdouttrace.WithoutTimestamps(),
	)
}

// newResource returns a resource describing this application.
func newResource() *resource.Resource {
	r, _ := resource.Merge(
		resource.Default(),
		resource.NewWithAttributes(
			semconv.SchemaURL,
			semconv.ServiceNameKey.String("fib"),
			semconv.ServiceVersionKey.String("v0.1.0"),
			attribute.String("environment", "demo"),
		),
	)

	return r
}
```

You should then use theses functions in this order

```go
func main() {
	// Write telemetry data to a file.
	f, err := os.Create("traces.txt")
	if err != nil {
		l.Fatal(err)
	}
	defer f.Close()

	exp, err := newExporter(f)
	if err != nil {
		l.Fatal(err)
	}

	tp := trace.NewTracerProvider(
		trace.WithBatcher(exp),
		trace.WithResource(newResource()),
	)
	defer func() {
		if err = tp.Shutdown(context.Background()); err != nil {
			l.Fatal(err)
		}
	}()
	otel.SetTracerProvider(tp)
  // The rest of the code
}
```

If you want to expose the traces to the jaeger

```go
func main(){
  // Create the Jaeger exporter
	exp, err = jaeger.New(jaeger.WithCollectorEndpoint(jaeger.WithEndpoint(url)))
	if err != nil {
		panic(err)
	}

	// There are some protocols to send these traces
  // OTEL_EXPORTER_JAEGER_AGENT_HOST should be set for agent host
	// OTEL_EXPORTER_JAEGER_AGENT_PORT should be set for agent port
	exporter, err := jaeger.New(jaeger.WithAgentEndpoint())
	if err != nil {
		return nil, err
	}
}
```

You can use this code for propagation

```go
// Set propagation
otel.SetTextMapPropagator(jaeger_propagator.Jaeger{})
```

To get current span you can use

```go
ctx := context.TODO()
span := trace.SpanFromContext(ctx)
```