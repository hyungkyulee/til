# Configure the AWS dev environment

## AWS Credentials
1) Set up AWS Account : https://console.aws.amazon.com/console/home 
2) Configure AWS IAM User
  > Services -> IAM -> Add User -> create a user for this serverless hosting
    - User name: e.g. 'serverless-admin'
    - Options: 'Programmatic access', 'Attach existing policies directly', 'AdministratorAccess' 
  > Copy the 'Access Key ID' and 'Secret Access Key' (or download ".csv") to login later
3) Set up AWS Cli Development environment (for MAC)
   ```
   $ brew install awscli

   $ aws configure
   AWS Access Key ID [None]: your access key
   AWS Secret Access Key [None]: your secret access key
   Default region name [None]: your region name (e.g. us-west-2)
   Default output format [None]: ENTER
   ```
    
