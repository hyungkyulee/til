# Terrafom deployment via Github Action to Azure 

## Terraform hierachy and Basic Commands
![image](https://user-images.githubusercontent.com/59367560/125361635-77709100-e365-11eb-9cea-cbfaf24098f0.png)

- terraform init
``` terraform init ```
- terraform plan
``` terraform plan ```

## Terraform State Resource
- create .github/workflows/terraform.yml on project root
```
name: 'run terraform scripts'
on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  terraform:
    name: 'Terraform'
    defaults:
      run:
        working-directory: "./terraform"
    env:
      ARM_ACCESS_KEY: ${{ secrets.AZURE_STORAGE_ACCESS_KEY }}
      ARM_CLIENT_ID: ${{ secrets.AZURE_AD_CLIENT_ID }}
      ARM_CLIENT_SECRET: ${{ secrets.AZURE_AD_CLIENT_SECRET }}
      ARM_SUBSCRIPTION_ID: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
      ARM_TENANT_ID: ${{ secrets.AZURE_AD_TENANT_ID }}
    
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v1
      - name: Terraform Format
        id: fmt
        run: terraform fmt -check
      - name: Terraform Init
        id: init
        run: terraform init
      - name: Terraform Plan
        id: plan
        run: terraform plan -no-color
        continue-on-error: true
      - name: Terraform Plan Status
        if: steps.plan.outcome == 'failure'
        run: exit 1
      - name: Terraform Apply
        if: github.ref == 'refs/heads/main' && github.event_name == 'push'
        run: terraform apply -auto-approve
```
> workflow_dispatch - enable a button to run manually on github action

- register 'Action secrets' on github
tstate > 

- Create Storage account for Terraform State resource
> azure > storage accounts > create a storage account
  - subscription : development
  - resource gorup : create new > xxx
  - Storage account name : tfstatexxx
  > in progress > completed

- azure > resource manager > go to created resource name > access key
  > copy the key to github action

- azure > app registrations > new registration > trraform principle (non-human = app ca, hubman = smater)
  > human = user, non-human(aka. surbice) = principle 
  > Roles and administrators 

- terraform-principle > certificates & secrets > new client secret

- add the created principal of terraform
  > Subscriptions > Development > IAM

- add container 
  > resource > containers > add container

> Azure hiearchy
account > multiple subscriptions > resource group > resource accounts > 

## Azure API Management Service
- API Management Service
resource "azurerm_api_management" "[proj name]" {
  name = "[proj name]-apim"
  location = [location variable name]
  resource_group_name = [resource group variable name]
  publisher_name = "[company name]"
  publisher_email = "[company email]"
  
  sku_name = "Consumption_0"
  
  policy {
    xml_content = <<XML
    <policies>
      <inbount />
      <backend />
      <outbound />
      <on-error />
    </policies>
  }
XML
}

## Azure Function App
- Create a Azure Resource Management Function App
```terraform
resource "azurerm_app_service_plan" "[proj name]" {
  name                = "[proj name]"
  location            = var.[location variable]
  resource_group_name = azurerm_resource_group.[proj name].name
  kind                = "functionapp"
  reserved            = true

  sku {
    tier = "Dynamic"
    size = "Y1"
  }
}

resource "azurerm_function_app" "[proj name]" {
  name                       = "[proj name]"
  location                   = var.[location variable]
  resource_group_name        = azurerm_resource_group.[proj name].name
  app_service_plan_id        = azurerm_app_service_plan.[proj name].id
  storage_account_name       = azurerm_storage_account.[proj name].name
  storage_account_access_key = azurerm_storage_account.[proj name].primary_access_key
  os_type                    = "linux"
}
```

## Deploy the code via github action
Run the terraform and Build the source code to outoput file.

> root > .github/workflows > build-and-deploy.yml
```
name: 'build and deploy'
on:
  push:
    branches:
      - main
  workflow_dispatch:
  
env:
  DOTNET_VERSION: '3.x'
  AZURE_FUNCTIONAPP_PACKAGE_PATH: 'src/Deepeyes.ShowMeMoney.Apis'
  
jobs:
  terraform:
    name: 'Terraform'
    defaults:
      run:
        working-directory: "./terraform"
    env:
      ARM_CLIENT_ID: ${{ secrets.AZURE_AD_CLIENT_ID }}
      ARM_CLIENT_SECRET: ${{ secrets.AZURE_AD_CLIENT_SECRET }}
      ARM_SUBSCRIPTION_ID: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
      ARM_TENANT_ID: ${{ secrets.AZURE_AD_TENANT_ID }}
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v1
      - name: Terraform Format
        id: fmt
        run: terraform fmt -check
      - name: Terraform Init
        id: init
        run: terraform init
      - name: Terraform Plan
        id: plan
        run: terraform plan -no-color
        continue-on-error: true
      - name: Terraform Plan Status
        if: steps.plan.outcome == 'failure'
        run: exit 1
      - name: Terraform Apply
        if: github.ref == 'refs/heads/main' && github.event_name == 'push'
        run: terraform apply -auto-approve
  build:
    needs: terraform
    runs-on: ubuntu-latest
    steps:
    - name: 'Checkout GitHub Action'
      uses: actions/checkout@main

    - name: Setup DotNet ${{ env.DOTNET_VERSION }} Environment
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: ${{env.DOTNET_VERSION}}

    - name: 'Build'
      shell: bash
      run: |
        pushd './${{ env.AZURE_FUNCTIONAPP_PACKAGE_PATH }}'
        dotnet build --configuration Release --output ./output
        popd
```

## Publish (Deploy Azure Functions)
> ref: build and deploy using github actions
Github > Secrets > Copy the secrets from Azure portal
```
build:
  
  ...
  
  - name: 'Deploy Azure Functions'
    uses: Azure/functions-action@v1.3.2
    id: fa
    with:
      app-name: ${{ env.AZURE_FUNCTIONAPP_NAME }}
      package: '${{ env.AZURE_FUNCTIONAPP_PACKAGE_PATH }}/output'
      publish-profile: 4{{ secrets.AZURE_FUNCTIONAPP_PUBLISH_PROFILE }}
```
> uses: Azure/functions-action@v1 (latest version has an issue at 13 July 2021)

