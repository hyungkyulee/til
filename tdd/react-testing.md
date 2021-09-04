# React Testing on React Testing Library and Jest
React Testing Library is not an alternative to Jest, but an mutual supporter which needs each other.

## Jest
- Jest is a test runner
- Jest's test runner matches all files with a test.js (or spec.js) suffix by default.
- cli : ``` npm test ```

*** example ***
[package.json]
```
{
  :
  "dependencies": {
    :
    "@testing-library/jest-dom": "^5.14.1",
    "@testing-library/react": "^11.2.7",
    "@testing-library/user-event": "^12.8.3",
    :
```

[Hello.js]
```
const Hello = () => {
    return (
        "Hello"
    )
}

export default Hello
```

[Hello.spec.js]
```
import Hello from "./Hello.js"

describe("my react component", () => {
  test("is working as expected", () => {
    expect(Hello()).toBe("Hello");
  });
});
```

[Test result]
```
 PASS  src/componets/Hello.spec.js
  my react component
    ✓ is working as expected (2 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        1.549 s, estimated 2 s
Ran all test suites related to changed files.

Watch Usage
 › Press a to run all tests.
 › Press f to run only failed tests.
 › Press q to quit watch mode.
 › Press p to filter by a filename regex pattern.
 › Press t to filter by a test name regex pattern.
 › Press Enter to trigger a test run.
```

## React Testing Library (RTL)
React Testing Library (RTL) by Kent C. Dodds got released as alternative to Airbnb's Enzyme.
Officieal doc: https://testing-library.com/docs/react-testing-library/intro/

*** example ***

[Hello.js]
```
import React from 'react'

const message = 'This is the RTL rendering test'

const Hello = () => {
    return (
        <div>{message}</div>
    )
}

export default Hello
```

[Hello.spec.js]
```
import React from 'react'
import { render, screen } from '@testing-library/react'

import Hello from "./Hello.js"

describe("Temp TEST-SUITE", () => {
  test("renders Hello component", () => {
    render(<Hello />)

    screen.debug()
  })
})
```

[Test result]
```
 PASS  src/componets/Hello.spec.js
  Temp TEST-SUITE
    ✓ renders Hello component (74 ms)

  console.log
    <body>
      <div>
        <div>
          This is the RTL rendering test
        </div>
      </div>
    </body>
    
    /Users/kyu/dev/btm/voting-webapp/src/componets/Hello.spec.js:10:12
       8 |     render(<Hello />)
       9 |
    > 10 |     screen.debug()
         |            ^

      at logDOM (node_modules/@testing-library/react/node_modules/@testing-library/dom/dist/pretty-dom.js:80:13)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        2.101 s
Ran all test suites related to changed files.

Watch Usage: Press w to show more.
```

### Why RTL
RTL is to resolve the problem which developers have in Enzyme. 
Problem is any component modification can affect to test rewriting and cause to a slow development and productivity.

### Install dependencies
> this part is nomally and automatically installed together on projects created with ``` npx create-react-app my-app ```

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
    "xxx": "xxx",
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
