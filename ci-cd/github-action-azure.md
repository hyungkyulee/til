
Azure > App REgistrations ![Pipeline Status](https://github.com/sainsburys-tech/ProductionPlanning-Frontend/actions/workflows/build_and_deploy-frontend.yml/badge.svg)

- create a new app registrations for automation
  > this app will be a service principal
- add owners
- add a base federated credential
- add a evironmental related federated credential
  - federated credential scenario = GitHub Actions deploying Azure resources
  - environment name = have a [env] prefix (e.g. dev-xxxxx)

Azure > Enterprice application

Group > add a member with the above service principal
Container registries > [Name] > IAM > Role assingments 
 -> the resource needs to have a github action permission of AcrPush
 
 
Github > settings > Environments
- add new environments (same as the above environments)
- set a rule

Github > repo > .github > workflows
- add .yml file
