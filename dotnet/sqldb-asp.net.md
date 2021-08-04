# SQL Database on ASP.NET project

## Project Creation
### ASP.NET Web API with React JS
![image](https://user-images.githubusercontent.com/59367560/128099517-610b28e8-910b-4500-aca0-c1c955bb0b83.png)

### Nuget Packages
- Dapper
- System.Data.SqlClient
![image](https://user-images.githubusercontent.com/59367560/128100085-85719799-6946-4513-9a50-b98c100b2a43.png)

## DB connection String configuration
### appsettings.json
```
{
  "Logging": {
      "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
      }
    },
  "ConnectionStrings": {
    "TestProject": "Server=awl-sv-nav.aurorauk.local;Database=nav-dev-bc;User Id=AuroraHQ2;Password=yoohoo202!;"
  },
"AllowedHosts": "*"
}
```

### Inject the connection string service on Startup
[Startup.cs]
```
...
public void ConfigureServices(IServiceCollection services)
{
    var connectionString = Configuration.GetConnectionString("TestProject");

    services.AddControllersWithViews();
    services.AddSingleton<ISalesLineRepository>(x => 
        new SalesLineRepository(connectionString,
            x.GetService<ILogger<SalesLineRepository>>()));

    ...
}
...
```
> ServiceCollection is ready to handle connection-string

### Repository for the SQL Table
[xxxRepository.cs]
```
public class SalesLineRepository : ISalesLineRepository
{
    private readonly string _connectionString;
    private readonly ILogger<SalesLineRepository> _logger;

    public SalesLineRepository(string connectionString, 
        ILogger<SalesLineRepository> logger)
    {
        _connectionString = connectionString;
        _logger = logger;
    }

    public async Task<IEnumerable<Order>> GetBulkOrders()
    {
        _logger.LogInformation(_connectionString);
        
        var sql = @$"
                SELECT TOP 100 [Document Type] as DocumentType, *
                  FROM [dbo].[Aurora World Ltd_$Sales Line]

            ";
            using var connection = new SqlConnection(_connectionString);
            connection.Open();

            var results = (await connection.QueryAsync<Order>(sql)).ToList();
            
            return results;
    ...
```

## Controller
```
namespace [proj name].Controllers
{
    [ApiController]
    [Route("[controller]")]
    public class SalesOrdersController : ControllerBase
    {
        private readonly ILogger<SalesOrdersController> _logger;
        private readonly ISalesLineRepository _salesLineRepository;

        public SalesOrdersController(ILogger<SalesOrdersController> logger,
            ISalesLineRepository salesLineRepository)
        {
            _logger = logger;
            _salesLineRepository = salesLineRepository;
        }

        [HttpGet]
        public async Task<IActionResult> Get()
        {
            var orders = await _salesLineRepository.GetBulkOrders();
            return Ok(orders);
        }
        
    }
}
```

## [Appendix] SQL Query - More
```
var sqlCompanies = @$"
    SELECT TOP (@limit) *
      FROM onboarding.Companies c WITH (NOLOCK) JOIN onboarding.Entities e WITH (NOLOCK)
        ON e.EntityId = c.CompanyId
     WHERE e.TenantId = @tenantId 
       AND e.EntityStatusId = {EntityActiveStatus}
     {GetCompanyNameConditionStatement(conditions)}
     ORDER BY e.EntityId DESC
";
using var connection = new SqlConnection(connectionString);
connection.Open();

var results = (await connection.QueryAsync<Company>(sqlCompanies, new
{
    tenantId,
    limit = limit + 1
})).ToList();
```
