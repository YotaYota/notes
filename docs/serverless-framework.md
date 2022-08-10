# Serverless Framework

Helps develop your AWS Lambda along with required AWS infrastructure around it.

```
serverless create -t <template, eg aws-nodejs>
```

```
serverless deploy --stage <stage>
```

```
serverless info
```

```
serverless invoke -f hello --log
```

```
serverless logs -f hello --tail
```

```
serverless remove
```

## Core Concepts

- A **Function** is an AWS Lambda function
- **Resources** are AWS infrastructure components which your Functions use
- Anything that triggers an AWS Lambda Function to execute is regarded by the
Framework as an **Event**
- A **Service** is the Framework's unit of organization
- You can overwrite or extend the functionality of the Framework using
**Plugins**

## Permissions

- `provider.profile`: framework user permissions for creation ( lookup in *~/.aws/credentials*)
- `provider.iam.deploymentRole`: deployment role after creation
- `provider.iam.role.statements`: lambda permissions

### Deployer Role

- Fetch configuration data required to synthesise a new CloudFormation template.
(eg SSM)
- Create S3 Bucket for storing deplyment artifact
- Validating CloudFormation template
- Deploy CloudFormation template

## Plugins

Install the plugin then configure `serverless.yml` file

```
plugins:
  - <serverless plugin name>
```

## `serverless.yml`

Can also be a json, js or ts file.

```
service: users

functions: # Your "Functions"
  usersCreate:
    events: # The "Events" that trigger this function
      - httpApi: 'POST /users/create'
  usersDelete:
    events:
      - httpApi: 'DELETE /users/delete'

resources: # The "Resources" your "Functions" use.  Raw AWS CloudFormation goes in here.
```

