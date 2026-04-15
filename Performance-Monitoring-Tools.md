---
title: Performance Monitoring Tools
tags:
  - linux
  - performance
  - monitoring
  - diagnostics
---
# System Performance & Load Diagnostics

Understanding system load, CPU usage, and I/O wait — and critically, how to tell whether a performance problem is compute-bound or I/O-bound. This distinction changes everything about how you fix it.

---

## top — The Starting Point

`top` gives a live view of system resource consumption. It should be the first command you run when a system feels slow.

### Usage

```bash
top -bn1 | head -10   # single snapshot, non-interactive
```

### Output breakdown

```
top - 01:48:03 up 22:40, 3 users, load average: 14.78, 15.50, 12.03
Tasks: 318 total, 2 running, 316 sleeping, 0 stopped, 0 zombie
%Cpu(s): 25.0 us, 7.9 sy, 0.0 ni, 17.1 id, 46.1 wa, 0.0 hi, 3.9 si, 0.0 st
MiB Mem:  3795.7 total, 111.2 free, 2735.5 used, 1156.3 buff/cache
MiB Swap: 2048.0 total, 1861.8 free, 186.2 used, 1060.3 avail Mem
```

---

## Load Average: The Key Metric

### What it measures

Load average counts the number of processes in one of two states at any given moment:

**Running** — actively executing on a CPU core right now.

**Runnable** — ready to run, but no CPU core is free. Waiting in the scheduler queue.

**Uninterruptible sleep (D state)** — waiting for I/O to complete. Cannot be interrupted even by signals. This is the critical one for storage diagnostics.

The three numbers represent exponentially weighted moving averages over the last 1 minute, 5 minutes, and 15 minutes:

```
load average: 14.78, 15.50, 12.03
               ↑        ↑       ↑
             1 min   5 min  15 min
```

### Interpreting relative to CPU count

Load average is only meaningful relative to the number of CPU cores. A load of 4.0 means completely different things on a 1-core vs 16-core machine.

```bash
nproc   # check CPU count
```

| Load / CPU count | Meaning |
|---|---|
| < 1.0 | Well under capacity — cores idle |
| = 1.0 | Exactly saturated — no headroom |
| 1.0–2.0 | Moderately overloaded — some queuing |
| > 2.0 | Significantly overloaded — degraded performance |
| > 3.5 (on 4 cores) = 14 absolute | Severely overloaded — your rasp-2 situation |

### Reading the trend

**Critical:** the three numbers are always displayed as `1min, 5min, 15min` — most recent first, oldest last. This is the opposite of how you might intuitively read them as a timeline.

```
load average: 14.78,  15.50,  12.03
                ↑        ↑       ↑
              NOW     5 min   15 min
                              ago     ago
```

Because the most recent value is on the left and the oldest is on the right, you read the trend **right to left** to understand what happened over time:

```
load average: 14.78, 15.50, 12.03
                              ↑
                        was 12 fifteen minutes ago
                        peaked at 15.50 five minutes ago
                        now at 14.78 — slowly recovering
```

| top output | What it means over time | Trend |
|------------|------------------------|-------|
| `14, 8, 2` | Was 2 (15min ago), rose to 8 (5min ago), now 14 | 🔴 **Worsening** — escalating now, find the cause immediately |
| `2, 8, 14` | Was 14 (15min ago), dropped to 8 (5min ago), now 2 | ✅ **Recovering** — peak has passed, monitor |
| `14, 15, 12` | Was 12 (15min ago), peaked at 15 (5min ago), now 14 | 🟠 **Stable high** — sustained overload, structural problem |

The common mistake is reading left-to-right as a timeline and getting the trend backwards. Always remember: **left = now, right = past**.

**Real example from rasp-2:**

```
load average: 14.78, 15.50, 12.03
```

On a 4-core Raspberry Pi 4, this is 3.7x overload. The system was slightly worse 5 minutes ago (15.50) — it peaked and is very slowly recovering. This is a sustained structural problem, not a temporary spike.

---
---

# See Also

- [I/O Monitoring Tools](IO-MONITORING-TOOLS.md) — iostat, iotop, and storage diagnostics
- [Network Monitoring Tools](NETWORK-MONITORING-TOOLS.md)
- [Kubernetes](Kubernetes.md)
- [Longhorn](Longhorn.md)
