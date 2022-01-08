# Gatsby website project with Typescript and Tailwind CSS

## Dev Environment

### Gatsby Cli
```
// yarn
yarn add gatsby-cli

// npm
npm i -g gatsby-cli

gatsby --version
```
> if gatsby and gatsby-cli major version are different (e.g. v3 and v4), update gatsby with a latest version
  ``` npm install gatsby@latest ```
> Please note: If you use npm 7 you’ll want to use the --legacy-peer-deps option when following the instructions in this guide. For example, the above command would be: ```npm install gatsby@latest --legacy-peer-deps```

### templated project
Gatsby + Typescript + Tailwind CSS + Jest
```
// create a new Gatsby site using gatsby-typescript-tailwind-starter
gatsby new my-tswind-starter https://github.com/jagdcake/gatsby-typescript-tailwind-starter

cd [project folder]
gatsby develop
```
> open the website on localhost:8080 as a default

### manually build up from the scratch
#### Gatsby Init
```
mkdir [project name]
cd [project name]
yarn init gatsby
...

yarn
```

#### Install essential dependencies
```
yarn add gatsby react react-dom
...
```

#### edit script, a first build, and launch app
```
// package.json
...
  "scripts": {
    "develop": "gatsby develop",
    "start": "gatsby develop",
    "build": "gatsby build",
    "serve": "gatsby serve",
    "clean": "gatsby clean"
  },
  "dependencies": {
    "gatsby": "^3.14.2",
    "react": "^17.0.1",
    "react-dom": "^17.0.1"
  }

...

// build static web output
yarn build

// index.js at src/pages/index.js
import React from 'react'

const Init = () => {
  return (
    <div>
      Index Page
    </div>
  )
}

export default Init


// launch dev app
yarn develop
```

#### other dependencies
```
// typescript
yarn add typescript gatsby-plugin-typescript
yarn add @types/react @types/react-dom @types/node eslint eslint-config-prettier eslint-plugin-prettier

// add 'gatsby-config.js'
module.exports = {
  siteMetadata: {
    siteUrl: "https://www.simplix.app",
    title: "Simply Website Template",
  },
  plugins: [
    `gatsby-plugin-typescript`,
  ],
};

// change 'index.js' to 'index.tsx'
```

```
// tailwindcss
yarn add tailwindcss autoprefixer 
yarn add @types/tailwindcss 
yarn add @tailwindcss/forms @tailwindcss/aspect-ratio

// add 'tailwind.config.js'
module.exports = {
  purge: [],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {},
  plugins: [],
}

// post CSS
yarn add postcss gatsby-plugin-postcss

// update 'gatsby-config.js'
...
plugins: [
  ...
  `gatsby-plugin-postcss`
],

// add 'postcss.config.js' and link plugin
module.exports = {
  plugins: [
    require('tailwindcss')('./tailwind.config.js'),
    require('autoprefixer'),
  ]
}

// update 'tailwind.config.js' importing plugins and color set
module.exports = {
  purge: {
    content: [
      './src/**/*.html',
      './src/**/*.js'
    ],
    options: {
      safelist: {
        standard: [
          /[company name]-[color1 name]-light$/, 
          /[company name]-[color2 name]$/, 
          /[company name]-[color3 name]-dark$/, 
        ]
      },
    },
  },
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend:{},
    colors: {
      [company name or site name]: {
        [color1 name]: {
          light: '#D9F0FC',
          DEFAULT: '#7CCDF5',
          dark: '#54ACE1',
        },
        [color2 name]: {
          light: '#3BA3D4',
          DEFAULT: '#0070BF',
          dark: '#005A93',
        },
        [color3 name]: {
          light: '#00000026',
          DEFAULT: '#757474',
          dark: '#383838'
        }
      }
    },
    fill: theme => theme('colors')
  },
  variants: {
    extend: {},
  },
  plugins: [
    require('@tailwindcss/forms'),
    require('@tailwindcss/aspect-ratio'),
  ],
}

// import classes in the most prior entry point: e.g. index.tsx
import * as React from 'react'
import "tailwindcss/dist/base.min.css"
import "tailwindcss/dist/components.min.css"
import "tailwindcss/dist/utilities.min.css"
import "tailwindcss/tailwind.css"
```
> on an error of yarn build with 'not found of tailwindcss/dist/xxx.min.css', link them with path of node_modules libraries
> e.g. 
> ```
> import "tailwindcss/base.css"
> import "tailwindcss/components.css"
> import "tailwindcss/utilities.css"
> import "tailwindcss/tailwind.css"
> ```


### manually build up from plugin
#### Gastby init project(js) with 'gatsby-plugin-typescript'
```
// -- package.json
...
"scripts": {
    "develop": "gatsby develop",
    "start": "gatsby develop",
    "build": "gatsby build",
    "serve": "gatsby serve",
    "clean": "gatsby clean"
  },
  "dependencies": {
    "gatsby": "^3.14.2",
    "gatsby-plugin-google-analytics": "^3.14.0",
    "gatsby-plugin-sitemap": "^4.10.0",
    "react": "^17.0.1",
    "react-dom": "^17.0.1"
  }
```

#### clean a project
- clean gatsby
```
$ gatsby clean                                                                              ✔  16s  12:06:08 
info Deleting .cache, public, /Users/kyu/dev/sekyee/sekyee-website/node_modules/.cache/babel-loader,
/Users/kyu/dev/sekyee/sekyee-website/node_modules/.cache/terser-webpack-plugin
info Successfully deleted directories

Hello! Will you help Gatsby improve by taking a four question survey?
It takes less than five minutes and your ideas and feedback will be very helpful.

Give us your feedback here: https://gatsby.dev/feedback
```

- if there is a set of below files, remove them
```
rm gatsby-node.js
rm gatsby-browser.js
rm gatsby-ssr.js
```

- remove node_modules

#### installing a set of initial dependencies for typescript
- Install Typescript, typings for React/Node, ESLint (and default config + prettier integration), and finally the Gatsby plugin for Typescript support:
> some of the dependencies might be pre-installed on starter project of gatsby
```
npm i typescript gatsby-plugin-typescript
npm install --save-dev @types/react @types/react-dom @types/node eslint eslint-config-prettier eslint-plugin-prettier
```
> if necessary (optional), please install more eslint dependencies for eslint-config-airbnb
  ```npx install-peerdeps --dev eslint-config-airbnb```

- update gatsby-config to enable typescript plugin
```
...
plugins: [
    {
      resolve: "gatsby-plugin-google-analytics",
      options: {
        ...
      },
    },
    ...
    `gatsby-plugin-typescript`
  ],
```

- update package.json (optional)
```
...
"scripts": {
    ...
    "lint": "eslint . --ext ts --ext tsx'",
    "test": "npm run type-check && npm run lint",
    "type-check": "tsc --pretty --noEmit"
  },
```

#### Tailwind CSS setup
> ref: https://www.gatsbyjs.com/docs/how-to/styling/tailwind-css/

- Tailwind CSS overview
a utility-first CSS framework for rapidly building custom user interfaces.

- three ways to use tailwind css in Gatsby project
  - PostCSS : to use 'className'
  - CSS in JS : to integrate Tailwind classes into Styled Components
  - SCSS : to support Tailwind classes in your SCSS files

- pre-requisit
```
npm install tailwindcss autoprefixer
npx tailwindcss init

// tailwind.config.js (* generated by npx tailwindcss init)
module.exports = {
  purge: [],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {},
  plugins: [],
}
```
> Since Tailwind does not automatically add vendor prefixes to the CSS it generates, we recommend installing autoprefixer to handle this
> tailwindcss has a build-in script(npx ... init) to create a tailwind config file.
> If you’d like to customize your Tailwind installation, generate a config file

- PostCSS setup
```
// install package
npm install postcss gatsby-plugin-postcss

// add plugin into [gatsby-config.js]
...
plugins: [`gatsby-plugin-postcss`],

// Create a [postcss.config.js], and link plugin
module.exports = {
  plugins: [
    require('tailwindcss'),
    require('autoprefixer'),
  ]
}

// create a CSS file [style.css]
@tailwind base;
@tailwind components;
@tailwind utilities;
/* customise inherited tailwind css components */
@layer components {
    .btn {
        @apply px-4 py-2 bg-blue-600 text-white rounded;
    }
}

// optimise a build on dev and production via [tailwind.config.js]
 module.exports = {
 purge: [
    './src/**/*.html',
    './src/**/*.js',
  ],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {},
  plugins: [],
}

```
> we can use ```@tailwind``` to use utilities, preflight, and functions; and ```@apply```
> if tailwindcss cannot be found on typescript, ```npm i --save-dev @types/tailwindcss```
> @tailwind directive to inject Tailwind’s base, components, and utilities styles -> ailwind will swap these directives out at build-time with all of the styles
  - if you're using 'postcss-import', use @import instead of @tailwind 
    ```
    @import "tailwindcss/base";
    @import "tailwindcss/components";
    @import "tailwindcss/utilities";
    ```
  - if you're working in a js framework like react or vue, you can import 'tailwindcss/tailwind.css' instead of creating CSS file ```import "tailwindcss/tailwind.css"```
> When building for production, be sure to configure the purge option to remove any unused classes for the smallest file size

- CSS in JS setup
- SCSS setup

- custom colors
> Purged CSS doesn't know to keep classes like "bg-test-xxx-xxx" because that string doesn't appear in the code, only "bg-" appears. it's needed to make sure any classes to Purge CSS to keep appear as complete strings in the css, they cannot be dynamically concatenated.
```
module.exports = {
  purge: {
    content: [
      './src/**/*.html',
      './src/**/*.js'
    ],
    options: {
      safelist: {
        standard: [
          /test-skyblue-light$/, /test-skyblue$/, /test-skyblue-dark$/,
          /test-blue-light$/, /test-blue$/, /test-blue-dark$/,
          /test-gray-light$/, /test-gray$/, /test-gray-dark$/
        ]
      },
    },
  },
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend:{},
    colors: {
      test: {
        skyblue: {
          light: '#7FCEF536',
          DEFAULT: '#7CCDF5',
          dark: '#54ACE1',
        },
        blue: {
          light: '#3094D4',
          DEFAULT: '#0070BF',
          dark: '#005A93',
        },
        gray: {
          light: '#00000026',
          DEFAULT: '#757474',
          dark: '#383838'
        }
      }
    },
    fill: theme => theme('colors')
  },
  variants: {
    extend: {},
  },
  plugins: [
    require('@tailwindcss/forms'),
  ],
}
```
