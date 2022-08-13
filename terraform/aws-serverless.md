
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
