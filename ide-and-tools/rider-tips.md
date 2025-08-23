# Rider Tips on projects

## Git Handling
### Conflict management

- conflict happened on git merge
![image](https://user-images.githubusercontent.com/59367560/125325486-e5529380-e338-11eb-8100-e48ed9d57a36.png)

- resolve conflict
> Rider > Git > Resolve Conflict > double click on any conflict item on the list
![image](https://user-images.githubusercontent.com/59367560/125280834-6dbb3f00-e30d-11eb-9cee-d6ce32806d59.png)
![image](https://user-images.githubusercontent.com/59367560/125280990-9cd1b080-e30d-11eb-8387-5bd932acd90e.png)

- merge and apply
  - left : original code
  - right : source coming from your 
  - middle : target to be a result.
![image](https://user-images.githubusercontent.com/59367560/125281751-82e49d80-e30e-11eb-8848-85215a8a318b.png)

## Device Project Setting
- Manual Edit
> Rider > highlight Project on Explorer > F4 (or mouse right-click)

## Build and Run
- Build > Solution Rebuild
- set a device on top bar and run/debug application
![image](https://user-images.githubusercontent.com/59367560/125325807-4712fd80-e339-11eb-9643-9eda88c8f691.png)

## Rider Run from the terminal on Mac
- create rider script file on ```/usr/local/bin ```
- edit 'rider' script with ``` open -na "Rider.app" --args "$@" ```
- run 'rider' cli with solution file e.g. ``` ❯ rider abcProject.sln ```
