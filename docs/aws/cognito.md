# Cognito

- User Pools
- Identity Pools

Access Token is not the same as Identity Token.

## User Pool Lambda Triggers

```yaml
CognitoUserPoolLogicalName:
  Type: 'AWS::Cognito::UserPool'
  Properties:
    ...
    LambdaConfig:
      CreateAuthChallenge: String
      CustomEmailSender: CustomEmailSender
      CustomMessage: String
      CustomSMSSender: CustomSMSSender
      DefineAuthChallenge: String
      KMSKeyID: String
      PostAuthentication: String
      PostConfirmation: String
      PreAuthentication: String
      PreSignUp: String
      PreTokenGeneration: String
      UserMigration: String
      VerifyAuthChallengeResponse: String
```
