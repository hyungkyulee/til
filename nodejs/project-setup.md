# nodejs project setup with typescript

## Initial Node environment
> e.g. project name : simplix-apis
```
mkdir simplix-apis
cd simplix-apis
yarn init -y
yarn set version stable
```

- config a module link as a node_modules
[.yarnrc.yml]
```
nodeLinker: node-modules

yarnPath: .yarn/releases/yarn-3.3.0.cjs
```
> yarn will use pnp.cjs file to map the packages instead of node_modules. However, if you need to creaete node_modules, use nodeLinker

## typescript configuration
### basic configuration
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
      "jest"
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

### code hierarchy
 |- .yarn
 |- package.json
 |- tsconfig.json
 |- dist
    |- [handler name]/index.js
 |- src
    |- [handler name]/index.ts
    |- ...

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
