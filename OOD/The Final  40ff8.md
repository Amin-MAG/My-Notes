# The Final Exam

### When not to use Object Oriented Paradigm?

- Don’t wrap everything in classes when they don’t need to be. Sometimes there is no need and the extra overhead just makes your code slower and more complex.
- Object state can get very complex
- If we are developing a small project like a small micro-service that is a small part of large system. The Object Oriented paradigm for implementing the project is an overhead and it’s over engineering.
- Also when we need more performance and we have some limitations in memory and cpu usage, We should consider the overhead of this paradigm. (To Creating and using  an object for everything.)

### What is the methods of Locality of change in softwares?

The locality in softwares development states that the person working closely, in terms of time and space, to an artifact is the most qualified person to remove defects associated with it.

There are several different types of locality of reference:

- **Temporal locality**: If at one point a particular memory location is referenced, then it is likely that the same location will be referenced again in the near future. There is temporal proximity between adjacent references to the same memory location. In this case it is common to make efforts to store a copy of the referenced data in faster memory storage, to reduce the latency of subsequent references. Temporal locality is a special case of spatial locality (see below), namely when the prospective location is identical to the present location.
- **Spatial locality**: If a particular storage location is referenced at a particular time, then it is likely that nearby memory locations will be referenced in the near future. In this case it is common to attempt to guess the size and shape of the area around the current reference for which it is worthwhile to prepare faster access for subsequent reference.
    - **Memory locality** (or *data locality*): Spatial locality explicitly relating to [memory](https://en.wikipedia.org/wiki/Computer_memory).
        
        [[3]](https://en.wikipedia.org/wiki/Locality_of_reference#cite_note-NistBig1-3)
        
- **[Branch](https://en.wikipedia.org/wiki/Branch_(computer_science)) locality**: If there are only a few possible alternatives for the prospective part of the path in the spatial-temporal coordinate space. This is the case when an instruction loop has a simple structure, or the possible outcome of a small system of conditional branching instructions is restricted to a small set of possibilities. Branch locality is typically not spatial locality since the few possibilities can be located far away from each other.
- **Equidistant locality**: Halfway between spatial locality and branch locality. Consider a loop accessing locations in an equidistant pattern, i.e., the path in the spatial-temporal coordinate space is a dotted line. In this case, a simple linear function can predict which location will be accessed in the near future.

In order to benefit from temporal and spatial locality, which occur frequently, most of the information storage systems are [hierarchical](https://en.wikipedia.org/wiki/Computer_data_storage#Hierarchy_of_storage). Equidistant locality is usually supported by a processor's diverse nontrivial increment instructions. For branch locality, the contemporary processors have sophisticated branch predictors, and on the basis of this prediction the memory manager of the processor tries to collect and preprocess the data of plausible alternatives.

### What is the difference in Risk-Driven approaches for software developing?

1. Identify and prioritize risks
2. Select and apply a set of techniques
3. Evaluate risk reduction

The risk-driven model guides developers to apply a minimal set of architecture techniques to reduce their most pressing risks.

The key element of the risk-driven model is the promotion of risk to prominence. What you choose to promote has an impact. Most developers already think about risks, but they think about lots of other things too, and consequently risks can be overlooked

### What is the pros and cons of Dependency Injection?

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

![Untitled](OOD/Lectures%20efbb2/Untitled.png)

### What is the difference between viewpoints and perspectives in software architecture?

Even tough views and viewpoints are powerful, the need for specifying quality properties remain.

The Three pillars of architectural design

- Stakeholders are the People impacted by the architecture, who have differing expectation and needs.
- Viewpoints are used to structure architecture definition by focusing on aspects of the system being designed.  *A **viewpoint** is a collection of patterns, templates, and conventions for constructing one type of view. It defines the stakeholders whose concerns are reflected in the viewpoint and the guidelines, principles, and template models for constructing its views. There are different accepted types of viewpoints, and a software architect can choose any of them to address in the specific project.*
    - ***Context viewpoint*** addresses overall relationships and dependencies between the components of the project and the way the system interacts with its environment
    - ***Functional viewpoint*** describes the system’s runtime functional elements, their responsibilities, interfaces, and interactions between them
    - ***Information viewpoint*** is intended to address the ways data is collected, stored, and managed to make the system work
    - ***Concurrency viewpoint*** includes aspects of how concurrent units of the program are handled and what parts specifically can execute concurrently
    - ***Development viewpoint*** is made for use of technical staff, which build and test this software. It includes design specifications of software and is used as a guideline when preparing the product
    - ***Deployment viewpoint*** describes where and how the system will be deployed and includes technical details for dependencies and other requirements
    - ***Operational viewpoint*** handles the part that comes in a production environment, which includes how the software will be operated, administered, and supported
- Perspectives focus on how a particular quality attribute impacts each viewpoint of the architecture. *An **architectural perspective** is a collection of architectural activities, tactics, and guidelines that are used to ensure that a system exhibits a particular set of related quality properties that require consideration across a number of the system’s architectural views.*
    - Security
    - Performance and Scalibility
    - Availability and Resilience
    - Evolution

### Explain the difference between Encapsulation, Abstraction, and Modularization.

Abstraction is generalization of the behavior in a class. To make other child classes inherit from this parent and implement their behavior. used to avoid DRY. They have polymorphic behaviors. Here we have an `is` (has the same behavior) relation between the parent and child class.

Encapsulation is to hide additional information. To hide complexity and define some interfaces as an API to communicate with different component of the program. We kinda limit the interaction between components. If the APIs has been defined well, changes shouldn’t break other components.

*Modularization is the process of separating the functionality of a program into independent, interchangeable modules, such that each contains everything necessary to execute only one aspect of the desired functionality.* A good point of this modular code base for a backend project is that making it to some micro services is much easier than non-modular spaghetti code.

# References

[Software Architecture Concepts: Architectural Perspectives](https://medium.com/analytics-vidhya/software-architecture-concepts-architectural-perspectives-dda93b775292)

[Software Architecture: Views and Viewpoints](https://frmusazade.medium.com/software-architecture-views-and-viewpoints-113d1592fe3a)

[Locality of reference - Wikipedia](https://en.wikipedia.org/wiki/Locality_of_reference)