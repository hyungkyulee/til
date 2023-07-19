
## Nodejs Project

- create 'terraforms' folder
- add config.tf variables.tf, provider.tf as a set of basic setup
```
// config.tf
terraform {
  backend "s3" {
    bucket = "[bucket name]"
    key    = "[key name]"
    region = "eu-west-1"
  }
  required_version = "~>1.1"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

// provicer
```
> ref: https://registry.terraform.io/providers/hashicorp/aws/latest/docs

- init
```
terrafrom init
```

> policy 를 부여받는 주체는 User or Principal


### Lambda
- create lambda
- provide log policy to lambda to access cloudwatch
- create cloudwatch resource (a.k.a log group)

#### a customised role for Lambda
- run
- access cloudwatch : log policy
- access dynamodb : dynamodb policy
- access cognito : cognito policy
- access s3 : s3 policy

- policy 는 패키지로 설정되어야 한다. => e.g. log policy = aws_iam_policy_document + aws_iam_policy + aws_iam_role_policy_attachment
- 
