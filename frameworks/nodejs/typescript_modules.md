## Background
sometimes you might see the below error : 
```
ReferenceError: exports is not defined in ES module scope
```

This occurs because you are using ES modules (ECMAScript modules) instead of CommonJS modules. In ES modules, you should use export and import statements instead of module.exports and require.

## What is ES modules and CommonJs modules?
ES Modules (ECMAScript Modules) and CommonJS Modules are two different module systems used in JavaScript to organize and manage code.

### ES Modules (ECMAScript Modules)
ES Modules are the standardized module system introduced in ECMAScript 2015 (ES6).
They are designed to be more flexible and efficient than CommonJS modules.
ES Modules use import and export statements to include and export code.

### Key features
- Static structure: The module structure is determined at compile time, which allows for better optimization and tree-shaking.
- Asynchronous loading: ES Modules can be loaded asynchronously, which is beneficial for performance.
- Browser support: ES Modules are natively supported in modern browsers.

### CommonJS Modules
CommonJS is the module system used by Node.js. It uses require to include modules and module.exports or exports to export code.

[example code snipets]
```
// CommonJS
const express = require('express');
module.exports = app;

// ES Module
import express from 'express';
export default app;
```

### Key features
- Synchronous loading: Modules are loaded synchronously, which is suitable for server-side development.
- Widely used: CommonJS has been the standard module system in Node.js for a long time, and many existing Node.js projects and libraries use it.
- Simplicity: The CommonJS module system is simple and easy to understand.

### Differences and Usages
- Differences
Syntax: ES Modules use import and export, while CommonJS uses require and module.exports.
Loading: ES Modules support asynchronous loading, whereas CommonJS modules are loaded synchronously.
Scope: ES Modules have a static structure, which allows for better optimization, while CommonJS modules are more dynamic.
- Usage
ES Modules: Preferred for modern JavaScript development, especially for front-end development and when using modern build tools like Webpack, Rollup, or Babel.
CommonJS Modules: Commonly used in Node.js projects and for compatibility with existing Node.js libraries and tools.


## How to setup for CommonJs Modules in your nodejs project by typescript?
1. Configure TypeScript to use CommonJS modules
Update your tsconfig.json to use commonjs as the module system.

```
{
  "compilerOptions": {
    "module": "commonjs",
    // ...existing code...
  }
}
```

2. Ensure Node.js runs your code as CommonJS
If you want to keep using CommonJS syntax, make sure your package.json does not have "type": "module".
If it does, remove it or change it to "type": "commonjs".

3. Usage
```
const express = require('express');
module.exports = app;
```

| what's the benefits to use commonjs module? and why does the default of tsconfig file set with 'commonjs'?
  1. Compatibility: Many Node.js projects and libraries are written using CommonJS. Using "module": "commonjs" ensures compatibility with these projects and libraries.
  2. Stability: CommonJS has been around for a long time and is well-supported in the Node.js ecosystem. It provides a stable and reliable module system.
  3. Tooling: Many tools and build systems in the JavaScript ecosystem are designed to work with CommonJS modules.

## How to setup for ES Modules in the nodejs project?
1. Configure TypeScript to use ES modules
setting to "esnext" or "es6" in your tsconfig.json.
```
   {
  "compilerOptions": {
    "module": "esnext",
    "moduleResolution": "node",
    "target": "es2020",
    "outDir": "./dist",
    "rootDir": "./src",
    // ...existing code...
  },
  "include": ["src/**/*"]
}
```

2. ensure your package.json has "type": "module" to indicate that your project uses ES modules:
```
{
  // ...existing code...
  "type": "module",
  // ...existing code...
}
```

## Can I use the ES module systex with the commonjs module declaration?
Yes, TypeScript will compile the code including ES module systex to CommonJS syntax, so you don't need to change your import/export statements.

## ES modules are recommended in nodejs projects these days?
Yes, using ES Modules (ESM) is increasingly recommended even in Node.js projects these days. Node.js has added support for ES Modules starting from version 12, and it has become stable in later versions. Here are some reasons why you might want to use ES Modules in your Node.js projects:

- Standardization: ES Modules are part of the ECMAScript standard, making your code more consistent with modern JavaScript practices.
- Interoperability: Using ES Modules can make it easier to share code between front-end and back-end projects, as both can use the same module syntax.
- Future-Proofing: As the JavaScript ecosystem continues to evolve, ES Modules are likely to become the default module system. Adopting them now can help future-proof your code.
- Tooling Support: Modern build tools and transpilers like Webpack, Rollup, and Babel have excellent support for ES Modules, enabling advanced optimizations like tree-shaking.
