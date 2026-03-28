---
title: ACID
draft: true
tags: [databases, distributed-systems, reference]
---
# ACID

ACID stands for Atomicity, Consistency, Isolation, and Durability. These properties ensure reliable database transactions, even when something goes wrong.

## Atomicity 

All transactions are all or nothing. If a part of transaction failed, the whole thing gets rolled back like it never happened.

> **Tip:** Transaction management systems often use logging to enable rollback feature.

## Consistency 

Consistency means that a transaction must follow all the rules and leave the database in a good state. It means all constraints, triggers, and other rules will always be enforced. 

> **Tip:** Keep in mind that Consistency makes databases harder to scale.

## Isolation

Isolation is all about how concurrent transactions interact with each other. Different kinds of transactions will not interfere with each other.

### Need to read more about

- Dirty Read
- Non-repeatable Read
- Phantom Read

## Durability

They offer durability because the data is stored on the disk. Also, In distributed databases, durability also means replicating data across multiple nodes. So if you lose one node, you do not lose any committed transactions.
