# SQL

SQL or Structured Query Language consist of several sections.

- DML: Data Manipulation Language
- DDL: Data Definition Language
- DCL: Data Control Language
- TCL: Transaction Control Language

## DML

Data Manipulation Language consists of keywords related to queries or mutating data in database.

### Constraints

- PRIMARY KEY
- FOREIGN KEY
- UNIQUE: Makes sure there is not duplicate value for that field
- CHECK: Enforces some conditions on data
- DEFAULT: Specifies a default value for the column

### Operations

- `SELECT`
- `INSERT`
- `JOIN`
- `DELETE`
- `UPDATE`

### Operators

- Logical Operator: `AND`, `OR`, `NOT`
- Numeric Operator: Addition, Substraction, Multiplication, Division, Modulus 
- String Operator: `Like`, Concatenation

### Functions

- Numeric Functions (Such as `SUM`)
- String Functions (Such as `CONCAT`, `SUBSTRING`)
- Time Functions (Such as `GETDATE`, `DATEADD`)
- Aggregate Functions (`COUNT`, `MIN`, `MAX`) which used by `GROUP BY` and `HAVING`

### Types 

- Numeric
- String
- Date/Time
- Boolean

### Indexes

Indexes allow faster queries by creating a searchable structure, similar to an index in a book. They can also introduce overhead for insert, update, and delete operations.

## DDL 

Data Definition Language contains statements like `CREATE TABLE`, `ALTER TABLE`.

## DCL

Data Control Language manages access permissions using `GRANT` and `REVOKE`.

## TCL 

Transaction Control Language handles transaction management with `COMMIT`, `ROLEBACK`, and `SAVEPOINT`, ensuring data integrity through ACID properties.

# See More

- [MySQL](MySQL.md)
- [PostgreSQL](PostgreSQL.md)
- [SQLite3](SQLite3.md)

# Things I need to know more about

1. Query Optimization
2. Database Normalization
3. Transaction Management