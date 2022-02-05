# Orchestrating Serverless Workflows

Server-less computing is a new paradigm in cloud computing that abstracts away infrastructure management tasks like load-balancing, scaling from tenants. This makes the users to focus solely on the application development.

# FaaS

Serverless Function as a Service (FaaS) is becoming a very popular programming model in the cloud thanks to its simplicity, billing model and inherent elasticity.

The FaaS model has also proven ideally suited for executing embarrassingly parallel computing
tasks. But both PyWren and ExCamera required their own ad-hoc external orchestration services to synchronize the parallel executions of functions.

Faas is based on event-driven programming.

# Trigger-flow

We present Trigger-flow, a novel building block for composing event-based services. As more applications are moved to the Cloud, this service will enable to control the life-cycle of those applications in a reactive and extensible way.

Major Contributions of this paper is to

1. Present a rich Trigger framework: The composite event detection and event routing mechanisms are mediated by reactive event-based middleware.
2. Demonstrate Trigger-flow’s extensibility and universality.
3. Propose a generic implementation

## Architecture

This feature is representing the Trigger-flow architecture in the article.

![Untitled](Orchestrating%20Serverless%20Workflows%204f2496a943004f16af85c2ab5e8a2cf3/Untitled.png)

Goals in new architecture

- Supports for heterogeneous workflows
- Extensibility and Computational Reflection
- Serverless Design
- Performance

### Trigger Service

It follows the ECA (`Event Condition Action`) architecture in which triggers define which actions should be executed in response to specific events or condition evaluation.

**Workflow**: It’s an FSM a 6-tuple. 

```python
workflow = (
	set_of_input_events,
	set_of_context_variables,
	set_of_states_that_maps_to_actions_based_on_ECA_model,
	initial_state,
	final_state,
	state_transition_functions,
)
```

**Trigger**: can be defined as state transition function. It has a 4-tuple structure.

```python
trigger = (
	events,
	context,
	condition,
	action,
)
```

The events have subject and type fields that by use of those we can match an event to its trigger. Subject fields is used to match to its corresponding trigger and type represent the type of event (like being failure or success).

The context is a key-value data structure that contains the states of the triggers.

The conditions are active rules. Based on these rule it will decide to launch corresponding action or not. These condition evaluate rules over primitive or composite (like counters stored in context) events.

The action is the user-defined code. It can be serverless function, container VM, or container.

## Prototype implementation

Components of the prototype implementation:

- Front-end RESTful API, where a user can interact with Triggerflow
- Database, responsible for storing workflow information
- Controller, responsible for creating workflows in kubernetes
- Workflow Workers (TF-Worker), responsible for processing the events

There were 2 development, one of the was over Knative and the other one used K8s event-driven Autoscaling (KEDA).

Knative is very well suited for push-based scaling. KEDA is the option for having push-based scaling.

### KEDA Implementation

In this case, the Trigger-flow Controller integrates KEDA for the monitoring of Event Sources and for launching the appropriate TF-Workers, and scaling them to zero when necessary.

The advantage here is that, unlike in Knative Eventing, our TF-Workers are connected directly to the Message Broker (Kafa, Redis Streams) using the native protocol of the broker.

![Untitled](Orchestrating%20Serverless%20Workflows%204f2496a943004f16af85c2ab5e8a2cf3/Untitled%201.png)

Figure 2 shows a high-level perspective of our implementation using KEDA. In this deployment, Trigger-flow works as follows:

Through the client, a user must firstly create an empty workflow to the Trigger-flow registry, and reference an event source that this workflow will use. Then, the user can start adding triggers to it (1). 

All the information is persisted in the database (for example, Redis) (2). 

Then, immediately after creating the workflow, the front-end API communicates with the Trigger-flow controller (3), deployed as a single stateless pod container (service) in Kubernetes, to create the auto-scalable TF-Worker in KEDA (4). 

From this moment, KEDA is responsible to scale up and down the TF-Workers (5). 

In KEDA, as stated above, the TF-Worker is responsible for communicating directly to the event source (6) to pull the incoming events. Finally,

TF-Workers periodically interact with the database (7) to keep the local cache of available triggers updated, and to store the context (checkpointing) for fault-tolerance purposes.

## Use cases

### Directed Acyclic Graphs - DAGs

In this graph, each vertex represents a single the tasks of the workflow and the edges represents the dependencies between those tasks.

Obviously, you need to know what tasks have to be executed before a certain one.

> Apache Airflow is an example of orchestration platform that uses DAGs
> 

To orchestrate a workflow defined as a DAG with triggers, we will define a trigger for every edge of the DAG:

- Activation Events: To Register the task IDs needs to be completed. (Dependencies)
- Condition: To count number of events needs to be aggregated.
- Action: To Register the actual task

According to Airflow’s core ideas, an Operator describes what is the actual work logic that is carried out by a task.

![Untitled](Orchestrating%20Serverless%20Workflows%204f2496a943004f16af85c2ab5e8a2cf3/Untitled%202.png)

### State Machines & Nested Workflows

Amazon uses state machines to represent the workflows. The state machines are defined by a declarative JSON object called ASL. Like the previous use case each state needs to know what is the next step.

There will be a trigger for each one of these state transitions.

Kinds of states in this State Machine:

- **Task** or **Pass** state: ****These state types carry out the actual workflow computational logic.
- **Choice** state:  ****It defines a set of possible outcomes that execute depending on some basic boolean logic.
- **Parallel** state:  ****It defines a set of sub-state machines that run in parallel.
- **Map** state:  ****Similarly to the Parallel state type, this state defines a single sub-state machine that executes for every element in an iterable data structure input in parallel.
- **Wait** state:  ****It waits for a range of time or until a specific point of time.
- **Fail** or **Success** state: ****This will end the workflow with and determines that the workflow completed successfully or with a failure.

![Untitled](Orchestrating%20Serverless%20Workflows%204f2496a943004f16af85c2ab5e8a2cf3/Untitled%203.png)

### Workflow as Code & Event Sourcing

> PyWren and Azure Durable Functions are the examples of this section
> 

You can handle the task by simple a code.

```python
import pywren_ibm_cloud as pywren

def my_function (x):
	return x + 7

pw = pywren. ibm_cf_executor ()
future = pw._call_async(my_function, 3)
res = future.result()
futures = pw.map(my_function, range(res))
print(pywren.get_result(futures))
```

In this article, they used even sourcing in 2 ways to ensuring restarting and continuing this PyWren code. 

1. Native Scheduler
2. External Scheduler

![Untitled](Orchestrating%20Serverless%20Workflows%204f2496a943004f16af85c2ab5e8a2cf3/Untitled%204.png)

## Validation

Validations are consisted of load-testing, auto-scaling, completion time and overhead, and scientific workflows.

## Keywords to search for it

- PyWren
- ExCamera
- ECA: Event condition action
- Event composite event detection
- veteran Active Database Systems: Norman W Paton and Oscar D´ıaz. 1999. Active database systems. ACM Computing Surveys (CSUR) 31, 1 (1999), 63–103
- Apache OpenWhisk
- IBM Composer service
- Amazon Step Functions, Amazon Step Functions Express
- Microsoft’s Azure Durable Functions
- Google Cloud Composer
- NATS and Apache kafka
- Kubernetes Event-driven Autoscaling (KEDA)

# OSCAR

> Server-less Workflows for Containerized Applications in the Cloud Continuum
> 

This article will introduce a function definition language to create the functions together with its relationship with data-driven server-less computing.

On of main purposes of this paper is to define new OSCAR framework.

On-premises cloud infrastructure would be hardware that is related to cloud services or activities, that is nonetheless located on-site at the client’s physical business location.

OSCAR is the component of the system that is in charge of creating a function together with the required resources. It is going to support event-driven batch-based GPU-aware executions on the top of K8s cluster for scientific computing.

The users are going to upload a file on an Object storage like MinIO which will trigger the execution of the function. So they are the source of events for this workflow.

## Architecture of OSCAR

![Untitled](Orchestrating%20Serverless%20Workflows%204f2496a943004f16af85c2ab5e8a2cf3/Untitled%205.png)

The clusters become autonomous in deciding whether to scale out due to CLUES elasticity manager.

To aim asynchronous executions, OSCAR creates a Kubernetes job for each asynchronous invocation that are delegated into the Kubernetes workload scheduler for efficient execution.

### Security

For security, Lambda functions use pre-defined IAM (Identity and Access Management) Roles6 that follow the Principle of Least Privilege (PoLP) so that they can
only access the resources required.

The OSCAR cluster itself need a basic authentication.

## Function Definition Language

This article suggests defining a YAML-based Functions Definition Language (FDL) that specifies the requirements for each function and how they are linked.

Using docker images beside this YAML-based file powers us to use different kinds of OS and distributions and providing complex environments.

![Untitled](Orchestrating%20Serverless%20Workflows%204f2496a943004f16af85c2ab5e8a2cf3/Untitled%206.png)

They use a general form to run the applications. Instead of having a Programming Language they deal with bash files that makes us to run any kind of application that is supported by command line in that container. 

![Untitled](Orchestrating%20Serverless%20Workflows%204f2496a943004f16af85c2ab5e8a2cf3/Untitled%207.png)

As you can see, you can focus on the definition of the workflow and let the cluster auto-scale within the on-premises cloud.

## Use Case - Deep Learning Video Processing

A mask-wearing detection in COVID-19 global pandemic via deep learning vide processing.

![Untitled](Orchestrating%20Serverless%20Workflows%204f2496a943004f16af85c2ab5e8a2cf3/Untitled%208.png)

Data is captured at the edge (camera devices), pre-processing is carried out in an on-premises Cloud (to blur the faces) for regulatory compliance purposes and, finally, processing and storing of final results is carried out in a public Cloud using a server-less platform for increased elasticity and long-term persistence.

![Untitled](Orchestrating%20Serverless%20Workflows%204f2496a943004f16af85c2ab5e8a2cf3/Untitled%209.png)

> Case study is not noted yet.
> 

## Keywords to search for it

- SCAR and OSCAR Open source project
- Minicon
- OpenFaaS
- Define workflow with SCAR
- Blurry faces tool

# SWEEP

> Evaluation of server-less computing for scalable execution of a joint variant calling
workflow
> 

## Workflow Definition

SWEEP workflows are represented as Directed Acyclic Graphs (DAGs), where the nodes correspond to tasks, and the arrows indicate order of execution.

The task units can be constructed by a function or a container. These workflows are going to be defined in some JSON based files.  

![Untitled](Orchestrating%20Serverless%20Workflows%204f2496a943004f16af85c2ab5e8a2cf3/Untitled%2010.png)

# Securing Function Workflows - VALVE

It presents `Valve` a server-less platform that enables dynamic information flow tracking and control in distributed function workflows.

# References

[Triggerflow | Proceedings of the 14th ACM International Conference on Distributed and Event-based Systems](https://dl.acm.org/doi/10.1145/3401025.3401731)

[https://github.com/triggerflow/triggerflow](https://github.com/triggerflow/triggerflow)

[What is an On-Premises Cloud Infrastructure? - Definition from Techopedia](https://www.techopedia.com/definition/32287/on-premises-cloud-infrastructure)