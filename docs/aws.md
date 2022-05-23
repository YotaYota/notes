# AWS

## IAM

An **IAM permission** contains three elements

- _Effect_: ("Allow" | "Deny")
- _Action_: what action `<service namespace>:<action>`
- _Resource_: ARN(s)

(it may also optionally include _Condition_.)

**IAM permissions** are attached to *users* or *roles*.

**Note**: IAM roles do not have long term credentials, ie no password access.
They are assumed to be used by trusted entities.

## Compute

### EC2

Instance families:

- general purpose
- compute optimized
- memory optimized
- accelerated computing
- storage optimized

#### Scaling

Autoscaling additional instances.

Elastic Load Balancing (ELB) routes traffic.

### Lambda

For less than 15 minutes.

## Distribute

### ELB

Elastic Load Balancing 

## Message

### SQS

Simple Queue Service.

Send, store, and receive messages between software components, without losing
messages or requiring other services to be available.

### SNS

Simple Notification Service.

Publish/subscribe model. Publish to a topic. Subscribe to topics.

## Container Orchestration Tools

Docker containers.

### ECS

Elastic Container Service.

### EKS

Elastic Kubernetes Service.

### Fargate

Run containers on top of serverless compute platform for ECS or EKS.

