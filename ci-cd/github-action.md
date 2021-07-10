# Github Action for terrafom deployment

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

- azure > storage accounts > create a storage account
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
