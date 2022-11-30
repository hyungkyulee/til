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

- run with webpack cli
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
