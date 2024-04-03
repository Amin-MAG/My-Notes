# Concurrency

## Classic Problems
### The Sleeping Barber## Concurrency Patterns### Worker Poollimit the total CPU usageWe can create as many thread or routines as we want, but We should consider the bottleneck in the real world. For instance, in computing, - When a network can only handle a certain number of outbound connection- When a website can only handle a certain number of simultaneous connectionAnd that's why we use worker pools.### Pipeline