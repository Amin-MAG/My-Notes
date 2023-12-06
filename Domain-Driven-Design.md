# Domain Driven Design  (DDD)

Domain-Driven Design (DDD) is a software development approach and methodology that focuses on designing software systems around the core concepts and models of the problem domain.

1. Collaborative Nature
2. Modeling Centric
3. Incremental Development

Each project have multiple sub-domains. For instance, sub-domains of Netflix can be Video Steaming, Billing, Recommendation, etc. In an iterative process consists of meetings between both engineering and product teams, we can recognize the sub-domains of the system. Sometimes, It is possible to break down a sub-domain to smaller sub-domains during the project.

## The key principles and components

### Ubiquitous Language

It ensures that everyone involved in the project understands and uses the same terminology, reducing misunderstandings and communication gaps.

### Bounded Context

A large domain can be broken down into smaller, self-contained areas called bounded contexts. Within each bounded context, specific models and terms can be defined. Bounded contexts help manage complexity and prevent conflicts between different parts of the system. For example, a Billing domain might call `Subscribers` customers while in the VideoStreaming domain, it might be called viewers. This is completely ok as long as you define the subdomains considering their relationship to each other.

### Entity

Entities are mutable objects with a distinct identity that have a lifespan within the application. They are core elements of the domain, such as customers, products, or orders.

### Aggregate

Aggregates are clusters of related entities and value objects treated as a single unit. They have a well-defined boundary and a root entity that controls access to the aggregate. Aggregates help maintain data consistency and encapsulation within a bounded context. You might have some rules in your aggregates; for example, calculating the price of an order aggregate.

**It also creates a transactional boundary.** So whenever changes are made to the aggregate, they should be either committed or rolled back to the database to be consistent.


### Context Mapping

It addresses the interactions and boundaries between different bounded contexts within a complex software system.

Some of the common types of context mapping relationships include

1. Shared Kernel
2. Customer/Supplier
3. Conformist
4. Anti-Corruption Layer
5. Open Host Service
6. Published Language
7. Partnership
#### Anti-Corruption layer

When two bounded contexts have significantly different models and terminologies, an anti-corruption layer (ACL) is introduced. This layer translates data and requests between the two contexts, ensuring that each context remains insulated from the other's complexities and inconsistencies.
### Value Object

Value objects represent concepts without an identity of their own. They are immutable and defined solely by their attributes. Examples include dates, monetary values, or geographic coordinates. You can check and validate these kinds of objects in their constructors. (So you do not have to check it each time)

### Repository

Repositories provide an abstraction for data access, allowing you to store and retrieve aggregates without exposing the underlying data storage details.

### Service

Domain services encapsulate domain logic that doesn't naturally fit into entities or value objects. They are stateless and used for operations that are not tied to a specific entity.
