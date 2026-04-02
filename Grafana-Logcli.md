---
title: Grafana Logcli
draft: true
tags: [devops, monitoring, logging, loki, grafana, reference]
---
# LogCLI — Loki Command Line Interface

`logcli` is the official CLI tool for querying Loki directly from your terminal, made by Grafana Labs. It supports the full LogQL query language without needing the Grafana UI.

---

## Installation

```bash
curl -O -L "https://github.com/grafana/loki/releases/latest/download/logcli-linux-amd64.zip"
unzip logcli-linux-amd64.zip
chmod +x logcli-linux-amd64
mv logcli-linux-amd64 /usr/local/bin/logcli
```

---

## Configuration

Point `logcli` at your Loki instance via environment variable:

```bash
export LOKI_ADDR=https://loki.example.com
```

If Loki is not publicly exposed, use port-forwarding:

```bash
kubectl port-forward svc/loki 3100:3100 -n monitoring
export LOKI_ADDR=http://localhost:3100
```

---

## Basic Usage

```bash
# List all available labels
logcli labels

# List values for a specific label
logcli labels app

# Query last 100 lines (most recent first by default)
logcli query '{namespace="default"}' --limit=100

# Query in forward (oldest first) order
logcli query '{namespace="default"}' --limit=100 --forward

# Tail logs live (like tail -f)
logcli query '{namespace="default"}' --tail
```

---

## Time Range Flags

```bash
# Relative (from now)
--since=30m
--since=1h
--since=24h
--since=7d

# Absolute range
--from="2026-03-16T10:00:00Z" --to="2026-03-16T12:00:00Z"

# From a fixed point until now
--from="2026-03-17T00:00:00Z"
```

---

## Output Formatting

```bash
--output=default    # timestamp + labels + message
--output=raw        # message only (clean, readable)
--output=jsonl      # full JSON per line (ideal for piping/parsing)
```

---

## Advanced Flags

### Batching & Performance

```bash
--batch=1000               # lines fetched per API call (default: 1000)
--limit=5000               # total lines to return
--parallel-duration=1h     # split query into parallel time chunks
--parallel-max-workers=4   # number of parallel workers
```

### Tail / Follow

```bash
--tail              # stream new logs live
--delay-for=2       # delay in seconds before tailing (avoids missing late-arriving logs)
```

### Display

```bash
--quiet             # suppress query info and stats
--no-labels         # hide label columns in output
```

---

## LogQL Filtering (Query Level)

These are LogQL expressions, not flags, but essential for effective querying:

```bash
# Multi-label match
'{namespace="bots", app="schotify"}'

# Contains string
'{namespace="bots"} |= "error"'

# Exclude string
'{namespace="bots"} != "debug"'

# Regex match
'{namespace="bots"} |~ "ERR.*timeout"'

# Parse JSON logs then filter by field
'{namespace="bots"} | json | level="error"'
```

---

## Metric Queries

```bash
# Instant metric query
logcli instant-query 'count_over_time({namespace="bots"}[5m])'

# Range metric query with step
logcli range-query 'rate({namespace="bots"}[1m])' --since=1h --step=1m
```

---

## Practical Examples

```bash
# Tail errors from a specific app live
logcli query '{namespace="bots", app="schotify"} |= "error"' --tail --output=raw

# Last 2 hours of logs, pipe to jq
logcli query '{namespace="bots"}' --since=2h --output=jsonl | jq '.message'

# Investigate a specific incident window
logcli query '{namespace="monitoring"} |= "alert"' \
  --from="2026-03-17T08:00:00Z" \
  --to="2026-03-17T09:00:00Z" \
  --limit=2000 --output=raw

# Fuzzy search through logs with fzf
logcli query '{namespace="monitoring"}' --limit=500 --output=raw | fzf
```
