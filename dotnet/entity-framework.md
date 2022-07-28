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
