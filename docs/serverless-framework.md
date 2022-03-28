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
- **Resources** are AWS infrastructure components which your Functions
- A **Service** is the Framework's unit of organization
- You can overwrite or extend the functionality of the Framework using
**Plugins**

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

