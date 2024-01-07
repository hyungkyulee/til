0. create-react-app
```
npx create-react-app my-app --template typescript
```

```
npm install --save typescript @types/node @types/react @types/react-dom @types/jest
```

2. install packages
   ```
   $ yarn add -D tailwindcss postcss autoprefixer
   $ yarn add -D css-loader postcss-loader postcss-preset-env
   $ yarn add -D @tailwindcss/aspect-ratio @tailwindcss/forms
   $ yarn add @headlessui/react @heroicons/react
   ```
3. setting of webpack.config.js
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
            include: path.resolve(__dirname, 'src'),
            use: ['style-loader', 'css-loader', 'postcss-loader'],
          },
        ],
      },
     resolve: {
       extensions: [".tsx", ".ts", ".js"],
     },
     // target: 'electron-renderer', // or 'web' for web
     output: {
       filename: "bundle.js",
       path: path.resolve(__dirname, "dist"),
     },
     plugins: [
       new HtmlWebpackPlugin({
         // HtmlWebpackPlugin simplifies creation of HTML files to serve your webpack bundles
         template: path.join(__dirname, 'public', 'index.html'),
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

5. create and setting of tailwind.config.js
   ```
   $ npx tailwindcss init -p
   ```
7. postcss.config.js
   ```
   const tailwindcss = require('tailwindcss');
   const autoprefixer = require('autoprefixer');
   module.exports = {
     plugins: [
       tailwindcss,
       autoprefixer
     ],
   };
   ```
   > you shouldn't do this because it will show error
   ```
   plugins: [
      tailwindcss('./tailwind.config.cjs'),
      autoprefixer
   ],
   ```
   ![image](https://github.com/hyungkyulee/til/assets/59367560/aeab3b03-9006-47b5-8954-9c26c841da27)


9. create and import './style.css'
   [src/style.css]
   ```
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   ```

   [src/index.tsx]
   ```
   import React from "react";
   import { createRoot } from "react-dom/client";
   import './style.css';

   import App from "./App";
   ```
   
10. create public files and Link our bundle.js in the HTML file
    [public/index.html]
    ```
    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta http-equiv="origin-trial" content="AjskG7XjAzhXrbO5iSypw0OSMflgB2aEgoT9BuzVaQngLsZbHPNipBOjM5tTKu6K+S0lotXG8JBfV/1QUGK2iA8AAABgeyJvcmlnaW4iOiJodHRwOi8vbG9jYWxob3N0OjgwODAiLCJmZWF0dXJlIjoiVW5yZXN0cmljdGVkU2hhcmVkQXJyYXlCdWZmZXIiLCJleHBpcnkiOjE3MDk4NTU5OTl9">
        <!-- <link rel="stylesheet" href="globals.css" /> -->
        <title>Document</title>
      </head>
      <body>
        <div id="root"></div>
        <div id="content"></div>
        <script src="bundle.js"></script>
      </body>
      <html></html>
    </html>
    ```
    
12. build script
    [package.json]
    ```
    ...
      "scripts": {
        "start": "webpack-cli serve --mode=development --env development --open --hot",

    ...
      "devDependencies": {
        "@tailwindcss/aspect-ratio": "^0.4.2",
        "@tailwindcss/forms": "^0.5.6",
        "autoprefixer": "^10.4.16",
        "css-loader": "^6.8.1",
        
        "tailwindcss": "^3.3.3",
        "postcss": "^8.4.30",
        "postcss-loader": "^7.3.3",
        "postcss-preset-env": "^9.1.4",
    ...
    ```

