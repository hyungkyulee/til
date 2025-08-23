# API Documentation

## openAPI / Swagger on Asp.Net Core Web API
https://docs.microsoft.com/en-us/aspnet/core/tutorials/web-api-help-pages-using-swagger?view=aspnetcore-6.0

### Background
| API type | API Language | Definition Language | Toolbox |
|---|---|---|---|
| SOAP/WCF | XML | WSDL (Web Service Definition Language) | ASMX / WCF framework |
| REST API | JSON | OpenAPI (formerly, Swagger Specification) : an API description format for REST APIs | NSwag / Swashbuckle |

> diff. between Swagger and OpenAPI : https://swagger.io/blog/api-strategy/difference-between-swagger-and-openapi/ 
> OpenAPI = Spec vs Swagger = open source tools to build up (or implement) Spec
> Swagger spec was owened by SmartBear, but donated to 'Open API initiative'. 
> The name has been changed to 'Open API Specification' since the version OpenAPI2.0. 

### Swashbuckle vs NSwag
- Visual Studio의 "Enable OpenAPI support" 옵션에서 Swashbuckle 패키지를 기본으로 제공
- with NSwag, explicitly, the url is required to type at browser
  > Swagger UI - http://localhost:<port>/swagger
  > Web API Spec - http://localhost:<port>/swagger/v1/swagger.json

### NSwag
#### Github
https://github.com/RicoSuter/NSwag 

#### Setup
https://github.com/RicoSuter/NSwag/wiki/AspNetCore-Middleware

- Install Nuget Package on a API project
  > NSwag.AspNetCore (by Rico Suter
  
- Config Services
  ```
  // Startup
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddControllersWithViews();
      services.AddCors(options =>
      {
          options.AddPolicy(name: "Web",
              builder =>
              {
                  builder.AllowAnyOrigin();
              });
      });

      services.AddOpenApiDocument(document => document.DocumentName = "OpenApi");
      services.AddSwaggerDocument(document => document.DocumentName = "Swagger");
      
      services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
      ...
      

  // Configure
  public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
  {
      ...
      app.UseRouting();
      app.UseCors("Web");
  
      app.UseOpenApi();
      app.UseSwaggerUi3();
      ...
      
      app.UserEndpoints ...

  ```
