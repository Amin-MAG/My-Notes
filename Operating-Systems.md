# Operating System

## CPU Throttling

CPU Throttling is the process of intentionally reducing the performance of the CPU for several reasons; Power consumption, heat dissipation, or protecting it from overheating.

```bash
# Checking the number of throttling in the system
dmesg | grep -i throttling
```

## CPU Bound

A CPU-bound task is a task that primarily depends on the processing power of the CPU. This type of task requires a significant amount of CPU time to complete, and its performance is limited by the speed of the CPU; Mathematical computations, encryption and decryption, and video rendering.

## I/O Bound

An I/O-bound task is a task that primarily depends on input/output (I/O) operations. This type of task requires a significant amount of time waiting for I/O operations to complete, and its performance is limited by the speed of the I/O system; Reading from or writing to a disk, network, or other external devices.

### Not use thread pools for I/O Bound Workloads

Using a fixed-size goroutine pool for I/O bound workloads can lead to over-provisioning of goroutines, which can result in unnecessary memory and CPU usage. Additionally, if the pool size is too small, it can lead to underutilization of resources and increased latency.

- [Linux](Linux.md)
- [Memory Management](Operating-Systems/Memory%20Management.md)
- [Building the kernel](Operating-Systems/Building%20The%20Kernel.md)
- [Mpich](Operating-Systems/Mpitch.md)
- [Latency Numbers](Operating-Systems/Latency-Numbers)