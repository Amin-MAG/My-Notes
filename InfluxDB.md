---
title: InfluxDB
draft: true
tags: []
---
# InfluxDB

It’s an Open-Source time series database written in Go. The metrics from Telegraf are going to be stored here.

## Core Concepts

### Bucket

A bucket is like a database or a table. It contains time series data. We can set retention policies for buckets.

### Measurement

A measurement is like table name in RDB. It groups data based on their type.

### Tag

Tags are indexed key-value pairs used for filtering and grouping. They are like indexed columns in RDB.

### Field

Fields are key-value pairs that hold actual numeric or staring data. They are not indexed. So it is better to store metrics in fields, they are bad for filters. There can be multiple fields per measurement.

### Timestamp

Every point in InfluxDB has timestamp. 

### Line Protocol

This is a simple text format used to write data into InfluxDB

```influxdb
measurement,tag_key=tag_value field_key=field_value timestamp
```

for instance

```influxdb
weather,location=montreal temperature=21.5 1640995200
```

### Organization

-InfluxDB v2.x uses the concept of organizations to group buckets and users.

### Flux Language

A functional query language for querying, transforming, and analyzing data. Here is an example

```flux
from(bucket: "mybucket")
  |> range(start: -1h)
  |> filter(fn: (r) => r._measurement == "weather" and r.location == "montreal")
```

### Retention Policy

Retention Policy defines how long data in a bucket is kept. InfluxDB will automatically delete data older than this duration.


| Feature             | Tags                      | Fields                                        |
| ------------------- | ------------------------- | --------------------------------------------- |
| Indexed?            | ✅ Yes                     | ❌ No                                          |
| Query performance   | ✅ Fast filtering          | ⚠️ Slower filtering                           |
| Use in aggregations | ❌ No aggregations on tags | ✅ Used in aggregations like `mean()`, `sum()` |
| Can be numeric?     | ❌ No (always strings)     | ✅ Yes                                         |
### Organization

-InfluxDB v2.x uses the concept of organizations to group buckets and users.

### Flux Language

A functional query language for querying, transforming, and analyzing data. Here is an example

```flux
from(bucket: "mybucket")
  |> range(start: -1h)
  |> filter(fn: (r) => r._measurement == "weather" and r.location == "montreal")
```

### Retention Policy

Retention Policy defines how long data in a bucket is kept. InfluxDB will automatically delete data older than this duration.


| Feature             | Tags                      | Fields                                        |
| ------------------- | ------------------------- | --------------------------------------------- |
| Indexed?            | ✅ Yes                     | ❌ No                                          |
| Query performance   | ✅ Fast filtering          | ⚠️ Slower filtering                           |
| Use in aggregations | ❌ No aggregations on tags | ✅ Used in aggregations like `mean()`, `sum()` |
| Can be numeric?     | ❌ No (always strings)     | ✅ Yes                                         |
