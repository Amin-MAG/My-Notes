# CAP Theorem

CAP Theorem tells us that in any distributed data store, we can only guarantee two of these three properties.

## Consistency

It means that every read gets to most recent write or returns an error.

## Availability

It ensures that every request gets a response even if it is not the latest.

## Partition Tolerance

It means the systems stays operational even when there are network faults. Consider a scenario that some servers can not communicate with each other due to the network issue. There are 2 ways to handle this problem; If it prioritizes consistency, Some users might get an error due to this problem.If it prioritizes the availability, every request will get a response, even if the response is not the latest data.
