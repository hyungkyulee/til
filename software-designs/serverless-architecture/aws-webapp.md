# Webapp with serverless platform on AWS

## General App Setting
serverless.yml
```
service: votingsystem-apis
app: bvsapp

frameworkVersion: '2'

provider:
  name: aws
  profile: btm
  runtime: dotnetcore3.1
  stage: dev
  region: eu-west-1
  userpoolId: rgeEa6PQw
  lambdaHashingVersion: 20201221
  environment:
    DOMAIN_SUFFIX: btm
    stage: ${opt:stage, self:provider.stage}

package:
  individually: true

```


## API gateway and Lambda Handler
```
functions:
  createNominator:
    handler: VotingSystem.Apis::VotingSystem.Apis.Controllers.NominatorsController::Create
    package:
      artifact: bin/Release/netcoreapp3.1/package.zip
    events:
      - http:
          path: nominators
          method: post
          cors: true
#          authorizer:
#            arn: arn:aws:cognito-idp:eu-west-1:870289185104:userpool/eu-west-1_${opt:userpoolId, self:provider.userpoolId}
          
```

## Dynamo DB
```
resources:
  Resources:
    nominatorsTable:
      Type: AWS::DynamoDB::Table
      Properties:
        TableName: Nominators-${opt:stage, self:provider.stage}
        AttributeDefinitions:
          - AttributeName: category
            AttributeType: S
          - AttributeName: nominatorId
            AttributeType: S
          - AttributeName: nominatedDate
            AttributeType: S
          - AttributeName: referralDate
            AttributeType: S
          - AttributeName: votedDate
            AttributeType: S
          - AttributeName: schoolName
            AttributeType: S
#          - AttributeName: refereeEmail
#            AttributeType: S
        KeySchema:
          - AttributeName: category
            KeyType: HASH
          - AttributeName: nominatorId
            KeyType: RANGE
        LocalSecondaryIndexes:
          - IndexName: nominatedDateLsi
            KeySchema:
              - AttributeName: category
                KeyType: HASH
              - AttributeName: nominatedDate
                KeyType: RANGE
            Projection:
              ProjectionType: ALL
          - IndexName: referralDateLsi
            KeySchema:
              - AttributeName: category
                KeyType: HASH
              - AttributeName: referralDate
                KeyType: RANGE
            Projection:
              ProjectionType: ALL
          - IndexName: votedDateLsi
            KeySchema:
              - AttributeName: category
                KeyType: HASH
              - AttributeName: votedDate
                KeyType: RANGE
            Projection:
              ProjectionType: ALL
          - IndexName: schoolNameLsi
            KeySchema:
              - AttributeName: category
                KeyType: HASH
              - AttributeName: schoolName
                KeyType: RANGE
            Projection:
              ProjectionType: ALL
#          - IndexName: refereeEmailLsi
#            KeySchema:
#                - AttributeName: category
#                  KeyType: HASH
#                - AttributeName: refereeEmail
#                  KeyType: RANGE
#            Projection:
#              ProjectionType: ALL
        ProvisionedThroughput:
          ReadCapacityUnits: 5
          WriteCapacityUnits: 5
```

## Congnito and Auth
serverless.yml
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
