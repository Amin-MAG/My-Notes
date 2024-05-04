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

## Using Modules

There are several modules for working with useful containers such as databases, message brokers, etc. For instance, In case that you need to create a test PostgreSQL container,

```go
package vclouddb

import (
	"context"
	"testing"
	"time"

	"github.com/stretchr/testify/assert"
	"github.com/testcontainers/testcontainers-go"
	"github.com/testcontainers/testcontainers-go/modules/postgres"
	"github.com/testcontainers/testcontainers-go/wait"
)

const (
	dbName     = "testdb"
	dbUser     = "testuer"
	dbPassword = "testpass"
)

func TestAddUser(t *testing.T) {
	ctx := context.Background()

	// Create the test container
	postgresContainer, err := postgres.RunContainer(ctx,
		testcontainers.WithImage("docker.io/postgres:16-alpine"),
		postgres.WithDatabase(dbName),
		postgres.WithUsername(dbUser),
		postgres.WithPassword(dbPassword),
		testcontainers.WithWaitStrategy(
			wait.ForLog("database system is ready to accept connections").
				WithOccurrence(2).
				WithStartupTimeout(5*time.Second),
		),
	)
	assert.NoError(t, err)

	// Clean up the container
	t.Cleanup(func() {
		err := postgresContainer.Terminate(ctx)
		assert.NoError(t, err)
	})

	uri, err := postgresContainer.ConnectionString(ctx)
	assert.NoError(t, err)

	db, err := ConnectWithURI(uri)
	assert.NoError(t, err, "can not connect to the database", err)

	phoneNumber := "981727375662"
	email := "test@gmail.com"
	actualUser := User{
		Username:    "test",
		Email:       &email,
		Password:    "123123qweqwe",
		PhoneNumber: &phoneNumber,
		FirstName:   "test first name",
		LastName:    "test last name",
	}

	u, err := db.AddUser(actualUser)
	assert.NotNil(t, u)
	assert.NoError(t, err)
	assert.Equal(t, u.Username, actualUser.Username)
	assert.Equal(t, u.Email, actualUser.Email)
	assert.Equal(t, u.PhoneNumber, actualUser.PhoneNumber)
	assert.Equal(t, u.FirstName, actualUser.FirstName)
	assert.Equal(t, u.LastName, actualUser.LastName)
	assert.NotEqual(t, u.Password, actualUser.Password)
	assert.NotEmpty(t, u.UUID)

	newUser, err := db.AddUser(actualUser)
	assert.Nil(t, newUser)
	assert.Error(t, err)
}
```
