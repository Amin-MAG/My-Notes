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
# References

[Triggerflow | Proceedings of the 14th ACM International Conference on Distributed and Event-based Systems](https://dl.acm.org/doi/10.1145/3401025.3401731)

[https://github.com/triggerflow/triggerflow](https://github.com/triggerflow/triggerflow)