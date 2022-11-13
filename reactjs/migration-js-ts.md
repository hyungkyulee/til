## Write a configuration for typescript
- add tsconfig.json
- template and values
```
{
  "compilerOptions": {
    /* Language and Environment */
    "target": "ES2020",                                     /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */

    /* Modules */
    "module": "CommonJS",                                /* Specify what module code is generated. */
    "moduleResolution": "node",                       /* Specify how TypeScript looks up a file from a given module specifier. */
    "types": [
      "jest"
    ],                                      /* Specify type package names to be included without being referenced in a source file. */

    /* JavaScript Support */
    "allowJs": true,                                  /* Allow JavaScript files to be a part of your program. Use the `checkJS` option to get errors from these files. */

    /* Emit */
    "sourceMap": true,                                /* Create source map files for emitted JavaScript files. */
    "outDir": "./dist",                                   /* Specify an output folder for all emitted files. */
    "preserveConstEnums": true,                       /* Disable erasing `const enum` declarations in generated code. */

    /* Interop Constraints */
    "isolatedModules": true,                       /* Ensure that each file can be safely transpiled without relying on other imports. */
    "allowSyntheticDefaultImports": true,             /* Allow 'import x from y' when a module doesn't have a default export. */
    "esModuleInterop": true,                             /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables `allowSyntheticDefaultImports` for type compatibility. */
    "forceConsistentCasingInFileNames": true,            /* Ensure that casing is correct in imports. */
##
    /* Type Checking */
    // "strict": true,                                      /* Enable all strict type-checking options. */
    "noImplicitAny": true,                            /* Enable error reporting for expressions and declarations with an implied `any` type.. */
    // "strictNullChecks": true,                         /* When type checking, take into account `null` and `undefined`. */
    // "strictFunctionTypes": true,                      /* When assigning functions, check to ensure parameters and the return values are subtype-compatible. */
    // "strictBindCallApply": true,                      /* Check that the arguments for `bind`, `call`, and `apply` methods match the original function. */
    // "strictPropertyInitialization": true,             /* Check for class properties that are declared but not set in the constructor. */

    /* Completeness */
    "skipLibCheck": true                                 /* Skip type checking all .d.ts files. */
  },
  "include": ["./src/**/*"],
  "exclude": ["node_modules", "**/*.test.ts", "dist"]
}
```
## Integrate with buil tools
### Gulp (TBD)
### Webpack
it's a simple way with ts-loader, combined with source-map-loader for easier debugging.
- install the packages
```
yarn add ts-loader source-map-loader
```

- webpack configuration
> merge the below options into your existing webpack.config.js
> ts-loader will need to run prior to any other loader that deals with .js files.
```
module.exports = {
  entry: "./src/index.ts",
  output: {
    filename: "./dist/bundle.js",
  },
  // Enable sourcemaps for debugging webpack's output.
  devtool: "source-map",
  resolve: {
    // Add '.ts' and '.tsx' as resolvable extensions.
    extensions: ["", ".webpack.js", ".web.js", ".ts", ".tsx", ".js"],
  },
  module: {
    rules: [
      // All files with a '.ts' or '.tsx' extension will be handled by 'ts-loader'.
      { test: /\.tsx?$/, loader: "ts-loader" },
      // All output '.js' files will have any sourcemaps re-processed by 'source-map-loader'.
      { test: /\.js$/, loader: "source-map-loader" },
    ],
  },
  // Other options...
};
```

## Install some type declarations
```
yarn add @types/node @types/jest @types/luxon -D
```
