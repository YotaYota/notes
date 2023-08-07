# AWS Certified Cloud Practitioner Foundational (CLF-C01)

## Order of Certificates

1. AWS Certified Cloud Practitioner Foundational (CCP)
2. AWS Certified Solutions Architect Associate (CSAA)
   **most in demand**
3. AWS Certified Developer Associate
   **overlaps CSAA**
4. AWS Sysops Administrator Associate

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

- _General purpose_, `M` and `T`
- _Compute optimized_ (eg batch processing), `C`
- _Memory optimized_, `R` and `X`
- _Accelerated computing_ (utilize hardware accelerators; coprocessors), `P` and `G`
- _Storage optimized_ (> 10k IOPS), `I` and `D`
- _HPC optimized_, `Hp`

#### Pricing

- _On-demand_ (per hour or per second)
- _Savings plan_ (usage above committed usage is charged on-demand prices. 1 or 3 year term)
- _Reserved instances_ (1 or 3 year term, for steady state workload)
- _Spot instances_ (can be reclaimed with 2 min warning, should be able to handle interruptions)
- _Dedicated host_ (no shared tenancy)

### Scalability and Elasticity

**Scaling**

Vertical scaling: _scale up_ / _scale down_

Horizontal scaling: _scale in_ / _scale out_.

_Amazon EC2 Auto Scaling_ provides automatic horizontal scaling.

- _Dynamic scaling_ responds to changing demand
- _Predictive scaling_ schedule

**Load Balancing**

_Elastic Load Balancing, (ELB)_

- Regional construct (highly available)
- Distributes traffic across multiple resources
- Scales automatically

### Messaging and Queueing

If services communicate directly, that is a _Tightly Coupled Architecture_; errors
in application B will propagate to application A. _Loosely Coupled Architecture_
means that single failures does not cause cascading failures. Queues and buffers
solve this.

_Simple Queue Service (SQS)_

- Send/Recieve
- Store
- Supports any volume
- Scale automatically

_Simple Notification Service (SNS)_

- Publish/Subscibe model (SNS Topic)

#### Monoliths and Microservices

- Applications consists of Components
- Components need to communicate
- Monoliths promote tight coupling
- Microservices are loosely coupled by design

### Additional Compute Services

_Serverless_: You cannot see or access the underlying infrastructure.

**Lambda**

- Scale automatically
- <15 min
- zip or Docker container

**Container Orchestration Tools**

Container (Docker) Services.

- Elastic Container Service (ECS)
- Elastic Kubernetes Service (EKS)

The host is an EC2 instance.

_Fargate_: Serverless compute platform for ECS or EKS, allows to run containers
on top of a Serverless compute platform. An alternative to running containers
directly in an EC2 machine.

---

## Module 3: Global Infrastructure and Reliability

Edge Locations are not the same as Regions. Regions can push their content to
several edge locations.

### Regions and Availability Zones

Regions are geographically isolated from one another without an explicit approve.

Regions can be connected with high speed fiber network.

_Availability Zone (AZ)_: One data center inside a region.

**Best Practice**: Run across at least 2 AZs for mitigating effect of downtime.
Any service marked with _Regionally scoped service_ are already doing this by default.

Factors when deciding a region:

- Compliance with data governance and legal requirements
- Proximity
- Feature availability
- Pricing

### Edge Locations

_Content Delivery Network (CDN)_: Cache data close to client.

_Amazon CloudFront_ is the CDN in AWS.

_Route 53_: DNS server.
