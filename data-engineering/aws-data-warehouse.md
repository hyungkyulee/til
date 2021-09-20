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
- Create Deliver Stream > select options (below) > create
  - Source : Kinesis Data Stream collected or Direct PUT
  - Transform source records with AWS Lambda : disabled (later will be enabled)
    > For records that aren't in JSON format, create a Lambda function that converts them to JSON in the Transform source records with AWS Lambda
  - Convert record format : disabled (later will be enabled)
    > Data in Apache Parquet or Apache ORC format is typically more efficient to query than JSON. Kinesis Data Firehose can convert your JSON source records using a schema from a table defined in AWS Glue.
  - Destination : three options (e.g. the above s3 bucket)
  - s3 bucket prefix : optional (default is date prefix)
  - buffer hints : higher buffer memory (low cost - high latency) vs lower buffer memory (high cost - low latency)
  - buffer interval
    > in test, 128MiB + 60sec is recommended for low cost
  - compression and encryption : default as it was 
    > we don't need to set this enabled as data will be already compressed by apache parquet on a nice format
  - permission : IAM role for kinesis firehose role
  
#### Analyse Data Stream (Analytics)


## Connect RDS DB on .NET application
### Connection Configuration to RDS
```
namespace FirehoseSoundLake.Helpers
{
    public class RdsConfig
    {
        public static string GetRDSConnectionString()
        {
            var appConfig = ConfigurationManager.AppSettings;

            string dbname = appConfig["RDS_DB_NAME"];

            if (string.IsNullOrEmpty(dbname)) return null;

            string username = appConfig["RDS_USERNAME"];
            string password = appConfig["RDS_PASSWORD"];
            string hostname = appConfig["RDS_HOSTNAME"];
            string port = appConfig["RDS_PORT"];

            return "Data Source=" + hostname + ";Initial Catalog=" + dbname + ";User ID=" + username + ";Password=" + password + ";";
        }
    }
}
```

```
using FirehoseSoundLake.Helpers;
using FirehoseSoundLake.Models;
using Microsoft.EntityFrameworkCore;

namespace FirehoseSoundLake.Repositories
{
    public class PlaylistContext : DbContext
    {
        // public PlaylistContext() : base(RdsConfig.GetRDSConnectionString())
        // {
        // }
        
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            base.OnConfiguring(optionsBuilder);
            optionsBuilder
                .UseSqlServer(connectionString: @"Data Source=soundlake.***.eu-west-1.rds.amazonaws.com,1433;Initial Catalog=ArtistDatabase;Persist Security Info=True;User ID=hyungkyu;Password=p***;MultipleActiveResultSets=True");
            // optionsBuilder.UseSqlServer(connectionString: RdsConfig.GetRDSConnectionString());

        }
        
        public DbSet<Artist> Artists { get; set; }
        public DbSet<Album> Albums { get; set; }
    }
}
```

### Dotnet EntityFramework Migration
This is a guideline of .NET Core Cli 
> Visual Studio can be referred at https://docs.microsoft.com/en-us/ef/core/managing-schemas/migrations/?tabs=dotnet-core-cli

#### EntityFramework Cli installation on Mac
```
> dotnet tool install --global dotnet-ef
> dotnet ef --help
```

#### Code First Migration based on the above models
- Migration Settings on a Project
```
> dotnet ef migrations add InitPlaylistMigration
Build started...
Build succeeded.
Done. To undo this action, use 'ef migrations remove'
```

![image](https://user-images.githubusercontent.com/59367560/134059665-26cb35e3-fafd-4525-adfa-2c01571ac2dd.png)
> EF Core will create a directory called Migrations in your project, and generate some files.

- Create Tables from Migration Settings to Database
```
> dotnet ef database update                   1 х  20:08:59 
Build started...
Build succeeded.
Applying migration '20210920185722_InitPlaylistMigration'.
Done.
```

![image](https://user-images.githubusercontent.com/59367560/134060908-cbe851fe-af41-4c98-8699-03edd91cb54d.png)
> with your refresh on Database client (e.g. Azure Data Studio)
