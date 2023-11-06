# Architecture Pattern

## Layered Architecture

This architecture separates each component of a system including

- **Presentation Layer**
- **Business Layer**
- **Persistence Layer**
- **Database Layer**

For example, MVP or Model View Presenter Pattern separates the component of the software and it is an specialized form of Layered Architecture.

The main purpose of this kind of architecture is Abstraction and Encapsulation. The fact that layers don't negatively impact on each other.

## Event-Driven Architecture

This pattern promote the production and consumption of events between loosely coupled software component and services.

By use of publishing and subscribing in this architecture, It allows for highly decoupled architectures. 

CQRS or Command Query Responsibility Segregation is one prominent example. In this pattern, the data write operations or commands are separated from read operations or queries.

## Microkernel Architecture

This pattern emphasizes separating core system functionality into small microkernels and extended functionality into add-ons or plugins. 

For example, In an operating system a microkernel might oversee vital tasks like inter-process communication. In the meanwhile, It offloads other systems functions to external components. 
Eclipse IDE is also another example of this design. It's core runtime handles the plugin architecture, but the features are delivered as plugins. This design prioritize the extensibility, ease of maintenance, and fault isolation.

## [Microservice Architecture]()

This decompose an application into a collection of small, loosely coupled services. Each microservice is responsible for a specific business which contains its data models and APIs.

This architecture promotes modularization of functionality so services can be developed, deployed, and scaled independently. On the other hand, It can add complexity to inter-service communication and maintaining data consistancy.

> You can read more about [Microservices]().

## Monolothic

Everything in a monolothic application is in one code base and one single instance. This simplifies development and deployment, however Modular Monolothic suggest the same single instance of code base but in a emphasizes on clear boundaries within the code base. That helps the future migrations to microservice architecture.

# References

- [Top 5 Most Used Architecture Patterns](https://www.youtube.com/watch?v=f6zXyq4VPP8)