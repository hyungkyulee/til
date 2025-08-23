# Entity Framework

## EF Migrations
https://docs.microsoft.com/en-us/ef/core/managing-schemas/migrations/?tabs=dotnet-core-cli

### Add Migration
1) Add Entity (Class)
2) Add EntityConfiguration 
3) Add it onto Context
4) do 'add-migration'
5) do 'update-database'

> if something is wrong, you can remove last migration.

> when you handle more than one entity, it will be added into one 'add-migration' job

> without adding DbSet with a new entity, migration will not create table for the entity

> sometimes, after removing migration, if add migration is not applied a new fix, do update-database first.

[ Example ]
```c#
// ***
// Proj.DataAccess/Entities/Blog.cs
// ***
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }
}

// ***
// Proj.DataAccess/Entities/BlogConfiguration.cs
// ***
//  Noe : Group Congiration => To reduce the size of the OnModelCreating method 
//        all configuration for an entity type can be extracted 
//        to a separate class implementing IEntityTypeConfiguration<TEntity>.
public class BlogEntityTypeConfiguration : IEntityTypeConfiguration<Blog>
{
    public void Configure(EntityTypeBuilder<Blog> builder)
    {
        builder
            .Property(b => b.Url)
            .IsRequired();
    }
}

// ***
// ProjContext.cs
// ***
public class ProjContext : DbContext
{
  public projContext() {}
  
  public ProjContext(IserviceProvider serviceProvider, DbContextOptions<ProjContext> options) : base(options)
  {
    var configuration = serviceProvider.getRequiredService<IConfigurationRoot>();
    var dbAccessTokenResource = configuration[" name of proj resource definition "];
  }
  
  ...
  public virtual DbSet<Blog> Blogs { get; set; }
  
  protected override void OnModelCreating(ModelBuilder modelBuilder)
  {
    modelBuilder.HasAnnotation( ... );
    
    ...
    
    modelBuilder.ApplyConfiguration(new BlogConfiguration());
    ...
  }
}
```

### other examples
#### make it nullable with foreign-key
https://stackoverflow.com/questions/13650257/adding-a-foreign-key-with-code-first-migration 


