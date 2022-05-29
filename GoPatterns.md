# Go Patterns

## Anti-corruption Layer pattern

This pattern was first described by Eric Evans in *Domain-Driven Design*. We use this pattern to avoid depending on a third-party service. Anti-corruption layer pattern is also beneficial when we use multi-data-source and want to compare sources or interact with them.

For example, Consider two weather services; One gives the temperature in Fahrenheit and the other one in Kelvin. We design a layer between the third-party services and use cases and convert all of them to Celsius.

## Repository

It is nice to create such an interface for interacting with your databases.

```go
type CustomerRepository interface {
	GetCustomer(ctx context.Context, ID uuid.UUID) (*Customer, error)
	SearchCustomers(ctx context.Context, specification CustomerSpecification) (Customers, int, error)
	SaveCustomer(ctx context.Context, customer Customer) (*Customer, error)
	UpdateCustomer(ctx context.Context, customer Customer) (*Customer, error)
	DeleteCustomer(ctx context.Context, ID uuid.UUID) (*Customer, error)
}
```

In this way, any data source you want to use should implement these functions. You are independent of the database which you used. 

# See more
- [Software Design](Software%20Engineering/Software%20Design.md)
- [Design Patterns](Software%20Engineering/Design%20Patterns.md)
- [Microservices](Microservices.md)