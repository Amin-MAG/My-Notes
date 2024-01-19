# Interviews

## System Design Interview - Step By Step Process

In System design interviews, having a step-by-step framework can help us to manage our time and represent our thought processes. Here is a framework suggested by the ByteByte Go channel.

> **Note**: It's very important to keep in the mind to asked enough questions. Usually, the system design interview questions are vague and open ended. You have to ask questions and understand the importance of each factor of your solution design.

1. **Understand the problem and establish design scope (w=1)**: 
2. **Propose high-level design and get buy-in (w=4)**:
3. **Design Deep Dive (w=3)**: 
4. **Wrap Up (w=1)**: 

> **Note**: It's a red flag to give a solution without understand the exact problem. You need to ask enough questions about the problem given to you.

### Understand the problem

The goal of asking questions is to clarify the problem.

- Why to build such a system?
- Who the users are?
- What features we have to build? What are the functional requirements?
- What are non-functional requirements? 
	- **Performance**
	- **Scale**
	- Security
	- Consistency
	- Freshness
	- Accuracy
- Calculate an estimation of the requests, like amount of data per seconds or Query per Second.

> In the end, you should come up with a list of requirements including functional and non-functional requirements.

### Propose high-level design

#### Design the APIs

We can use a top-down approach and start designing the APIs. Be careful that some of the services need to bidirectionally communicate to users.

#### Lay out a Diagram

Start to create services and connect to to each other in a diagram. 
- Keep in mind that here you should use load balancers. 
- We also specify the data storage layer. 
- You can introduce a communication to CDN in this stage.
- You should create a list of discussion points for later.
- Do not create so much detail before having the full picture of system.

#### Data model and schema

In this step, we need to discuss

- Data Access patterns
- Read/Write ratio
- The database to use
- Indexing options

> Defer any detailed discussion to the "Design Deep Dive" section. For example, topics like sharding.

### Design Deep Dive

> **Note**: Review your design and make sure that you have the full system and features before starting this section.

You should analyze different part of your design and discuss parts that can be problematic. You should be able to demonstrate the trade-off between different approaches. At least, two solutions should be suggested and then, You should discuss about the trade-offs. For each problem, we have these steps to follow,

1. Articulate the problem
2. Come up with at least two solutions
3. Discuss the trade-offs
4. Pick a Solution

# Read more 

- You should read more about how to manage and support different kind of non-functional requirement
- How can we make a stateful service like a service using websocket more scalable?
- 

# Resources

- [System Design Interview: A Step-By-Step Guide](https://www.youtube.com/watch?v=i7twT3x5yv8)