# nodejs project setup with typescript

## Quick blank project
- node install or update
  ```
  node -v
  nvm ls
  nvm use v[x.x]
  ```
- npm init -y
- (test) javascript code : src/index.js
  ```
  console.log('hello world')
  node src/index.js
  ```
- typescript install
  ```
  npm i -D typescript @types/node
  tsc -v
  tsc --init
  ```
  * npm i -g typescript (if necessary)
- update tsconfig.json
  - "outDir": "./dist" -> this will store the build outcomes into /dist folder
- (test) typescript code : src/index.ts
  ```
  mv index.js index.ts
  
  npx tsc             // (compile all accoridng to tsconfig spec)
  node dist/index.js  // execute buidl outcome 
  ```
  * tsc src/index.ts (direct compile a single file)
  * use ts-node if you want to use typescript REPL
- install server packages
  ```
  npm i express @types/express dotenv
  ```
- 

## Initial Node environment
- update yarn with a latest version
```
brew upgrade yarn
yarn -v
```

- migrate berry version of yarn
  - Make sure you're using Node 18+
  - Run corepack enable to activate Corepack
  - Go into your project directory
  - Run yarn set version berry
  - Convert your .npmrc and .yarnrc files into .yarnrc.yml (details here)
  - Run yarn install to migrate the lockfile
  - Commit all changes


- yarn init
> e.g. project name : simplix-apis
```
mkdir simplix-apis
cd simplix-apis
yarn init -y
```

- create yarnrc and config a module link as a node_modules
yarn will use pnp.cjs file to map the packages instead of node_modules. However, if you need to creaete node_modules, use nodeLinker

[.yarnrc.yml]
```
nodeLinker: node-modules

yarnPath: .yarn/releases/yarn-3.3.0.cjs // or set a specific yarn version for a teamwork
```
> if you run 'yarn' after this, it will delete pnp and generate node_modules

## initial code and run
- create a file 'src/index.js'
```
console.log('hello')
```

- run the project
```
node src/index.js
```

## typescript configuration
### basic configuration
- install typescript
```
yarn add -D typescript
```

```
// install typescript cli globally
npm install typescript -g
tsc -v

// generate typescript config file
tsc --init

// update some options at tsconfig.json
{
  "complierOptions": {
    "target": "es2020,
    "module": "commonjs",
    "moduleResolution": "node",
    "types": [
      "jest",
      "node"
    ], 
    "allowJs": true,
    "sourceMap": true,
    "outDir": "./dist",
    "preserveConstEnums": true,
    "isolatedModules": true,
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "preserveSymlinks": true,
    "forceConsistentCasingInFileNames": true,
    // "strict": true,   --> disable this
    "noImplicitAny": true,
    "skipLibCheck": true
  },
  "exclude": ["node_modules", "**/*.test.ts", "dist"]
}
```
- details
  - sourceMap : A source map is a file that maps your compiled JavaScript code back to your original TypeScript code. This is particularly useful for debugging purposes because it allows you to see the original TypeScript code in your browser's developer tools instead of the compiled JavaScript code.
  - outDir : location to store build outcomes

- extend a default configuration for node version.
install the @tsconfig/xxx to tune to a particular runtime environment. see https://github.com/tsconfig/bases

```
yarn add --dev @tsconfig/recommended
```

OR
```
// tsconfig.json
{
  "extends": "@tsconfig/node16/tsconfig.json",
  "compilerOptions": {
    "outDir": "dist"
  },
  "include": ["src"],
  "exclude": ["node_modules"]
}

// cli
yarn add -D @tsconfig/node16
```

- include type definition for nodejs
```
yarn add -D @types/node
```

### complie and run typescript project
- compile typescript and outcome will be in dist/
```
yarn tsc
```

- run the build outcome
```
node dist/indxt.js
```

OR
- run typescript file directly with package
```
yarn add -D ts-node
ts-node src/index.ts
```

### hot loading
- by nodemon or ts-node-dev
```
yarn add -D nodemon
yarn nodemon src/index.ts

// OR
yarn add -D ts-node-dev
yarn ts-node-dev --respawn src/index.ts
```

### code hierarchy with a first handler
```md
├── .yarn
├── package.json
├── tsconfig.json
├── dist
│   └── i[handler name]/index.js
├── src
    ├── [handler name]/index.ts
│   └── ...
│
```

[test handler]
```
// src/test-handler/index.ts
export const handler = () => {
  console.log('Test a handler')
  return 'Success.'
}
```

### integrate typescript with webpack
> ref: https://webpack.js.org/guides/typescript/ 
- install packages, and dependencies 
```
yarn add -D typescript ts-loader
yarn add -D webpack webpack-cli
yarn
```

- run with webpack cli (this will replace to use a webpack configuration: see the below webpack.config.js
```
npx webpack
```
> npx webpack will install webpack and webpack-cli before compile and run the handler


- config webpack
```
const path = require('path');

module.exports = {
  target: 'node',
  entry: {
    '[each handler name]': './src/[each handler name]/index.ts',
    ...
  },
  devtool: 'inline-source-map',
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/,
      },
    ],
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.js'],
  },
  output: {
    library: {
      type: 'commonjs2',
    },
    filename: '[name]/index.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```

- build a script
[pacakge.json]
```
{
  "name": "My Project",
  "version": "1.0.0",
  "description": "My APIs",
  "main": "index.js",
  "scripts": {
    "webpack:help": "webpack --help=verbose",
    "test:script": "node -e \"console.log(require('./events/[handler]-evnet.js'))\"",
    "build:dev": "webpack --mode=development --progress --profile --color",
    "build": "webpack --mode=production",
    ...
  }
...
}
```

```
yarn build:dev
```

### code hierarchy with a test event
```md
├── .yarn
├── package.json
├── tsconfig.json
├── dist
│   └── i[handler name]/index.js
├── src
    ├── [handler name]/index.ts
│   └── ...
├── events
    ├── [handler name]-event.js
    └── ...

```

example of [handler name]/index.ts
```
export const handler = async (ev: any) => {
  const userId = ev.queryStringParameters.[param-1]

  if (![param-1]) {
    return errorResponse(statusCodes.BAD_REQUEST, 'Missing [param-1]')
  }

  console.info('getting [handler's objects]: ', [param-1])

  const [data, errorMessage] = await get[handler name]By[param-1]([param-1])

  return errorMessage
    ? {
        statusCodes.INTERNAL_SERVER_ERROR,
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          errorMessage
        }),
        isBase64Encoded: false
      }
    : {
        statusCodes.OK,
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          data,
          message: errorMessage || 'Success'
        }),
        isBase64Encoded: false
      }
}

// event of handler
module.exports = {
  queryStringParameters: {
    [param-1]: "xxx",
  },
}

```

- test runner
[package.json]
```
{
...
  "scripts": {
    "webpack:help": "webpack --help=verbose",
    "test:script": "node -e \"console.log(require('./events/create-job-event.js'))\"",
    "build:dev": "webpack --mode=development --progress --profile --color",
    "build": "webpack --mode=production",
    "run:[handler name]": "yarn build:dev && node -e \"require('./dist/[handler name]').handler(require('./events/[handler name]-event.js'), {}).then(x => console.log(x));\""
  },
...
}
```

### Setup Jests on typescript via ts-jest
> ref: https://jestjs.io/docs/getting-started

- Basic setup
```
yarn add -D jest ts-jest @types/jest ts-node

// jest cli
npm install jest -g
jest --version

// jest init
main ?12 jest --init                                                                   
The following questions will help Jest to create a suitable configuration for your project

✔ Would you like to use Typescript for the configuration file? … yes
✔ Choose the test environment that will be used for testing › node
✔ Do you want Jest to add coverage reports? … yes
✔ Which provider should be used to instrument code for coverage? › babel
✔ Automatically clear mock calls, instances, contexts and results before every test? … yes

📝  Configuration file created at /Users/kyu/dev/deepeyes/simplix-apis/jest.config.ts
```
// jest.config.ts
{
  ...
  "preset": "ts-jest"
  ...
  transform: {
    '^.+\\.(ts|tsx)?$': 'ts-jest',
    "^.+\\.(js|jsx)$": "babel-jest",
  },
  ...
}
```
> In order for Jest to transpile TypeScript with ts-jest, you may also need to create a configuration file.
  
// package.json
{
  ...
  "scripts": {
    ...
    "test": "jest"
    ...
  },
  ...
}
```
>  @types/jest package provides types for Jest globals without a need to import them.
> @types/jest is a third party library maintained at DefinitelyTyped, hence the latest Jest features or versions may not be covered yet. Try to match versions of Jest and @types/jest as closely as possible. For example, if you are using Jest 27.4.0 then installing 27.4.x of @types/jest is ideal.
