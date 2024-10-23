# Introduction of Dynamo DB

## 5 Misconceptions about DynamoDB

1. DynamoDB is not just a key-value data store. You can also create one-to-one, one-to-many, and many-to-many relationships and do complex filtering.
2. DynamoDB was built for massive scale, but it is important to know how to design the schemas.
3. DynamoDB is not only for enormous scale. It has some other advantages in ease of monitoring, maintaining, and management.
4. You can keep working with DynamoDB even if your data model will change.
5. DynamoDB is schema-less, but it does not mean you do not need schema in your application.

## Key Characteristics

DynamoDB is a fully-managed, NoSQL database provided by Amazon Web Services.

- You can use Key-Value or Wide-Column data model.
- Infinite scaling with no performance degradation.
- It uses HTTP connection model.
- It is integrated with AWS IAM.
- It is Infrastructure as code friendly.
- Cost of using DynamoDB is based on Operation(READ and Write).
- DynamoDB has streams built into it.
- It is Fully managed.

## When to use DynamoDB

- Hyper Scale Applications
- Hyper Ephemeral Compute
- Online Transactional Processing Applications (ONTP)

# Core Concepts

## Basic Vocabulary

- **Table**: It is kind of similar with tables in relational databases with some differences.
- **Item**: Each record is called Item, so it is similar to a row in a relational database.
- **Primary Key**: For each item, we need a primary key that uniquely identify that item.
- **Attribute**: The other columns of an Item is called attributes. Keep in mind that with dynamoDB, it is not necessary to define all of the attributes. You just need to define the primary key for table.

> There are multiple types for each attribute: strings, numbers, and complex types like list, maps, and sets.

## Primary Keys

- **Simple Primary Keys (Partition Key)**
- **Composite Primary Keys (Partition Key + Sort Key)**: The combination of these two item should be unique.

## Secondary Index

You can flip the partition key and sort key and query based on the sort key if you have defined a secondary index. It will enable additional access pattern. 

## Item Collection

All items that share a particular partition key are set to be in an item collection. DynamoDB uses this item collection for partitioning and scaling. They are also important in case that you wanted to define a relationship.

## DynamoDB Stream

You can have multiple clients subscribing to the stream and they will be notified about creating, deleting, or updating items from DynamoDB.

- **Aggregation**: Clients can consume that data to do some aggregations and write it back to the table.
- **Analytics**: It is possible to export it to an analytics system.
- **Microservices**
