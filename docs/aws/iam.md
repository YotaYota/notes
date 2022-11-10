# IAM - Identity and Access Management

Default access is "Deny".

## IAM Identities

There are 3 types of IAM identities:

- IAM user
- IAM user group
- IAM role

**Note**: IAM identities live inside an AWS Account, which is a boundary - by
default, no access is given to identities outside the account (but can be
allowed).

**Note**: The **root** user has access to everything and cannot be limited by
IAM policies.

### IAM User

An **IAM user** represents a the person or service that interacts with AWS. IAM
users have long-term credentials.

### IAM Role

An **IAM role** is not associated with an identity, and has no long-term access
keys (as users do). IAM roles are *assumed* by trusted entities, which gives
that entity the permissions of the role temporarily (assumed role expires).

**Note**: Assuming a role gives credentials that last between 15 minutes and 36
hours.

### IAM Group

An **IAM group** is a collection of users.

---

**Best practice**: Prefer roles over users when possible.

**Best practice**: Prefer attaching a user to a group over giving credentials
directly to the user.

## IAM Policy (E-PARC)

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

### Policy Types

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

## IAM Evaluation Logic

1. Is the user the root user   -> **Allow**
2. Is there a specific "Deny"? -> **Deny**
3. Is there a specific "Allow" -> **Allow**
4. -> **Deny**
