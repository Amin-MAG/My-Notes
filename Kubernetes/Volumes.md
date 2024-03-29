# Volumes

On-disk files in a container are ephemeral, which can cause problems. Two main issues are:

1. Loss of files occurs when a container crashes.
2. Can't share files between containers running together in a pod.

The volumes actually fix both problems.

There are some ephemeral and persistent volumes. You can mount the volumes to a pod or a container and use it.

# Types of volumes

## Config Map

A ConfigMap provides a way to inject configuration data into pods. The data stored in a ConfigMap can be referenced in a volume of type configMap and then consumed by containerized applications running in a pod.

Configuration data can be consumed in pods in a variety of ways. A **`ConfigMap`** can be used to:

1. Populate the value of environment variables.
2. Set command-line arguments in a container.
3. Populate configuration files in a volume.

Both users and system components may store configuration data in a **`ConfigMap`**.

## Empty Dir

An `emptyDir` volume is first created when a Pod is assigned to a node, and exists as long as that Pod is running on that node. As the name says, the `emptyDir` volume is initially empty. All containers in the Pod can read and write the same files in the `emptyDir` volume, though that volume can be mounted at the same or different paths in each container. When a Pod is removed from a node for any reason, the data in the `emptyDir` is deleted permanently.

> Note: A container crashing does not remove a Pod from a node. The data in an emptyDir volume is safe across container crashes.
> 

Some uses for an `emptyDir` are:

- scratch space, such as for a disk-based merge sort
- checkpointing a long computation for recovery from crashes
- holding files that a content-manager container fetches while a webserver container serves the data

Depending on your environment, `emptyDir` volumes are stored on whatever medium that backs the node such as disk or SSD, or network storage. However, if you set the `emptyDir.medium` field to `"Memory"`, Kubernetes mounts a `tmpfs` (RAM-backed filesystem) for you instead. While tmpfs is very fast, be aware that unlike disks, `tmpfs`is cleared on node reboot and any files you write count against your container's memory limit.

> Note: If the SizeMemoryBackedVolumes feature gate is enabled, you can specify a size for memory backed volumes. If no size is specified, memory backed volumes are sized to 50% of the memory on a Linux host.
> 

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: k8s.gcr.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /cache
      name: cache-volume
  volumes:
  - name: cache-volume
    emptyDir: {}
```

## Persistent volume claim

A persistentVolumeClaim volume is used to mount a PersistentVolume into a Pod. PersistentVolumeClaims are a way for users to "claim" durable storage (such as a GCE PersistentDisk or an iSCSI volume) without knowing the details of the particular cloud environment.

## Secrets

A `secret` volume is used to pass sensitive information, such as passwords to Pods. You can store secrets in the Kubernetes API and mount them as files for use by pods without coupling to Kubernetes directly. `secret` volumes are backed by `tmpfs` (a RAM-backed filesystem) so they are never written to non-volatile storage.

> Note: You must create a Secret in the Kubernetes API before you can use it.
> 

> Note: A container using a Secret as a subPath volume mount will not receive Secret updates.
> 

# References

[Volumes](https://kubernetes.io/docs/concepts/storage/volumes/)