---
title: CAP
draft: true
tags: [distributed-systems, databases, system-design, reference]
---
# CAP Theorem

CAP Theorem tells us that in any distributed data store, we can only guarantee two of these three properties.

## Consistency

It means that every read gets the most recent write or returns an error.

## Availability

It ensures that every request gets a response even if it is not the latest.

## Partition Tolerance

It means the system stays operational even when there are network faults. Consider a scenario where some servers cannot communicate with each other due to a network issue. There are two ways to handle this problem: if it prioritizes consistency, some users might get an error. If it prioritizes availability, every request will get a response, even if the response is not the latest data.

# See Also

- [ACID](ACID.md)
- [Distributed-Systems](Distributed-Systems.md)
- [Databases](Databases.md)
