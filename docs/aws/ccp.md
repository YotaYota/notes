# AWS Certified Cloud Practitioner Foundational (CLF-C01)

## Order of Certificates

1. AWS Certified Cloud Practitioner Foundational (CCP)
2. AWS Certified Solutions Architect Associate (CSAA)
   **most in demand**
3. AWS Certified Developer Associate
   **overlaps CSAA**
4. AWS Sysops Administrator Associate

---

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

**Cloud Computing**: The on-demand delivery of IT resources over the internet
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

**AWS Local Zones**: for even more low-latency requirements.

### Regions and Availability Zones

Regions are geographically isolated from one another without an explicit approve.

Regions can be connected with high speed fiber network.

_Availability Zone (AZ)_: One data center inside a region.

**Best Practice**: Run across at least 2 AZs for mitigating effect of downtime.
Any service marked with _Regionally scoped service_ are already doing this by
default (eg ELB, SNS, SQS).

Factors when deciding a region:

- Compliance with data governance and legal requirements
- Proximity
- Feature availability
- Pricing

**AWS Outposts**: Run AWS services locally.

### Edge Locations

_Content Delivery Network (CDN)_: Cache data close to client.

_Amazon CloudFront_ is the CDN in AWS.

_Route 53_: DNS server.

### Provisioning

When interacting with AWS services, everything is an API call.

- AWS Management Console (Browser)
- AWS Command Line Interface (CLI)
- AWS Software Development Kits (SDK)
- Other tools (eg CloudFormation, Elastic Beanstalk)

**AWS Elastic Beanstalk**: Helps provision EC2 based environments.

**AWS CloudFormation**: IaC tool.

---

## Module 4: Networking

### VPC

![overview](img/igw_sg.png)

**Virtual Private Cloud (VPC)**: Provision logically isolated section.
Resources in VPC can be public facing or private.

**Subnets**: Chunks of IP adresses in the VPC that allows to group resources
together. They can be **public** or **private**.

Subnets plus networking rules control whether resources are publicly, or
privately available.

- For public traffic an **Internet Gateway (IGW)** must be attached to allow
  traffic in and out of VPC to the public internet.
- For private traffic **Virtual Private Gateway** is attached using VPN
  (public encrypted traffic to VPC).
- **AWS Direct Connect** physical private line to VPC.

**Note:** A VPC can have many different gateways, but connected to different
Subnets.

Packages entering and leaving a VPC gets checked against a **Network access
control list (Network ACL)**. Network ACL only checks if package can cross a
subnet boundary, and not what it can reach inside the subnet.

**Security Groups** blocks all incoming traffic to a group of resources (by
default), but allows all outgoing traffic. Security groups are stateful in the
sense that they allow responses regardless of SG rules.

**Note:** Security Group is stateful, Network ACL is stateless.

### Global Networking

DNS **Route 53** routing policies:

- Latency-based routing
- Geolocation DNS
- Geoproximity routing
- Weighted round robin

CDN **CloudFront** cache at an edge location.

---

## Module 5: Storage and Databases

### Elastic Block Storage (EBS)

Block level storage.

EC2 machines might have local storage called _Instance Store Volumes_. When the
image stops, that storage will be deleted (because Instance Store Volumes are
specific to the host machine which might change).

**Elastic Block Store (EBS)**. Written data is persisted between starts and
stops of the EC2 instance. Incremental backups are supported (_snapshots_).

- Attached to EC2
- Scoped to an AZ

### S3

Object storage.

- Stores data as objects. Each object consists of data, metadata, and a key.
- 5 TB object size limit
- Support versioning of objects
- Support staging data between different tiers
- Regionally distributed

Types:

- _S3 Standard_ stored in min 3 AZ
- _S3 Standard-Infrequent Access (S3 Standard-IA)_ stored in min 3 AZ. Less frequent but rapid access. Lower storage price, higher retrieval price.
- _S3 One Zone-Infrequent Access (S3 One Zone-IA)_ stored in 1 AZ but lower cost, used for reproducable data.
- _S3 Intelligent-Tiering_ Automatically moves objects between IA and standard tiers.
- _S3 Glacier Flexible Retrieval_ for less frequent access, slow retrieval.
- _S3 Glacier Instant Retrieval_ same retrieval as S3 Standard.
- _S3 Glacier Deep Archive_ lowest-cost storage, data retrieval from 12 to 48 hours. Objects replicated min three geographically dispersed AZs.
- _S3 Outposts_ storage on-premises AWS Outposts environment

_S3 Lifecycle management_: Move data automatically between tiers

### EBS vs S3

S3 is optimized for write once, ready many. Each object is immutable and
changes needs to upload the whole object again. EBS is not as optimized for
reading, but it supports delta changes to files.

### Elastic File System (EFS)

Managed Linux file system.

Use case a large number of services and resources need to access the same data
at the same time.

- Multiple instances can access data in EFS at the same time.
- Automatic scaling
- Regional resource

### Relational Database Service (RDS)

Managed service. Supports

- Automated patching
- Backups
- Redundancy
- Failover
- Disaster recovery
- Encryption

Six available database engines

- Aurora
- MariaDB
- MySQL
- PostgreSQL
- Oracle Database
- Microsoft SQL Server

Support _Lift and Shift Operation_ to move on premise DB to the cloud.

#### Aurora

- MySQL
- PostgreSQL

- Data replication
- Up to 15 read replicas
- Continuous backup to S3
- Point in time recovery

### Dynamo DB

NoSQL, key-value.

- Purpose built
- Millisecond response time
- Highly scalable
- Fully managed

### Redshift

Data warehouse as a service.

For historical analytics. Big data, allows to collect data from many sources.

### Database Migration Service (DMS)

Uses cases:

- Migrating a database to EC2 or RDS
- Database migratiom (Test against production data)
- Database consolidation (Combining several DBs into one)
- Continuous database replication (Sending ongoing copies of data to other sources)

#### Homogenous

Source can be:

- on-premises
- EC2
- RDS

Target can be:

- EC2
- RDS

#### Hetrogenous

First use AWS Schema Conversion Tool, then use DMS.

### Additional Database Services

- Netptune: Graph database
- Quantum Ledger Database (QLDB): complete immutable history
- Managed Blockchain: distributed ledger
- ElastiCache: caching layer on top of DB. Supports Redis and Memcached
- DynamoDB Accelerator (DAX): in-memory cache

---

## Module 6: Security

**AWS Shared Responsibility Model**: Customers are responsible for _security
**in** the cloud_, AWS is responsible for _security **of** the cloud_.

### Identity and Access Management (IAM)

The user that creates an AWS account becomes the _root user_ and has access to everything.
Should turn on MFA and use IAM Users instead.

- _IAM Policy_: JSON object that explicitly allows or denies access to resources. EPARC.
- _IAM User_: Identity, represents person or application that interacts with AWS
  services and resources. Starts with no permissions.
- _IAM Group_: Collection of IAM Users.
- _IAM Role_: Temporal permissions assumable by a trusted entity.

Best practice: Follow principle of least privilege. Use MFA.

### AWS Organizations

AWS Organizations automatically creates a **root**, which is the parent
container for all the accounts in the organization.

- Centralized management of accounts.
- Consolidated billing

**Organizational Units (OU)**: Groupings of accounts. Similar to IAM Groups.

**Service Control Policies (SCPs)**: Apply permissions to root, member account or an OU.
Allows to place restrictions on services, resource and API actions allowed.

### Compliance

- **AWS Artifacts** access to AWS security and compliance reports.
- **AWS Compliance**

### DDoS attacks

Some types of attacks

- **UDP Flood**
  - Fake return address.
  - _Solution_: Security Groups
- **HTTP Level Attack**
- **SlowLoris Attack**
  - Faking a slow connection
  - _Solution_: Elastic Load Balancer

**AWS Shield**: Shields against DDoS attacks.

- _Standard_: no cost, real time analysis of malicious traffic
- _Advanced_: paid service, can handle more sophisticated attacks. Integrates with other services.
  Integrates with _AWS WAF_.

**AWS WAF**: Web Application Firewall. Allows monitoring of network requests. Works together with
CloudFront and ELB. Uses a _web access control list (ACL)_ to block IP addresses.

### Additional Security

**AWS Key Management Service (AWS KMS)**: Create, use, and manage cryptographic keys.

- _Encryption at rest_
- _Encryption in transit_

**AWS Inspector**: Automated security assessment. Checks applications for security vulnerabilities and deviations from security best practices.

**AWS GuardDuty**: Threat detection by monitoring network activity and account behavior.

---

