# Kubectl

## Get started

```bash
# Installation on mac
brew install kubectl

# To check the command
kubectl version --client
```

## `kubectl` Configuration

In order for `kubectl` to find and access a Kubernetes cluster, it needs a [kubeconfig file](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/), which is created automatically when you create a cluster using [kube-up.sh](https://github.com/kubernetes/kubernetes/blob/master/cluster/kube-up.sh) or successfully deploy a Minikube cluster. By default, `kubectl` configuration is located at `~/.kube/config`.

## Create a cluster using MiniCube

**Kubernetes coordinates a highly available cluster of computers that are connected to work as a single unit.**

**Kubernetes automates the distribution and scheduling of application containers across a cluster in a more efficient way.**

A Kubernetes cluster consists of two types of resources:

- The **Control Plane** coordinates the cluster: **is responsible for managing the cluster.**
- **Nodes** are the workers that run applications: **A node is a VM or a physical computer that serves as a worker machine in a Kubernetes cluster.** Each node has a Kubelet, which is an agent for managing the node and communicating with the Kubernetes control plane.

```bash
# Install minikube 
brew update
# hyperkit, I used hyper kit, install one of these
brew install hyperkit
# You need virtual box before
brew install --cask virtualbox
brew install minikube

# Create cluster
# Checking minikube tool
minikube version

# Start the cluster
# Using hyperkit 
minikube start --vm-driver=hyperkit

# Stop cluster
minikube stop
# Delete the whole VM 
minikube delete
```

To view cluster details

```bash
kubectl cluster-info
```

The commands are much like open shift commands `oc`.

```bash
# To view the nodes in the cluster,
kubectl get nodes

# kubectl get - list resources
# kubectl describe - show detailed information about a resource
# kubectl logs - print the logs from a container in a pod
# kubectl exec - execute a command on a container in a pod

# To get everything
kubectl get all
```

To get a shell access

```bash
kubectl exec --stdin --tty hello-node-7567d9fdc9-zkcb9 -- /bin/bash
```

## Create  Deployment

The Deployment instructs Kubernetes how to create and update instances of your application.

Once the application instances are created, a Kubernetes Deployment Controller continuously monitors those instances. If the Node hosting an instance goes down or is deleted, the Deployment controller replaces the instance with an instance on another Node in the cluster. **This provides a self-healing mechanism to address machine failure or maintenance.**

Kubectl uses the Kubernetes API to interact with the cluster.

```bash
# To start the deployment 
kubectl create deployment hello-node  --image=k8s.gcr.io/echoserver:1.4

# To get the pods and deployments
kubectl get pods
kubectl get deployments

# To delete the deployment
kubectl delete deployment hello-node
```

To get events in terminal

```bash
kubectl get events
```

To wait for pods to get ready you can use `-w`:

```bash
kubectl get pod -n argocd -w
```

## Create Service

Services can be exposed in different ways by specifying a `type` in the ServiceSpec:

- *ClusterIP* (default) - Exposes the Service on an internal IP in the cluster. This type makes the Service only reachable from within the cluster.
- *NodePort* - Exposes the Service on the same port of each selected Node in the cluster using NAT. Makes a Service accessible from outside the cluster using `<NodeIP>:<NodePort>`. Superset of ClusterIP.
- *LoadBalancer* - Creates an external load balancer in the current cloud (if supported) and assigns a fixed, external IP to the Service. Superset of NodePort.
- *ExternalName* - Maps the Service to the contents of the `externalName` field (e.g. `foo.bar.example.com`), by returning a `CNAME` record with its value. No proxying of any kind is set up. This type requires v1.7 or higher of `kube-dns`, or CoreDNS version 0.0.8 or higher.

```bash
# Create a new service
# The --type=LoadBalancer flag indicates that you want to expose your Service outside of the cluster.
kubectl expose deployment hello-node --type=LoadBalancer --port=8080

# To show current services
kubectl get services

# To delete service
kubectl delete service hello-node
```

`minikube` can make the service accessible without routes.

```bash
minikube service hello-node
```

Services match a set of Pods using [labels and selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels), a grouping primitive that allows logical operation on objects in Kubernetes. Labels are key/value pairs attached to objects and can be used in any number of ways:

- Designate objects for development, test, and production
- Embed version tags
- Classify an object using tags

![Untitled](Kubernetes/Kubectl%2015099/Untitled.png)

## Port Forwarding

```bash
kubectl port-forward -n argocd svc argocd-server 8080:443
```

## Edit Configuration on cluster

```bash
kubectl edit deployment -n myapp myapp
```