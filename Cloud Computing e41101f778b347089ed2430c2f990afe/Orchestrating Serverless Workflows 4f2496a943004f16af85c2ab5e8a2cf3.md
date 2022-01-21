# Orchestrating Serverless Workflows

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

# References

[Triggerflow | Proceedings of the 14th ACM International Conference on Distributed and Event-based Systems](https://dl.acm.org/doi/10.1145/3401025.3401731)

[https://github.com/triggerflow/triggerflow](https://github.com/triggerflow/triggerflow)