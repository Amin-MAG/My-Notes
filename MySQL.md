---
title: MySQL
draft: true
tags: [databases, backend, reference]
---
# MySQL

## Connection

```bash
mysql -u <USERNAME> -h <HOST> -P <PORT> -p
```

# See Also

- [Databases](Databases.md)
- [PostgreSQL](PostgreSQL.md)
- [SQLite3](SQLite3.md)
## Dump Data

To dump the data of a specific table

```bash
mysqldump -u admin -p streamline_db dosing_data > output_file.sql
```
