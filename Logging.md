# Logging
## Beats

Beats are a collection of lightweight (resource efficient, no dependencies, small) and open source log shippers

-   Log files (Filebeat)
-   Network data (Packetbeat)
-   Server metrics (Metricbeat)
-   Heart beat (uptime monitoring)
-   Audit beat (user and process activity on your Linux servers)
-   WinLog beat (Windows Event logs)
-   Function beat (Server-less shipper)
-   Journal beat (Linux service journals)

To install your beat, use:

```bash
sudo apt-get update
sudo apt-get install <beatname>
```

Just like other services the configurations are located in `/etc/<beatname>`, for example, following configuration is in `/etc/filebeat/filebeat.yml`.

```yaml
filebeat.prospectors:
- type: log
  enabled: true
  paths:
    - /var/log/*.log
  fields:
    app_id: service-a
    env: dev
output.logstash:
  hosts: ["localhost:5044"]
```

🌐 [logz.io - Article About logging](https://logz.io/blog/beats-tutorial/)

## Databases

### Logstash

### Fluentd

🌐 [Fluentd-Logging](https://docs.fluentd.org/deployment/logging)

### Click House

ClickHouse is an open-source column-oriented database management system that allows the generation of analytical data reports in real-time.

-   Technology C++
    
-   Store data on a disk.
    
-   Append data to the end of the file when writing.
    
-   Support locks for concurrent data access.
    
    During **`INSERT`** queries, the table is locked, and other queries for reading and writing data both wait for the table to unlock. If there are no data writing queries, any number of data reading queries can be performed concurrently.
    
-   Do not support [mutations](https://clickhouse.com/docs/en/sql-reference/statements/alter/#alter-mutations).
    
-   Do not support indexes.
    
    This means that **`SELECT`** queries for ranges of data are not efficient.
    
-   Do not write data atomically.
    
    You can get a table with corrupted data if something breaks the write operation, for example, an abnormal server shutdown.
    

The **`Log`** and **`StripeLog`** engines support parallel data reading. When reading data, ClickHouse uses multiple threads.

The **`Log`** engine uses a separate file for each column of the table. **`StripeLog`** stores all the data in one file. As a result, the **`StripeLog`** engine uses fewer file descriptors, but the **`Log`** engine provides higher efficiency when reading data.

🌐 [Click House Log Family article](https://clickhouse.com/docs/en/engines/table-engines/log-family/)

There are some Click house integrations with some other services. For example, you can send your logs to a file storage service.

🌐 [Click House - S3 Integration](https://clickhouse.com/docs/en/engines/table-engines/integrations/s3)

### Hbase

HBase is an open-source non-relational distributed database modeled after Google's Bigtable and written in Java.

# See more
- [Fluentd](Fluentd.md)