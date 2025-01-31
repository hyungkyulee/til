## Init Account ##
signup corp user email
create organisation
invite members


## New Project ##
create a new project
success message will show the below steps
- new project
  ```
  npm install --global eas-cli && npx create-expo-app thirtywears && cd thirtywears && eas init --id [project id]
  ```
- link to existing project
  ```
  npm install --global eas-cli && eas init --id [project id]
  ```

link github repo

Login to your expo eaccount
```
eas login
```

configure the project
```
```


## Trouble-shooting ##
if you can see the below error, relogin
```
You don't have the required permissions to perform this operation.

This can sometimes happen if you are logged in as incorrect user.
Run eas whoami to check the username you are logged in as.
Run eas login to change the account.
```
