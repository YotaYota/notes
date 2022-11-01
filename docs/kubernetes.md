# Kubernetes

Open source orchestration system for Docker containers.

Handles scheduling onto nodes in a compute cluster.
Actively manages work loads so that state matches the user's declared intentions.

Using the concept of labels and pods it groups the containers which makes up an
application into logical units for easy management and discovery.

Pod: a runnable unit of work. Usually 1 container.

Service: tells what services your application provides.

Volume: represents a location where containers can access and store information.

Namespace: grouping mechanism and isolation from other parts of the cluster.

## Resources

A kubernetes cluster contains 2 types of resources:

- **Control Plane** - coordinates the cluster
- **Nodes**         - Workers that run applications

### Control Plane

Responsible for managing the cluster.

### Node

A VM or physical computer that serves as a worker machine in the cluster.

Has a **Kubelet**: an agent that manages the node and communicates with the
control plane.

## Deploy

Tell control plane to start the application containers. Control plane schedules
the containers to run on the cluster's nodes. Nodes communicate with the control
plane through Kubernetes API.

**Deployment configuration**: instructions on how to create and update instances
of application.

**Kubernetes Deployment Controller** monitors and replaces down or deleted with
instances on another node.

```
kubectl create deployment <deployment name> <app image location>
```

## Pod

A Kubernetes abstraction that represents 

- a group of one or more application containers
- shared resources
    - shared storage
    - networking
    - container information

Containers in a Pod share an IP Address and port space, are always co-located
and co-scheduled, and run in a shared context on the same Node. Pods run in an
isolated, private network.

A pod always runs on a Node.

## Node

A node can contain many pods.

A Node always contains:

- Kubelet: process responsible for communication with Control Plane.
- A container runtime (eg Docker).

## Service

Abstraction which defines a logical set of Pods and a policy by which to access
them.
Services allow your applications to receive traffic. Services can be exposed in
different ways by specifying a `type` in the ServiceSpec.

Services match a set of Pods using **lables** and **selectors**.

## `kubectl` CLI

`kubectl <ACTION> <RESOURCE>`

- `kubectl get` - list resources
- `kubectl describe` - show detailed information about a resource
- `kubectl logs` - print the logs from a container in a pod
- `kubectl exec` - execute a command on a container in a pod
- `kubectl cluster-info`
