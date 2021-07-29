# Interview Questions

# Top Questions

## What are the advantages of Microservices Architecture?

- Independent Development: All microservices can be easily developed based on their individual functionality.
- Independent Deployment: Based on their services, they can be individually deployed in any application.
- Fault Isolation: Even if one service of the application does not work, the system continues to function.
- Mixed Technology Stack: Different languages and technologies can be used to build different services of the same application.
- Granular Scaling: Individual components can scale as per need, there is no need to scale all components together

## What do you know about microservices?

- **Microservices**, aka m**icroservice architecture**, is an architectural style that structures an application as a collection of small autonomous services, modeled around a **business domain.**
- In layman terms, you must have seen how bees build their honeycomb by aligning hexagonal wax cells.
- They initially start with a small section using various materials and continue to build a large beehive out of it.
- These cells form a pattern resulting in a strong structure that holds together a particular beehive section.
- Here, each cell is independent of the other but it is also correlated with the other cells.
- This means that damage to one cell does not damage the other cells, so, bees can reconstruct these cells without impacting the complete beehive.

![https://www.edureka.co/blog/wp-content/uploads/2018/06/microservices-What-Are-Microservices-edureka.png](https://www.edureka.co/blog/wp-content/uploads/2018/06/microservices-What-Are-Microservices-edureka.png)

Refer to the above diagram. Here, each hexagonal shape represents an individual service component. Similar to the working of bees, each agile team builds an individual service component with the available frameworks and the chosen technology stack. Just as in a beehive, each service component forms a strong microservice architecture to provide better scalability. Also, issues with each service component can be handled individually by the agile team with no or minimal impact on the entire application.

## What are the features of microservices?

![https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/06/microservices-slide-4.png](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/06/microservices-slide-4.png)

- **Decoupling** – Services within a system are largely decoupled. So the application as a whole can be easily built, altered, and scaled
- **Componentization** – Microservices are treated as independent components that can be easily replaced and upgraded
- **Business Capabilities** – Microservices are very simple and focus on a single capability
- **Autonomy** – Developers and teams can work independently of each other, thus increasing the speed
- **Continuous Delivery** – Allows frequent releases of software, through systematic automation of software creation, testing, and approval
- **Responsibility** – Microservices do not focus on applications as projects. Instead, they treat applications as products for which they are responsible
- **Decentralized Governance** – The focus is on using the right tool for the right job. That means there is no standardized pattern or any technology pattern. Developers have the freedom to choose the best useful tools to solve their problems
- **Agility** – Microservices support agile development. Any new feature can be quickly developed and discarded again

## What are the best practices for microservices?

The following are the best practices to design microservices:

![https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/06/best.png](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/06/best.png)

## How does microservice architecture work?

A microservice architecture has the following components:

![https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/06/archi.png](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/06/archi.png)

- **Clients** – Different users from various devices send requests.
- **Identity Providers** – Authenticates user or clients identities and issues security tokens.
- **API Gateway** – Handles client requests.
- **Static Content** – Houses all the content of the system.
- **Management** – Balances services on nodes and identifies failures.
- Service Discovery – A guide to find the route of communication between microservices.
- **Content Delivery Networks** – Distributed network of proxy servers and their data centers.
- **Remote Service** – Enables the remote access information that resides on a network of IT devices.

## What are the pros and cons of microservices?

[Pros And Cons Table](Interview%20Questions%204b38b3e3e29b48d2ac1a84891042c2c9/Pros%20And%20Cons%20Table%20cb97a4b387754fa69e6b9af8a01ecb96.csv)

## What is the difference between monolithic, SOA, and microservice architecture?

![https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/06/microservices-slide-9.png](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/06/microservices-slide-9.png)

- **Monolithic Architecture** is similar to a big container wherein all the software components of an application are assembled together and tightly packaged.
- A **Service-Oriented Architecture** is a collection of services that communicate with each other. The communication can involve either simple data passing or it could involve two or more services coordinating some activity.
- **Microservice Architecture** is an architectural style that structures an application as a collection of small autonomous services, modeled around a business domain.

## What are the challenges you face while working with microservice architecture?

Developing a number of smaller microservices sounds easy, but the challenges often faced while developing them are as follows.

- **Automate the Components**: Difficult to automate because there are a number of smaller components. So for each component, we have to follow the stages of Build, Deploy and, Monitor.
- **Perceptibility**: Maintaining a large number of components together becomes difficult to deploy, maintain, monitor, and identify problems. It requires great perceptibility around all the components.
- **Configuration Management**: Maintaining the configurations for the components across the various environments becomes tough sometimes.
- **Debugging**: Difficult to find out each and every service for an error. It is essential to maintain centralized logging and dashboards to debug problems.

## What are the key differences between SOA and microservices?

The key differences between SOA and microservices are as follows:

[SOA VS MSA](Interview%20Questions%204b38b3e3e29b48d2ac1a84891042c2c9/SOA%20VS%20MSA%20edc73cf0ad484b20a3321e87759fba29.csv)

## What are the characteristics of microservices?

You can list down the characteristics of microservices as follows:

![https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/06/microservices.png](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/06/microservices.png)

## What is DDD? (Domain Driven Design)

![Interview%20Questions%204b38b3e3e29b48d2ac1a84891042c2c9/Untitled.png](Interview%20Questions%204b38b3e3e29b48d2ac1a84891042c2c9/Untitled.png)

## Why there is a need for DDD? (Domain-Driven Design)

![Interview%20Questions%204b38b3e3e29b48d2ac1a84891042c2c9/Untitled%201.png](Interview%20Questions%204b38b3e3e29b48d2ac1a84891042c2c9/Untitled%201.png)

## What is Ubiquitous Language?

If you have to define the **Ubiquitous Language (UL)**, then it is a common language used by developers and users of a specific domain through which the domain can be explained easily.

The ubiquitous language has to be crystal clear so that it brings all the team members on the same page and also translates in such a way that a machine can understand.

## What is cohesion?

The degree to which the elements inside a module belong together is said to be cohesion.

## What is coupling?

The measure of the strength of the dependencies between components is said to be coupling. A good design is always said to have High Cohesion and Low Coupling.

# References

[Top Microservices Interview Questions and Answers | Edureka](https://www.edureka.co/blog/interview-questions/microservices-interview-questions/)