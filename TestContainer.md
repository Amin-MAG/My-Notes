# TestContainer

Testcontainers is a library that provides lightweight, throwaway instances of common databases, Selenium web browsers, or anything else that can run in a Docker container for use in unit tests or automated integration tests. It simplifies the process of setting up and tearing down test environments, making it easier to write reliable and isolated tests that interact with external services or dependencies. 

Testcontainers handles the lifecycle of these containers, starting them before tests run and stopping them after tests complete, ensuring a clean and consistent testing environment.

## Using Testcontainer Core

For instance, let's say you want to use Redis in your unit test.

```go
import (
    "context"

    "github.com/testcontainers/testcontainers-go"
    "github.com/testcontainers/testcontainers-go/wait"
)

func TestWithRedis(t *testing.T) {
    ctx := context.Background()

	// To create the test container
    req := testcontainers.ContainerRequest{
        Image:        "redis:latest",
        ExposedPorts: []string{"6379/tcp"},
        WaitingFor:   wait.ForLog("Ready to accept connections"),
    }
    redisC, err := testcontainers.GenericContainer(ctx, testcontainers.GenericContainerRequest{
        ContainerRequest: req,
        Started:          true,
    })
    if err != nil {
        log.Fatalf("Could not start redis: %s", err)
    }
    
    // To terminate the test container after the test
    defer func() {
        if err := redisC.Terminate(ctx); err != nil {
            log.Fatalf("Could not stop redis: %s", err)
        }
    }()
    
    // Do your test...
}
```

Instead of using mock, for these kind of communication that are not real and take time to implement, you can create test containers for your tests.

```go
func TestWithRedis(t *testing.T) {
	// Create the test container
	// ...

	endpoint, err := redisC.Endpoint(ctx, "")
	if err != nil {
	    t.Error(err)
	}
	
	client := redis.NewClient(&redis.Options{
	    Addr: endpoint,
	})
	
	_ = client

	// Perform your test having this client
	// ...
}
```
