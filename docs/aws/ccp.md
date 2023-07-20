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
- Don't pay for data center maintainence
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

- General purpose, M and T
- Compute optimized (eg batch processing), C
- Memory optimized, R and X
- Accelerated computing (utilize hardware accelerators; coprocessors), P and G
- Storage optimized (> 10k IOPS), I and D
- HPC optimized, Hp

#### Pricing

- On-demand (per hour or per second)
- Savings plan (usage above committed usage is charged on-demand prices. 1 or 3 year term)
- Reserved instances (1 or 3 year term, for steady state workload)
- Spot instances (can be reclaimed with 2 min warning, should be able to handle interruptions)
- Dedicated host (no shared tenancy)

#### Scalability and Elasticity

*Amazon EC2 Auto Scaling* provides automatic scaling.
- *Dynamic scaling* responds to changing demand
- *Predictive scaling* schedule

