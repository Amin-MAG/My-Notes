# Lectures

## API Design

API is an interface that we can talk to other applications.

- Open or Public
- Partner: For organizations
- Internal: For a internal organization
- Composite: Mixture of above items

Why is it important to care about api design?

- Consistency in API
- To Benefit our users
- Better service

## Clean Code Rules Regarding Functions

Good functions:

- Should be small
- Block and indenting
- Do on thing in functions
- Names should be what it is going to do
- DRY
- Avoid side effects
- Remove dead codes
- Consider function inputs and outputs

## Dependency Injection

Pros

- Late Binding: Services can be swapped with other services without recompiling code.
- Extensibility
- Reduces the overall cost of changing the system
- Classes are more modular
- Introduces abstraction layers. Increases Single Responsibilities of classes.
- Makes our code more testable because we can mock the inputs and pass the (or literally inject  them) so with DI we provide everything needed for testing a method or a class.
- Separates the behavior into highly cohesive units, which has the side effect of creating smaller, more reusable components.

Cons

- Highlight design problems (So that many programmers start to blame DI)
- For programmers is more difficult to follow the code, due to the separation of responsibilities.

![Untitled](Lectures%20efbb2/Untitled.png)

### Patterns

Client Classes

Service Classes

Injector Classes

### Injections

1. Constructor Injection
2. Setter Injection
3. Method Injection