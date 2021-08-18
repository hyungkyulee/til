# Auth Flow on ReactJS

## Auth Overview

## AWS Cognito
### Serverless Cognito Resource
```
resources:
  Resources:
    serviceUserPool:
      Type: AWS::Cognito::UserPool
      Properties:
        UserPoolName: bvs-nominator-${opt:stage, self:provider.stage}
        AdminCreateUserConfig:
          AllowAdminCreateUserOnly: False
        UsernameAttributes:
          - email
        AutoVerifiedAttributes:
          - email
    serviceUserPoolClient:
      Type: AWS::Cognito::UserPoolClient
      Properties:
        ClientName: bvs-nominator-client-${opt:stage, self:provider.stage}
        AllowedOAuthFlows:
          - implicit
        AllowedOAuthFlowsUserPoolClient: true
        AllowedOAuthScopes:
          - phone
          - email
          - openid
          - profile
          - aws.cognito.signin.user.admin
        UserPoolId:
          Ref: serviceUserPool
        CallbackURLs:
          - http://localhost:3000/sign-in-callback
        ExplicitAuthFlows:
          - ALLOW_USER_SRP_AUTH
          - ALLOW_REFRESH_TOKEN_AUTH
        GenerateSecret: false
        SupportedIdentityProviders:
          - COGNITO
    serviceUserPoolDomain:
      Type: AWS::Cognito::UserPoolDomain
      Properties:
        UserPoolId:
          Ref: serviceUserPool
        Domain: bvs-nominator-${opt:stage, self:provider.stage}-${self:provider.environment.DOMAIN_SUFFIX}
```

### Hosted UI
- loginpath : 
  - [AUTH_URI] + 
  - ?client_id=$ + [CLIENT_ID] + 
  - &response_type=token&scope=$ + [SCOPE] + 
  - &redirect_uri=$ + [window.location.origin]/[callback path] + 
  - $state= + [react useLocation().path]
  e.g.
  ```
  <a href={`${config.AUTH_URI}?client_id=${config.CLIENT_ID}&response_type=token&scope=${config.SCOPE}&redirect_uri=${window.location.origin}/sign-in-callback&state=${useLocation().pathname}`} 
  ```
