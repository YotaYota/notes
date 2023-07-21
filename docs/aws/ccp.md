# AWS Certified Cloud Practitioner Foundational (CLF-C01)

## Module 1: Intro

Bird's eye view of AWS.

4 domains:
- 26% Domain 1: Cloud Concepts
- 25% Domain 2: Security and Compliance
- 33% Domain 3: Technology
- 15% Domain 4: Billing and Pricing

Exam:
- Passing ~700/1000
- 65 questions (50 scored, 15 unscored)
- 1.5 hours
- Valid for 36 months

**Client Server Model**: a client makes a request to a server.

### Cloud Computing

**Cloud Computing**: The on-demand delivery of IT resources iver the internet
with pay-as-you-go pricing.

- Trade upfront expenses for variable expenses
- Don't pay for data center maintenance
- Don't need to guess capacity
- Benefit from economies of scale
- Flexibility increases speed and agility
- Easy to go global

---

## Module 2: Compute in the Cloud

### EC2

Multitenancy - Hypervisor - VMs.

Supports vertical scaling.

Instance family types to optimize either memory, cpu, storage or networking
capacity.

- *General purpose*, `M` and `T`
- *Compute optimized* (eg batch processing), `C`
- *Memory optimized*, `R` and `X`
- *Accelerated computing* (utilize hardware accelerators; coprocessors), `P` and `G`
- *Storage optimized* (> 10k IOPS), `I` and `D`
- *HPC optimized*, `Hp`

#### Pricing

- *On-demand* (per hour or per second)
- *Savings plan* (usage above committed usage is charged on-demand prices. 1 or 3 year term)
- *Reserved instances* (1 or 3 year term, for steady state workload)
- *Spot instances* (can be reclaimed with 2 min warning, should be able to handle interruptions)
- *Dedicated host* (no shared tenancy)

### Scalability and Elasticity

**Scaling**

Vertical scaling: *scale up* / *scale down*

Horizontal scaling: *scale in* / *scale out*.

*Amazon EC2 Auto Scaling* provides automatic  horizontal scaling.
- *Dynamic scaling* responds to changing demand
- *Predictive scaling* schedule

**Load Balancing**

*Elastic Load Balancing, (ELB)*
- Regional construct (highly available)
- Distributes traffic across multiple resources
- Scales automatically

### Messaging and Queueing

If services communicate directly, that is a *Tightly Coupled Architecture*; errors
in application B will propagate to application A. *Loosely Coupled Architecture*
means that single failures does not cause cascading failures. Queues and buffers
solve this.

*Simple Queue Service (SQS)*
- Send/Recieve
- Store
- Supports any volume
- Scale automatically

*Simple Notification Service (SNS)*
- Publish/Subscibe model (SNS Topic)

#### Monoliths and Microservices

- Applications consists of Components
- Components need to communicate
- Monoliths promote tight coupling
- Microservices are loosely coupled by design

### Additional Compute Services

*Serverless*: You cannot see or access the underlying infrastructure.

**Lambda**

- Scale automatically
- <15 min
- zip or Docker container

**Container Orchestration Tools**

Container (Docker) Services.
- Elastic Container Service (ECS)
- Elastic Kubernetes Service (EKS)

The host is an EC2 instance.

*Fargate*: Serverless compute platform for ECS or EKS, allows to run containers
on top of a Serverless compute platform. An alternative to running containers
directly in an EC2 machine.

