## Entity Framework nuget package



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