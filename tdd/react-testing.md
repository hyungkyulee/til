# React Testing on React Testing Library and Jest
```zsh
```

## React Testing Library (RTL)
Officieal doc: https://testing-library.com/docs/react-testing-library/intro/

### Why RTL
RTL is to resolve the problem which developers have in Enzyme. 
Problem is any component modification can affect to test rewriting and cause to a slow development and productivity.

### Install dependencies
- Jest framework (env, cli, dom simulator, mock, functions, spying utilities, etc) and transpiler of JS files in tests
```zsh
npm i -D jest babel-jest
```

- testing libraries: APIs on DOM testing library(/react), custom dom element matchers(/jest-dom), advanced simulation utilities of browser interactions than built-in fireEvent(/user-event) 
```zsh
npm i -D @testing-library/jest-dom @testing-library/react @testing-library/user-event
```

### Jest configuration
- jest is a config free on javascript project
- here is an additional configuration from Mirco Bellagamba : https://dev.to/mbellagamba
- example config file : [proj root]/jest.config.js
```js
module.exports = {
  verbose: true,
  roots: ["<rootDir>/src"],
  collectCoverageFrom: ["src/**/*.{js,jsx,ts,tsx}", "!src/**/*.d.ts"],
  setupFilesAfterEnv: ["<rootDir>/test/setupTests.js"],
  testMatch: [
    "<rootDir>/src/**/__tests__/**/*.{js,jsx,ts,tsx}",
    "<rootDir>/src/**/*.{spec,test}.{js,jsx,ts,tsx}",
  ],
  testEnvironment: "jsdom",
  transform: {
    "^.+\\.(js|jsx|mjs|cjs|ts|tsx)$": "<rootDir>/node_modules/babel-jest",
    "^.+\\.css$": "<rootDir>/test/config/cssTransform.js",
    "^(?!.*\\.(js|jsx|mjs|cjs|ts|tsx|css|json)$)":
      "<rootDir>/test/config/fileTransform.js",
  },
  transformIgnorePatterns: [
    "[/\\\\]node_modules[/\\\\].+\\.(js|jsx|mjs|cjs|ts|tsx)$",
    "^.+\\.module\\.(css|sass|scss)$",
  ],
  moduleFileExtensions: [
    "web.js",
    "js",
    "web.ts",
    "ts",
    "web.tsx",
    "tsx",
    "json",
    "web.jsx",
    "jsx",
    "node",
  ],
  resetMocks: true,
};
```

- Jest setup config : [proj root]/test/setupTests.js
```js
import "@testing-library/jest-dom";
```

- CSS transformer : [proj root]/test/config/cssTransform.js
> turning style imports into empty objects
> won't import real CSS files
```js
"use strict";

module.exports = {
  process() {
    return "module.exports = {};";
  },
  getCacheKey() {
    // The output is always the same.
    return "cssTransform";
  },
};
```

- fileTransform : [proj root]/test/config/fileTransform.js
  - won't import real files 
  - Turning SVG files into Component or string. In our app we could import SVG both with import svg from '../path/to/asset.svg' and import { ReactComponent as Asset } from '../path/to/asset.svg'.
  - Turning other assets (images, videos, etc.) as strings. 
```js
"use strict";

const path = require("path");
const camelcase = require("camelcase");

module.exports = {
  process(src, filename) {
    const assetFilename = JSON.stringify(path.basename(filename));

    if (filename.match(/\.svg$/)) {
      const pascalCaseFilename = camelcase(path.parse(filename).name, {
        pascalCase: true,
      });
      const componentName = `Svg${pascalCaseFilename}`;
      return `const React = require('react');
      module.exports = {
        __esModule: true,
        default: ${assetFilename},
        ReactComponent: React.forwardRef(function ${componentName}(props, ref) {
          return {
            $$typeof: Symbol.for('react.element'),
            type: 'svg',
            ref: ref,
            key: null,
            props: Object.assign({}, props, {
              children: ${assetFilename}
            })
          };
        }),
      };`;
    }

    return `module.exports = ${assetFilename};`;
  },
};
```

- command script for jest at package.json
```json
{
  "scripts": {
    :
    :
    "test": "jest"
  }
}

#### Error case : Support for the experimental syntax 'jsx' isn't currently enabled
install Bable packages and packge json updated
- npm i @babel/preset-react @babel/preset-env
- npm i @babel/plugin-transform-react-jsx

[package.json]
```zsh
...,
  "devDependencies": {
  :
  :
},
  "babel": {
    "presets": [
      "@babel/preset-react",
      "@babel/preset-env"
    ],
    "plugins": [
      "@babel/plugin-transform-react-jsx"
    ]
  }
  :
  :
```

### Run Test
```zsh
npm test -- --watch

:
:

No tests found related to files changed since last commit.
Press `a` to run all tests, or run Jest with `--watchAll`.

Watch Usage
 › Press a to run all tests.
 › Press f to run only failed tests.
 › Press p to filter by a filename regex pattern.
 › Press t to filter by a test name regex pattern.
 › Press q to quit watch mode.
 › Press Enter to trigger a test run.

```
