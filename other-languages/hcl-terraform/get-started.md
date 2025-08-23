# Terraform

## Terraform Setup
### Create an Azure resource group using Terraform

#### configuration
```
~/.../terraform  main ⇣1 !15 ?1 brew install terraform 

// or
~/.../terraform  main ⇣1 !15 ?1 brew upgrade terraform

// check
~/.../terraform  main ⇣1 !15 ?1  terraform --version
Terraform v1.1.4
on darwin_amd64
```

#### initialize terraform
```
mkdir [terraform folder] (e.g. terraform)
cd [terraform folder]
...
~/.../terraform │ main  terraform init                                                                     ✔ │ 21:37:30 
Terraform initialized in an empty directory!

The directory has no Terraform configuration files. You may begin working
with Terraform immediately by creating Terraform configuration files.
```

#### terraform version management on each project
different versions can be changable with 'tfswitch' 

install 'tfswitch'
```
brew install warrensbox/tap/tfswitch
...
tfswitch -l  // [ or tfswitch --list-all ]
```
> Error: Cannot install warrensbox/tap/tfswitch because conflicting formulae are installed.
  terraform
  Please `brew unlink terraform` before continuing.
  ```
  brew unlink terraform
  Unlinking /usr/local/Cellar/terraform/1.1.4... 1 symlinks removed.
  ```
  
```
~/work  tfswitch -l                                                                                              ✔  16s  23:46:40
Creating directory for terraform binary at: /Users/kyu/.terraform.versions
✔ 0.13.2
Downloading to: /Users/kyu/.terraform.versions
35666505 bytes downloaded
Switched terraform to version "0.13.2"

 ~/work  terraform -v                                                                           ✔  1m 3s  Development az  23:47:55
Terraform v0.13.2
```

## AWS
> ref: https://developer.hashicorp.com/terraform/tutorials/aws/lambda-api-gateway

deploy a NodeJS function to AWS Lambda, and then expose that function to the Internet using Amazon API Gateway.

### Prerequisites
#### AWS Cli
```
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
which aws
aws --version
```

### Hierarchy
-- terraform
 |- variables.tf
 |- provider.tf
 
 
- variables.tf  : component name, environments
- provider.tf   : aws region
- config.tf     ; resource bucket at s3 

## Azure
```
Initializing the backend...

Successfully configured the backend "azurerm"! Terraform will automatically
use this backend unless the backend configuration changes.

Initializing provider plugins...
- Finding hashicorp/azurerm versions matching "~> 2.0"...
- Installing hashicorp/azurerm v2.94.0...
- Installed hashicorp/azurerm v2.94.0 (signed by HashiCorp)

Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

format > plan > apply the terraform script files
```
 ~/.../terraform  main ⇣1 !15 ?1  terraform fmt         ✔  21s  Development az  17:44:18 
provider.tf
resource-group.tf
variables.tf
 ~/.../terraform  main ⇣1 !15 ?1  terraform plan             ✔  Development az  17:45:18 
Acquiring state lock. This may take a few moments...

Terraform used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create

Terraform will perform the following actions:

  ... 
  action messages 
  ...

Plan: 4 to add, 0 to change, 0 to destroy.

────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly
these actions if you run "terraform apply" now.
 ~/.../terraform  main ⇣1 !15 ?1  terraform apply       ✔  20s  Development az  17:45:48 

Terraform used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create

Terraform will perform the following actions:

  ... 
  action messages 
  ...

Plan: 4 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

azurerm_resource_group.auroraweb: Creating...
azurerm_resource_group.auroraweb: Creation complete after 1s [id=/subscriptions/d20425c5-***-***-***/resourceGroups/[appname]-rg]
azurerm_cdn_profile.auroraweb: Creating...
azurerm_storage_account.auroraweb: Creating...
azurerm_cdn_profile.auroraweb: Still creating... [10s elapsed]
azurerm_storage_account.auroraweb: Still creating... [10s elapsed]
azurerm_cdn_profile.auroraweb: Still creating... [20s elapsed]
azurerm_storage_account.auroraweb: Still creating... [20s elapsed]
azurerm_cdn_profile.auroraweb: Still creating... [30s elapsed]
azurerm_storage_account.auroraweb: Still creating... [30s elapsed]
azurerm_cdn_profile.auroraweb: Still creating... [40s elapsed]
azurerm_storage_account.auroraweb: Still creating... [40s elapsed]
azurerm_cdn_profile.auroraweb: Creation complete after 42s [id=/subscriptions/d20425c5-***-***-***/resourceGroups/[appname]-rg/providers/Microsoft.Cdn/profiles/[appname]-cdn]
azurerm_cdn_endpoint.auroraweb_web: Creating...
azurerm_storage_account.auroraweb: Still creating... [50s elapsed]
azurerm_cdn_endpoint.auroraweb_web: Still creating... [10s elapsed]
azurerm_cdn_endpoint.auroraweb_web: Creation complete after 13s [id=/subscriptions/d20425c5-***-***-***/resourceGroups/[appname]-rg/providers/Microsoft.Cdn/profiles/[appname]-cdn/endpoints/[appname]]
azurerm_storage_account.auroraweb: Still creating... [1m0s elapsed]
azurerm_storage_account.auroraweb: Creation complete after 1m2s [id=/subscriptions/d20425c5-***-***-***/resourceGroups/[appname]-rg/providers/Microsoft.Storage/storageAccounts/[appname]ondev]

Apply complete! Resources: 4 added, 0 changed, 0 destroyed.
 ~/.../terraform  main ⇣1 !15 ?1   

```


