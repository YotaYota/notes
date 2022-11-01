# AWS

## AWS CLI

```
aws configure
```

*AWS Access Key ID* and *AWS Secret Access Key* are stored in
`~/.aws/credentials`. These are associated with an IAM user or role.

All other configuration is stored in `~/.aws/config`.


## IAM - Identity and Access Management

Default access is "Deny".

### IAM Identities

There are 3 types of IAM identities:

- IAM user
- IAM user group
- IAM role

**Note**: IAM identities live inside an AWS Account, which is a boundary - by
default, no access is given to identities outside the account (but can be
allowed).

**Note**: The **root** user has access to everything and cannot be limited by
IAM policies.

#### IAM User

An **IAM user** represents a the person or service that interacts with AWS. IAM
users have long-term credentials.

#### IAM Role

An **IAM role** is not associated with an identity, and has no long-term access
keys (as users do). IAM roles are *assumed* by trusted entities, which gives
that entity the permissions of the role temporarily (assumed role expires).

**Note**: Assuming a role gives credentials that last between 15 minutes and 36
hours.

#### IAM Group

An **IAM group** is a collection of users.

---

**Best practice**: Prefer roles over users when possible.

**Best practice**: Prefer attaching a user to a group over giving credentials
directly to the user.

### IAM Policy (E-PARC)

Policies are attached to IAM identities (users, groups and roles).

Required parts are

- **Effect**: statement result; "Allow" or "Deny"
- **Action**: activity or call the statement covers
- **Resource**: object or objects (by ARN) the statement covers

```
{
  "Version" "2012-10-17"
  "Statement": {
    "Effect": "Allow",            # Allow or Deny
    "Action": "ec2:RunInstances", # Describes the specific Action
    "Resource": "*"               # ARN it applies to (here: all EC2 instances)
  }
}
```

**Note**: *Action* and *Resource* accepts wildcards.

Optional parts are

- **Principal**: Specifies the principal that is allowed to access a resource.
- **Condition**: Specifies conditions for when a policy is in effect.

#### Policy Types

- Identity based policies (attached to an identity)
- Resource based policies (attached to a resource)

Resource based policies are used to attach permissions to the **Principal**.
Identity based policies do **not** make use of the Principal element.

**Example**: IAM Policy that allows all Principals, but ony from certail IP
addresses

```
"Principal": "*"`,
"Condition": { "IpAddress": { "aws:SourceIp": ["<IP ADDRESS>"]}}
```

### IAM Evaluation Logic

1. Is the user the root user   -> **Allow**
2. Is there a specific "Deny"? -> **Deny**
3. Is there a specific "Allow" -> **Allow**
4. -> **Deny**

## Cognito

- User Pools
- Identity Pools

Access Token is not the same as Identity Token.

## Compute

### ECS

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

#### Deployment

*Deployment packages* are used to deploy a lambda.

2 types of deployment packages are supported:

- .zip archive (uploaded to S3 or local machine)
- container image (uploaded to Amazon ECR)

## Network & Security

- Route 53 - DNS
- CloudFront - CDN
- Virtual Private Cloud (VPC)
- Security Groups
- Internet Gateway (IGW)
- Public/private subnets
- Elastic Load Balancer (ELB)

### API Gateway

- Websocket (Stateful)
- HTTP
- public REST API
- private REST API

#### Authentification and Authorization

3 options for Authentification
- IAM
- Custom Lambda
- Cognito

### VPC

- Comes with a default customizable network access control list (ACL)
- Can have private and public subnets

### IGW

Connects VPC to internet. Attach to VPC and specify as a target in subnet route
table.

- Enables internet connection of resource with public IP addresses in subnets
that are public
- Routes traffic to subnet according to routing table

### Subnet

Public and private grouping of resources. A range of IP addresses in a VPC.

Subnets must be connected to an ACL, if not it will be associated with the
default ACL of the VPC.

Get resources in subnet

```
aws ec2 describe-network-interfaces --filters Name=subnet-id,Values=subnet-id-here | grep Description
```

### ELB

Elastic Load Balancing. To connect with a VPC; specify one or more subnets for
the load balancer nodes.

Register listerners (a process that checks for connection requests) with a
specified target group and conditions. When conditions are met, traffic is
forwarded to the target group.

- Monitors health of registred targets and only routes to healthy

### Security Group

A Security Group acts as a virtual firewall. **Inbound** and **outbound** rules
specify which access are allowed. Unless specifically allowed, it defauls to
denied.

- Supports only allow rules, not deny rules
- Stateful: always allows response traffic (in both directions)
- Operates at instance level (applies to instance only if associated)
- When resources are associated with multiple Security Groups, their rules are
aggregated into one set of rules

### ACL

- Operates at subnet level (applies to all instances in the subnet)
- Supports allow rules and deny rules
- Stateless (return traffic must be explicitly allowed by rules)

## Messages

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

