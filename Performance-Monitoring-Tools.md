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
