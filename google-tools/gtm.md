## GTM Integration
Google Site : https://tagmanager.google.com/

### Overview
#### Terms 
- A tag is a piece of code that must be fired on a website under certain circumstances. It can be a tracking code
- A trigger is a condition that cause your tag to fire when certain events occur
- A variable is an attribute that can be used to simplify and automate your tag configurations.

#### GTM vs GA
GA is one of tag interfaces of GTM

### Container Setup
- Add a new container : Account Name (e.g. business name or client name) + Container name (e.g. project name) + Target platform (e.g. web or app)
- Get code-snippet and add this code into index.tsx(or jsx)
- Check the container if it's working : Tag Manager > Select container of account >  Workspace > Preview
- Add a Tag :
  - Tag Manager > Select container of account >  Workspace > Tags > New
  - Complete 'Tag configuration' with type and measurement Id of GA
  - Complete 'Triggering' with All pages or other events

### Setup Environment
- Tag Manager > Select container of account > Admin > Environments > New
- Add a custom Environments for nonprod environments (e.g. dev, test, preprod) with a destination endpoint 
  * tick the 'Enable Debugging by Default' for the nonprod environment.
- copy the code snippet from 'Actions' > 'Get snippet' of each Environment Name
  
### Setup Variables 
- Tag Manager > Select container of account >  Workspace > Variables > User-Defined Variables > New
- Add a contant variable for Dev environment (with a value for the Dev property of GA)
- Add a contant variable for Prod environment (with a value for the Prod property of GA)
- Add a loopup table variable to join them

### Test
Test it via "Preview" of workspace after put the code snippet on the project <head> tag

### Project Pipeline
Configure the different Auth and Preview according to the project pipeline env.
