# Project Configuration in Dotnet

## Web API project in Rider 2021.1.3
The initial project template will pre-set with a hostbuilder(web server), program main, and launchsettings in properties.

To make it just an independent API project on serverless platform, 
- remove program.cs
- project setting > change sdk type
  > ``` <Project Sdk="Microsoft.NET.Sdk.Web"> ```
  > ``` <Project Sdk="Microsoft.NET.Sdk"> ```
- remove Microsoft.AspNetcore packages
  ```
  using Microsoft.AspNetCore.Builder;
  using Microsoft.AspNetCore.Hosting;
  using Microsoft.AspNetCore.Http;
  ```
- Make the startup class empty

![image](https://user-images.githubusercontent.com/59367560/124050501-950f3380-da12-11eb-990d-21e700a57e8a.png)

## Serverless platform
### install serverless framework create lambda projec with c# template
``` zsh
(base) ➜  src $ npm install -g serverless
(base) ➜  src $ cd RoboticcistsApis.Apis
(base) ➜  RoboticistsApis.Apis $ sls create --template aws-csharp          
Serverless: Generating boilerplate...
 _______                             __
|   _   .-----.----.--.--.-----.----|  .-----.-----.-----.
|   |___|  -__|   _|  |  |  -__|   _|  |  -__|__ --|__ --|
|____   |_____|__|  \___/|_____|__| |__|_____|_____|_____|
|   |   |             The Serverless Application Framework
|       |                           serverless.com, v2.30.3
 -------'

Serverless: Successfully generated boilerplate for template: "aws-csharp"
Serverless: NOTE: Please update the "service" property in serverless.yml with your service name
```

  
## Nuget packages to install

[ServiceCollection]
- Microsoft.Extensions.DependencyInjection
  
[ConfigurationBuilder, AddJsonFile, AddEnvironmentVariables]
- Microsoft.Extensions.Configuration
- Microsoft.Extensions.Configuration.Abstractions
- Microsoft.Extensions.Configuration.Binder
- Microsoft.Extensions.Configuration.EnvironmentVariables
- Microsoft.Extensions.Configuration.Json

[AWSSDK, Lambda, DynamoDB]
- AWSSDK.Core
- AWSSDK.DynamoDBv2
- Amazon.Lambda.Core
- Amazon.Lambda.APIGatewayEvents
- Amazon.Lambda.Serialization.SystemTextJson
  
[SQL]
- sqlite-net-pcl
  
