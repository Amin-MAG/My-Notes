# ACID 

ACID Stands for A(Atomicity), C(Consistency), I(Isolation), D(Durability) that ensure reliable database transactions, event when some things go wrong.

## Atomicity 

All transactions are all or nothing. If a part of transaction failed, the whole thing gets rolled back like it never happened.

> **Tip:** Transaction management systems often use logging to enable rollback feature.

## Consistency 

Consistency means that a transaction must follow all the rules and leave the database in a good state. It means all constraints, triggers, and other rules will always be enforced. 

> **Tip:** Keep in mind that Consistency make databases harder to scale.

## Isolatio

Isolation is all about how concurrent transactions interact with each other. Different kind of transactions will not interfere with each other.

### Need to ream more about 

- Dirty Read
- Non-repeatable Read
- Phantom Read

## Durability

They offer durability because the data is stored on the disk. Also, In distributed databases, durability also means replicating data across multiple nodes. So if you loose one node, you do not lose any committed transactions.
