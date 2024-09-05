# API Gateway

- Websocket (Stateful)
- HTTP
- public REST API
- private REST API

## Authentification and Authorization

3 options for Authentification:

- IAM
- Custom Lambda
- Cognito

## Proxy vs Integration

Lambda-proxy sends request from API Gateway directly to lambda without
modifications.

In a Lambda integration the request and response can be modified in the API
Gateway using Velocity Template Language (VTL).

**Method Request**: request originated from the client.

**Integration Request** the transformation that you can do with API Gateway.
The request body can be transformed as per your body mapping template.

**Integration Response**: This is where you can assign appropriate status code
and do response transformation, if present. After transformation, the response
is sent to client.

