# Vanila React 18 + typescript + webpack + eslint

## Structure
dist/  # Build outputs (not in source control)
public/
  index.html
  globals.css
src/
  components/
  index.tsx
  App.tsx
.yarnrc.yml
.eslintrc.js
tsconfig.json
webpack.config.ts

## Installation and Configuration

1.project initialize
  ```yarn init -y```

2. install react & typescript
   ```yarn add react react-dom```
   ```
   yarn add -D @types/node @types/react @types/react-dom @types/html-webpack-plugin
   yarn add -D typescript ts-loader ts-node
   ```
4. configure typescript
   ```yarn tsc```
   
   [tsconfig.json]
   ```
    {
      "compilerOptions": {
        "module": "commonjs",
        "moduleResolution": "node",
        "target": "ESNext",
        "removeComments": true,
        "allowSyntheticDefaultImports": true,
        "jsx": "react",
        "allowJs": true,
        "baseUrl": "src",
        "esModuleInterop": true,
        "resolveJsonModule": true,
        "downlevelIteration": true,
        "paths": {
          "*": ["src/*"]
        },
        "lib": [
          "esnext",
          "dom",
          "dom.iterable"
        ],
        "sourceMap": true,
        "noImplicitAny": false,
      },
      "exclude": ["node_modules", "build"],
      "include": [
        "./src",
        "webpack.config.ts"
      ]
    }
   ```
6. create index.tsx and App.tsx
   ```
   // [src/index.tsx]
   import React from 'react';
   import ReactDOM from 'react-dom';
   import './index.css';
   import App from './App';

   ReactDOM.render(
     <React.StrictMode>
       <App />
     </React.StrictMode>,
     document.getElementById('root')
   );

   [src/App.tsx]
   import React, { useEffect } from 'react';

   function App() {
     return (
       <div className="App">
         <header className="App-header">
           {/* <img src={logo} className="App-logo" alt="logo" /> */}
           <p>
             Edit <code>src/App.tsx</code> and save to reload.
           </p>
           <a
             className="App-link"
             href="https://reactjs.org"
             target="_blank"
             rel="noopener noreferrer"
           >
             Learn React
           </a>
         </header>
       </div>
     );    
   }

   export default App;
   ```
7. install webpack & configure
   ```
   yarn add -D style-loader node-sass sass-loader
   yarn add -D webpack webpack-cli html-webpack-plugin webpack-dev-server
   yarn add -D @types/webpack @types/webpack-dev-server
   ```

   [webpack.config.ts]
   ```
    import path from "path";
    import { Configuration, DefinePlugin } from "webpack";
    import HtmlWebpackPlugin from "html-webpack-plugin";
    
    const webpackConfig = (): Configuration => ({
      mode:
        (process.env.NODE_ENV as "production" | "development" | undefined) ??
        "development",
      entry: "./src/index.tsx",
      module: {
        rules: [
          {
            test: /.tsx?$/,
            use: "ts-loader",
            exclude: /node_modules/,
          },
          {
            test: /\.css$/,
            use: ["style-loader", "css-loader"],
          },
        ],
      },
      resolve: {
        extensions: [".tsx", ".ts", ".js"],
      },
      output: {
        filename: "bundle.js",
        path: path.resolve(__dirname, "dist"),
      },
      plugins: [
        new HtmlWebpackPlugin({
          // HtmlWebpackPlugin simplifies creation of HTML files to serve your webpack bundles
          template: "./public/index.html",
        }),
        // DefinePlugin allows you to create global constants which can be configured at compile time
        new DefinePlugin({
          "process.env": process.env.production || !process.env.development,
        }),
        // new ForkTsCheckerWebpackPlugin({
        //   // Speeds up TypeScript type checking and ESLint linting (by moving each to a separate process)
        //   eslint: {
        //     files: "./src/**/*.{ts,tsx,js,jsx}",
        //   },
        // }),
      ],
    });
    
    export default webpackConfig;
   ```
9. install eslint & configure
   ```
   yarn add -D eslint eslint-plugin-import eslint-plugin-react eslint-plugin-react-hooks
   yarn add -D @typescript-eslint/eslint-plugin @typescript-eslint/parser

   npm init @eslint/config
   ```

   [.eslintrc.js]
   ```
    module.exports = {
        "env": {
            "browser": true,
            "es2021": true
        },
        "extends": [
            "eslint:recommended",
            "plugin:react/recommended",
            "plugin:@typescript-eslint/recommended"
        ],
        "overrides": [
        ],
        "parser": "@typescript-eslint/parser",
        "parserOptions": {
            "ecmaVersion": "latest",
            "sourceType": "module"
        },
        "plugins": [
            "react",
            "@typescript-eslint"
        ],
        "rules": {
        }
    }
   ```

   - lint check
   ```
   yarn lint
   ```

12. script
    [package.json]
    ```
    ...
    "scripts": {
      "start": "webpack-cli serve --mode=development --env development --open --hot",
      "build": "webpack --mode=production --env production --progress",
      "lint": "eslint src/**/*.{ts,tsx} --quiet --fix"
    },
    ```
    - run react
    ```
    yarn
    yarn start
    ```
