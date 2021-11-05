# SQLite3

This kind of database is the main database for small devices such as android smartphones.

To create a new database:

```bash
sqlite <database>.db
```

In the `sqlite3` environment:

```sql
# some common and basic commands
.databases
.help
.quit

```

## Import and export

Dump to a file

```bash
sqlite3 testDB.db .dump > testDB.sql
```

Import from an SQL file

```bash
sqlite3 testDB.db < testDB.sql
```