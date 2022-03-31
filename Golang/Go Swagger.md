# Swagger

## Gin Swagger

### Installation

```bash
# Install the go swag package
go get -u github.com/swaggo/swag/cmd/swag

# Init
swag init
# Flags for general information and locating the output doc files
swag init -g cmd/ride-recommender/main.go -o api/

# Install gin swagger
go get -u github.com/swaggo/gin-swagger
go get -u github.com/swaggo/files
```

### Usage

Put some comments for your routes

```go
// @BasePath /api/v1
// @Summary get passenger last ride
// @Description Get the latest ride for the passenger by passenger_id
// @Tags Rides
// @Accept json
// @Produce json
// @Param passenger_id path int true "Passenger ID"
// @Success 200 {object} passengerLastRideResponse
// @Failure 503 {object} errorResponse
// @Failure 400 {object} errorResponse
// @Router /passenger/{passenger_id}/ride/last [get]
```

Here is complete explanation on what do these comments and annotations mean.

[Introduction](https://swaggo.github.io/swaggo.io)

The you need to generate the new doc files.

```bash
swag init # with more options if it is needed
```

Config swagger at the start of your serving.

```go
// api is the package that contains the doc files `doc.go`

func init() {
	// Setup Swagger
	api.SwaggerInfo.BasePath = "/api/v1"
	api.SwaggerInfo.Title = "Ride Recommender"
	api.SwaggerInfo.Version = "0.1.0"
	api.SwaggerInfo.Description = "Suggests the next passenger ride"
}
```

Add this route to your routes to see the swagger ui.

```go
import (
   swaggerfiles "github.com/swaggo/files"
   ginSwagger "github.com/swaggo/gin-swagger"
)

r.GET("/swagger/*any", ginSwagger.WrapHandler(swaggerfiles.Handler))
```

Now the swagger ui is in `http://localhost:8080/api/v1/swagger/index.html`