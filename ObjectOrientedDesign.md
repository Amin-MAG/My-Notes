# OOD

It is going to focus on individual concepts and it’s not just generally speaking.

## Definition of Great Software

System Design: Maintainability, Flexibility, Extendability, Low Complexity

Non-Functional: Security, Performance & Efficiency, User Friendly

The definition of great software depends on the context of the software. Let’s see what do different programmers say about the Great Software.

The customer-friendly programmer

> The system do the things that the customer needs. (User Requirements, Not Buggy)
> 

The object-oriented programmer

> The system has Object-Oriented design and we follow the principles in that system. (DRY, Extendable design, Single Responsibility)
> 

The design-guru programmer

> Your objects in the system are loosely coupled and the system is open for extension but closed for modification.
> 

We have different stages to define the “Great Software”, The essential part of being a Great Software is to be `customer-friendly` and do what is expecting to do. (For the projects that are not starting from scratch) If we rich this part and want to have a better and Great Software, we need to follow `object-oriented` designs to make our code more extendable and better for future usage. At the last step, we need to consider the coupling for our system. (`design-guru`)

The main answer or approach:

1. Make sure the software does what the customer wants
2. Apply good object-oriented principles
3. Strive for a maintainable, reusable design

## Maintainability

- It has a readable code base, So another developer can read and understand it.
- It’s easy to fix the bugs in the project
- The system can work and continue properly.

## Encapsulation

- Hides the details
- Hides complexity
- Has some interface to interact with it (public methods)

Components use the API to interact and communicate with this component, So any changes in this component shouldn’t break the others (If the APIs are designed perfectly).

We kinda limit the interaction between components.

## DRY

Against of Copy-Paste programming. Don’t Repeat Yourself!

- It’s difficult to apply changes to the code
- Makes terrible bugs
- Makes our code less readable (It’s ugly)

## Information Expert

We have some classes and some methods, The Information Expert says “Put the methods in the classes which has the key and basic information for that method.”

## Regression testing

Your changes should be tested. The refactoring should comes with testing.

# Requirements

How to provide our software requirements, Stakeholders are the ones.

- Customers
- End User
- Development Team Members
- Management
- Technology providers

## Design for Dog remote

![Untitled](OOD/Untitled.png)

## Use cases

Use case describes all ways to achieve the goal from a specific starting point. It could be formal or informal.

Three part of a use case

- Clear Value: Every use case has a clear value to the system. It helps the users to achieve their goal.
- Start and Stop: Describe the start and stop state clearly and precisely.
- External Initiators

The constant thing in softwares is change 🙂

# Software in Real World

Asking `What if` Question helps us to find different scenarios.

## Analysis

- Studying the nature of something, determining its essential features and their relations.
- Exhibiting complex concepts or propositions
- Evaluation of an activity

## Delegation

Delegation is to pass parameter and give the duty by passing objects to another class.

Delegation shields your objects from implementation changes to other objects in your software.

## Use case

`Requirement Analysis`

It Addresses complete goal-oriented sequences of actions the system must perform. Use cases if for requirement analysis. 

Explicitly describes multiple paths in our system. It can used for user story estimation.

## CRC

`Technical`

Class-Responsibility-Collaborator is a kind of diagram that is different from class diagrams.

- It’s good to show the single responsibilities. (To recognize God-classes and blob classes)
- It’s a good diagram to show the dependencies between the classes.
- It helps us to make sure that we consider all of the scenarios.

![Untitled](OOD/Untitled%201.png)

The CRC cares about the requirements, but The class diagram shows the code structure.

Implicitly represents multiple path and scenarios.

## User Stories

`Technical`

On contrast of use cases, We can talk about the technical aspect of system. It describes a single feature or unit of work for a developer. The user stories should be designed in a way that is testable. (It means that it shouldn’t be vague) 

The user stories shouldn’t get too long. 

It’s good for individual task estimation.

## Use Case, CRC, User Story

![Untitled](OOD/Untitled%202.png)

## Epic VS stories agile development

![Untitled](OOD/Untitled%203.png)

### Initiative

Each project includes some different parts that a separate team is going to work on it. Each one of these parts are an Initiative of our project.

### Epic

Each Initiative has some Epics. Epics are the feature of that Initiative that should be done.

> Epic is use case here
> 

### Story

The Epics consist of some Stories or Tasks that developer is going to estimate and complete it. (Developer add some subtasks to the tasks) 

## Opposing

Another vision is to have Theme instead of Initiator.

![Untitled](OOD/Untitled%204.png)

There are no wrong and correct point of view it depends on your context and your project

![Untitled](OOD/Untitled%205.png)

This is another point of view for a large scale project.

## Extract Classes

- Textual analysis: Look at the nouns in requirements
- Entities and concepts: From application domain
- Experience

# Design

- Designs have a purpose.
- Designs have enough informations so that someone can implement it.
- There are different styles of designs.

It is the process of planning how to solve a problem through software. It’s a blueprint for developer. 

It should be easy to understand, flexible, satisfies the requirements, and...

By asking `What If` question, we can understand that how much our design is good.

## Use properties field

Some time the child classes don’t have any different behavior from their parents and the only thing is their fields. When you have different fields for a type you can make a `properties` field that is a Map data structure and put the attributes there. Having separate child classes is not needed here.

> Class are about behavior. If the class doesn’t have any behavior there is no need for class.
> 

## Hierarchy

We should avoid having a hierarchy design. Instead of that we can use composition or dependency injection.

The child classes should extend the parent. They have some different properties and methods.

## Cohesive class

A cohesive class does one thing really well and does not try to do or be something else. 

## Imperative abstraction

Classes are about being not doing!

## Speculative Development

It’s kinda over engineering

## Summary

- Good software is the one to can be changed and extended easily
- Having Cohesion for class or Single responsibility is important
- Avoid Hierarchy structures and try to use interfaces
- Use encapsulation

## Interface

They’re about behaviors and it’s not good to define properties in interface. Here the `realization` is the type of relation between interface and the class.

- Defines behavior
- Cannot be instantiated
- A class can implement multiple interfaces

## Abstract class

They have polymorphic behaviors. Here we have an `is` (has the same behavior) relation between the parent and child class.

- Defines behavior
- Can have implementation code
- Cannot be instantiated
- A class inherit from a single abstract class

# Solving Big Problems

1. Listen to customers and understand the requirements
2. List all of the features
3. Make user story diagrams for your scenarios
4. Design your components in system 
5. Start designing the class using o-o principles and design patterns that you’ve learned.
6. Again prioritize the features based on Three Qs, Find out architecture significant features.

## Traceability

It’s a matrix that specifies each users’ needs.

![Untitled](OOD/Untitled%206.png)

- We can trace that how much of our customers are satisfied with our current features.
- We can find out which feature is more important

## Domain Analysis

The process of identifying, collecting, organizing, and representing the relevant information of a domain, based upon existing systems and their histories, knowledge, experience, theories, and...

# Architecture

The structure of your application or system. Relations between the components.

Architecture is your design structure and highlights the most important parts of your app, and the relationships between those parts.

## Our blueprints

- Use cases
- UML Diagram
- Interfaces
- Prototypes

## Three Qs of architecture

To find out architecture significant Feature:

- Is it the part of the essence of the system?
- What does it really mean?
- How can I implement it?

## Build an executable architecture

1. Ask the customer
2. Commonality analysis
3. Implementation plan

You should focus on feature that is going to reduce the risk of project.

# Design Principles

They’re some concepts and basic tool that can be applied to designing or writing code to make that more maintainable, flexible, or extensible.

- Encapsulation
- DRY
- Single Responsibility

## Open-Closed Principle (OCP)

No one can change the class’s implementation. The class can be extended (With abstraction). Note that It can be also be done with delegation (To compose a logical field, like strategy).

## Single Responsibility Principle (SRP)

A systematic check is to use this table bellow. You use the class name for the first place and the method nam for the other.

![Untitled](OOD/Untitled%207.png)

Another method is LCOM. In this method we analyze the usage of the methods and fields of a class.

![Untitled](OOD/Untitled%208.png)

For example method `m1` uses the `v1`, `v3`, `v5`

![Untitled](OOD/Untitled%209.png)

## Liskov Substitution Principle (LSP)

The subtypes should be substitutable for their base types.

![Untitled](OOD/Untitled%2010.png)

We can use some interfaces here to fix the unexpected behavior of the child. Another example of this issue is when we create a bird class that has `fly()` method, But the ostrich, the child of this class, can not `fly()`

## Interface Segregation Principle (ISP)

We should split the interfaces to make a child implementation more flexiable.

## Dependency Inversion Principle (LIP)

A high-level module must not depends on the low-level module, It should depend on abstractions. 

# Other pages

[Lectures](Lectures%20efbb2.md)

[The Final Exam](The%20Final%20%2040ff8.md)