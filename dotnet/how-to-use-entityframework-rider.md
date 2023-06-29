## Entity Framework nuget package

## Use dotnet cli
> ref: https://blog.jetbrains.com/dotnet/2017/08/09/running-entity-framework-core-commands-rider/
> ref: https://learn.microsoft.com/en-us/ef/core/cli/dotnet#usage

install the command line tools
```
dotnet tool install --global dotnet-ef
```
OR update
```dotnet restore```

Verify the existing installation
```dotnet ef```

### usages
#### dotnet ef database update
Updates the database to the last migration or to a specified migration.

- Arguments:
  > Argument	Description
  > <MIGRATION>	The target migration. Migrations may be identified by name or by ID. The number 0 is a special case that means before the first migration and causes all migrations to be reverted. If no migration is specified, the command defaults to the last migration.
- Options:
  > Option	Description
  > --connection <CONNECTION>	The connection string to the database. Defaults to the one specified in AddDbContext or OnConfiguring.
- example : ```dotnet ef database update -s [startup project]```

#### dotnet ef migrations add
Adds a new migration.

- Arguments:
  > Argument	Description
  > <NAME>	The name of the migration.
- Options:
  > Option	Short	Description
  > --output-dir <PATH>	-o	The directory use to output the files. Paths are relative to the target project directory. Defaults to "Migrations".
  > --namespace <NAMESPACE>	-n	The namespace to use for the generated classes. Defaults to generated from the output directory.
- example : ```dotnet ef migrations add [name]```

#### dotnet ef migrations remove
Removes the last migration, rolling back the code changes that were done for the latest migration.

- Options:
  > Option	Short	Description
  > --force	-f	Revert the latest migration, rolling back both code and database changes that were done for the latest migration. Continues to roll back only the code changes if an error occurs while connecting to the database.
- example : ```dotnet ef migrations remove```

#### dotnet ef database drop
- Deletes the database.
- Options:
  > Option	Short	Description
  > --force	-f	Don't confirm.
  > --dry-run		Show which database would be dropped, but don't drop it.
  > The common options are listed above.

#### dotnet ef dbcontext info
Gets information about a DbContext type.

#### dotnet ef dbcontext list
Lists available DbContext types.

  
#### dotnet ef dbcontext scaffold
Generates code for a DbContext and entity types for a database. In order for this command to generate an entity type, the database table must have a primary key.

- Arguments:
  > Argument	Description
  > <CONNECTION>	The connection string to the database. For ASP.NET Core 2.x projects, the value can be name=<name of connection string>. In that case the name comes from the configuration sources that are set up for the project.
  > <PROVIDER>	The provider to use. Typically this is the name of the NuGet package, for example: Microsoft.EntityFrameworkCore.SqlServer.

  > 

## Use a 3rd-party plugin
- install the Entity Framework Core UI plugin at Rider
  > Rider Menu > settings (option + .) > Plugins > Marketplace tab > search 'Entity Framework Core UI'
  > ![image](https://user-images.githubusercontent.com/59367560/236954830-3502f60a-8218-44c7-96ef-ee27005e0cbb.png)

- save and restart IDE
- open a solution including a ef-migration project
- find a menu of EF plugin UI
  > tools > Entity Framework Core > Update Database (or Add migration, etc)
  > ![image](https://user-images.githubusercontent.com/59367560/236955246-080e45ea-0fb4-48e2-b5c7-c9c60478cebc.png)

### update database
- set a target migration : it will be a latest ef migration history
- set a startup project : run the plugin UI menu on the start-up project which has the appsettings.json file
- set a migrations project : select a project if the ef migration is configured in a different project under the solution

## trobleshootings
### 'Cannot authenticate using Kerberos' issue doing EF Core database
This is because the integrated security is working on windows system, but not on a linux OS or VM or Docker, etc. 
So, change the integrated security option to 'false' and use userId and Password instead to access the database
e.g. Appsettings.json 
```
{
  "ConnectionString": "Server=(local);Database=<database name>Trusted_Connection=False;TrustServerCertificate=True;User Id=sa;Password=<your sa password>",
}
```
> ref: https://www.connectionstrings.com/sql-server/ 
