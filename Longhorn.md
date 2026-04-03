---
title: Longhorn
tags:
  - kubernetes
  - storage
  - distributed-systems
---

# Longhorn — Cloud-Native Distributed Storage for Kubernetes

## What is Longhorn?

Longhorn is an open-source, cloud-native distributed block storage system built specifically for Kubernetes, originally developed by Rancher (now part of SUSE). It turns the local disks of your cluster nodes into a unified, highly available storage pool — without needing dedicated storage hardware or an external SAN/NAS.

Every volume you create in Longhorn is replicated across multiple nodes, meaning your data survives individual node or disk failures automatically. Longhorn runs entirely inside your Kubernetes cluster as a set of pods and custom resources, managed like any other workload.

It is a CNCF (Cloud Native Computing Foundation) project and is widely used in both production environments and homelabs running k3s, RKE, or vanilla Kubernetes.

---

## Core Concepts

### Volumes

A Longhorn volume is a block device backed by replicas distributed across your nodes. It maps 1:1 to a Kubernetes PersistentVolume (PV) and is consumed by workloads through PersistentVolumeClaims (PVCs).

### Replicas

Each volume is made up of N replicas, where N is your configured replica count (default: 3). Each replica lives on a different node. When a pod writes data, Longhorn writes to all replicas simultaneously. This is what gives Longhorn its fault tolerance — any single node can die and your data is safe on the remaining replicas.

> **Important:** Replicas protect against hardware failure, not logical failure. If you accidentally delete a PVC or a bug corrupts your data, all replicas faithfully replicate that change. Replicas are not a substitute for backups.
---

## Further Reading

- [Longhorn Official Docs](https://longhorn.io/docs/)
- [Longhorn GitHub](https://github.com/longhorn/longhorn)
- [Backup and Restore Guide](https://longhorn.io/docs/latest/snapshots-and-backups/)
- [Recurring Jobs](https://longhorn.io/docs/latest/tasks/recurring-tasks/)

# See Also

- [Kubernetes](Kubernetes.md)
- [Distributed Systems](Distributed-Systems.md)
- [Velero](Velero.md)
- [Cloud Computing](Cloud-Computing.md)