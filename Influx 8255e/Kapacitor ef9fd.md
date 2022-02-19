# Kapacitor

After collecting data using Telegraf and then storing these data in InfluxDB, Then you can use Kapacitor to process your data. You can import time series data to transform, analyze or act on them.

Kinds of tasks in kapacitor:

- `stream` tasks. A stream task replicates data written to InfluxDB in Kapacitor. Offloads query overhead and requires Kapacitor to store the data on disk.
- `batch` tasks. A batch task queries and processes data for a specified interval.