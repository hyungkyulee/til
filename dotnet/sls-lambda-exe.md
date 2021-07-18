# AWS Lambda Configuration on Serverless

## AWS credential setting for a Lambda connection
[Startup.cs]
```
public static class Startup
{
    private static readonly IServiceCollection Services = new ServiceCollection();
    private static IConfiguration _configuration;

    public static IServiceProvider Build()
    {
        return ConfigureServices().BuildServiceProvider();
    }

    public static IServiceCollection ConfigureServices()
    {
        var configBuilder = new ConfigurationBuilder();
        configBuilder.AddEnvironmentVariables();
        configBuilder.AddJsonFile("appsettings.json", false, true);
        _configuration = configBuilder.Build();

        var awsOptions = new AwsOptions();
        _configuration.GetSection("aws").Bind(awsOptions);

        Services.AddSingleton<AWSCredentials>(x =>
            new BasicAWSCredentials(awsOptions.AccessKey, awsOptions.SecretKey));

        return Services;
    }
}
```

[constructor at Controllers]
```
...
[assembly:LambdaSerializer(typeof(Amazon.Lambda.Serialization.SystemTextJson.DefaultLambdaJsonSerializer))]
namespace ...
...
public class AaaController
{
    public NominatorsController() : this(Startup.Build())
    {
    }
    public NominatorsController(IServiceProvider services)
    {
    }
```

[serverless.yml]
```
...
functions:
  createAaa:
    handler: [proj name].Apis::[proj name].Apis.Controllers.AaaController::Create
    package:
      artifact: bin/Release/netcoreapp3.1/package.zip
    events:
      - http:
          path: aaa
          method: post
          cors: true
...
```
