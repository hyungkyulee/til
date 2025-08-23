# npm Project with Typescript

## initialise an npm project
- npm init
> you can skip an init process of 'npm init' with ``` npm init -y ```

## Instal Dependencies

```
npm install typescript
npm install tslint
```

## configure typescript
- ts compiler configuration
```
tsc --init
```
[tsconfig.json]
```
{
  "compilerOptions": {
    "module": "commonjs",
    "esModuleInterop": true,
    "target": "es6",
    "moduleResolution": "node",
    "sourceMap": true,
    "outDir": "dist"
  },
  "lib": ["es2015"]
}
```

- tslint initiailsation
> configure typescript linting for the project and add 'no-console' rule on tslint.json because ts linter prevents the use of debugging with console statements by default
```
./node_modules/.bin/tslint --init
```
[tslint.json]
```
{
  "defaultSeverity": "error",
  "extends": ["tslint:recommended"],
  "jsRules": {},
  "rules": {
    "no-console": false
  },
  "rulesDirectory": []
}
```

## Customise package.json
- set a main file path : 'dist/app.js'
- update 'start' script : ts compile and run node of js outcome
[package.json]
```
{
  "name": "ts-npm-proj",
  "version": "1.0.0",
  "description": "",
  "main": "dist/app.js",
  "scripts": {
    "start": "tsc && node dist/app.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "tslint": "^6.1.3",
    "typescript": "^4.4.3"
  }
}
```

## Run project
- create a source folder file and an entry file
- build and run
```
mkdir src
touch src/app.ts
...
npm start
```

![image](https://user-images.githubusercontent.com/59367560/135727554-eb72b902-78f5-42a0-be39-2ef75a28d3ba.png)

