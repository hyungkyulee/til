# Data Warehouse on AWS

## AWS Environment Setup
### S3
- select options : S3 > create bucket
- set bucket details
  - name : (e.g. sounds3bucket)
    > name should be globaly unique
  - access : block all from the public
  - versioning : enable
  - encryption : SSE-S3 (server side encryption as a default)

- create

### RDS
#### Microsoft SQL DB Instance Creation
- select options: RDS > Standards create > MS SQL > SQL Server Express Edition > latest/stable version 
  > if free tier option is not avilable, you need to change a server version to a few step back.
- set db details
  - db instance name : (e.g. soundlake) 
    > name should be unique in each account
  - master user : hyungkyu
  - master password : p****
- other options
  - one of db classes : db.t2.micro (for free tier)
  - general purpose ssd (iops stands for input/output per second)
  - autoscaling
  - default VPC and subnet group
  - turn on public access
  - default security group
  - no preference zoen if you don't have any specific preference
  - default port : 1433
  - timezone
  - default backup optons / monitoring as it was

- create > available status on rds dashboard
  ![image](https://user-images.githubusercontent.com/59367560/133889590-3e3a929e-6a52-4d8d-9497-759f7750cbd7.png)

#### DB Connection via Azure Data Studio
- update an inbound rule : rds > select your db-instance > security > VPC security groups link > EC2 management console > inbound rules > edit inbound rules > add rules > config anywhere rule (tcp, anywhere ipv4=0.0.0.0) > save rules 
    > security setup will work immediately
    > e.g. ![image](https://user-images.githubusercontent.com/59367560/133889930-a41a2a30-084b-4dcb-97d3-5675093d49fa.png)

- Azure Data Studio > Add connection > Connection details (below) > Connect
  - Type : MS SQL SQL Server
  - Server : DB Instance's endpoint url
    > according to inbound rule, you need to put port number at the end of endpoint : [endpoint url],portnumber (e.g. xxxxx.eu-west-1.rds.amazonaws.com,1433)
  - Auth Type : SQL Login
  - User Name : SQL DB Instance username (e.g. hyungkyu)
  - Password : SQL DB Instance password (e.g. p****)
  > Azure Data Studio Installation : download link - https://docs.microsoft.com/en-us/sql/azure-data-studio/download-azure-data-studio?view=sql-server-ver15#download-azure-data-studio
 
![image](https://user-images.githubusercontent.com/59367560/133890004-9450379c-de4a-4a25-8596-d11ebeb0028a.png)

- error case
  ![image](https://user-images.githubusercontent.com/59367560/133889820-4ba7f956-ffd7-4d31-b73b-d63c3bc14798.png)
  > check if inbound rule is properly allowing the anywhere access


### Kinesis
#### Collect Data Stream
#### Deliver Data Stream (Firehorse)
- Create Deliver Stream > select (Source (e.g. Direct PIT) > selec other options :
  - Transform source records with AWS Lambda : disabled (later will be enabled)
  - Convert record format : disabled (later will be enabled)
    > Data in Apache Parquet or Apache ORC format is typically more efficient to query than JSON. Kinesis Data Firehose can convert your JSON-formatted source records using a schema from a table defined in AWS Glue . For records that aren't in JSON format, create a Lambda function that converts them to JSON in the Transform source records with AWS Lambda section above.
  - 
#### Analyse Data Stream (Analytics)








