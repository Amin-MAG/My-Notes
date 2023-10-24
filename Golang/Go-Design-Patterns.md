# Go Design Patterns

You might know about some of design patterns. People, when they are thinking about design patterns, suggests some implementations using Object Oriented Languages. Yep, some of the design patterns are quite useful sometimes, but what about the implementation of the exact same design patterns in languages that does not support OOP? As you might know, Go language does not support some fantasy features like Classes and inheritance. Are the traditional thinking of them still working? I am going to see 

- Whether common design patterns are useful in Go language or not.
- How can we use the identical design patterns' concept and implement it in Go.

## Creational

### Factory Method (useful but we need to do some changes)

Because of the structure of the Go, It's completely useless that you use the exact Factory Method design pattern in your Go code. Create a new Factory interface just to create the instance of your target Product? It just make your code weird.

The best way to create such related instances is to define a general interface for all of them, and add a `New(ops Options)` function for each instance. For example, let's say we have a company that can create multiple kinds of Cars. 

```go
type SedanOpts struct {
}

type Sedan struct {
    BodyType     string
    SeatingCapacity int
    TrunkSpace   string
    FuelEfficiency string
    GroundClearance string
}

type SUVOpts struct {
}

type SUV struct {
    BodyType     string
    SeatingCapacity int
    CargoSpace   string
    OffRoadCapability bool
    TowingCapacity string
}
```

By defining a general interface for cars, we are able to create different cars.

```go
type Car interface {  
    Drive()  
    GetInfo() string  
}

func NewSedan(opts SedanOpts) (Car, error) {
	// Use the specific Sedan Options to create the instance 
	return &Sedan{}, nil
}

func (s *Sedan) Drive() {
	// ...
}

func (s *Sedan) GetInfo() string {
	return "Sedan Car"
}

func NewSUV(opts SUVOpts) (Car, error) {
	// Use the specific SUV Options to create the instance 
	return &SUV{}, nil
}

func (s *SUV) Drive() {
	// ...
}

func (s *SUV) GetInfo() string {
	return "SUV Car"
}
```

Then, whenever we want to create a car-related instance. We use these `New()` functions to get the `Car`.  We are using the interface in our program, so it will become highly independent to the real type of car.

> Using the traditional Factory Method design pattern can make the creation process more complex. In addition, We need to pass some arguments to the factory method class that might not be used. This lead to confusion, when developer is using the factory class, because after a while the developer can not understand which arguments are essential for creating the target. In my opinion, also passing all of the prerequisites to the method is not a good approach.

