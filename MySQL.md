# MySQL

## Connection

```bash
mysql -u <USERNAME> -h <HOST> -P <PORT> -p
```

## Dump Data

To dump the data of a specific table

```bash
mysqldump -u admin -p streamline_db dosing_data > output_file.sql
```
