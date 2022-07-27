# Tailwind project on ReactJS webapp
ref: https://tailwindcss.com/docs/guides/create-react-app

### Setup Dev Environment
#### create an initial react app
```
// create a new project
// for typescript template : ```npx create-react-app my-project --template typescript```
npx create-react-app my-project
cd my-project

// gitignore for reactjs + typescript + tailwindcss
// please see appendix

// If you are new to React, we recommend using Create React App. 
// It is ready to use and ships with Jest! You will only need to add react-test-renderer for rendering snapshots.
yarn add --dev react-test-renderer
```

#### Update tsconfig with a separated path options
[tsconfig.path.json]
```
{
  "compilerOptions": {
    "baseUrl": "./src",
    "paths": {
      "@pages": ["pages"],
      "@pages/*": ["pages/*"],
      "@components": ["components"],
      "@components/*": ["components/*"]
    }
  }
}
```

[tsconfig.json]
```
{
  "extends": "./tsconfig.path.json",
  "compilerOptions": {
  ...
```

#### install CRACO to be able to optimise a configure
```
yarn add @craco/craco
yarn add -D ts-jest
```

1) add craco config file location at package.json
2) change scripts from 'react-scripts' to 'craco'
[package.json] 
```
  {
    // ...
    "
    "scripts": {
     // "start": "react-scripts start",
     // "build": "react-scripts build",
     // "test": "react-scripts test",
     "start": "craco start",
     "build": "craco build",
     "test": "craco test",
      "eject": "react-scripts eject"
    },
  }
```

[craco.config.js]
```
onst { pathsToModuleNameMapper } = require('ts-jest');
const { compilerOptions } = require('./tsconfig.path.json');

module.exports = {
  webpack: {
    alias: {
      '@components': path.resolve(__dirname, 'src/components'),
      '@pages': path.resolve(__dirname, 'src/pages'),
    }
  },
  jest: {
    configure: {
      preset: 'ts-jest',
      moduleNameMapper: pathsToModuleNameMapper(compilerOptions.paths, {
        prefix: '<rootDir>/src/',
      }),
    },
  },
};
```
> ref: https://dev.to/brandonwie/absolute-paths-in-cra-setting-using-craco-feat-typescript-and-ts-jest-2301

#### webpack configuration
```
yarn add -D webpack-bundle-analyzer
```

create report with node
[analyzer.js]
```
const webpack = require("webpack")
const BundleAnalyzerPlugin =
  require("webpack-bundle-analyzer").BundleAnalyzerPlugin
const webpackConfigProd = require("react-scripts/config/webpack.config")(
  "production"
)

webpackConfigProd.plugins.push(new BundleAnalyzerPlugin())

webpack(webpackConfigProd, (err, stats) => {
  if (err || stats.hasErrors()) {
    console.error(err)
  }
})
```

#### Tailwind and PostCSS
```
// if any dependency issue shows, npm install -D tailwindcss@npm:@tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9
npm install -D tailwindcss postcss autoprefixer
// (OR)
yarn add -D tailwindcss postcss autoprefixer

// install the tailwind forms packages because most applications will at some point require forms.
yarn add @tailwindcss/forms @tailwindcss/aspect-ratio
```
> Create React App doesn’t support PostCSS 8 yet so you need to install the Tailwind CSS v2.0 PostCSS 7 compatibility build for now as we’ve shown above.

#### create tailwind config scripts
```
npx tailwindcss init -p
```

#### Configure Tailwind and postcss
```
// tailwind.config.js
module.exports = {
 purge: [],
 purge: ['./src/**/*.{js,jsx,ts,tsx}', './public/index.html'],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: [
    require('@tailwindcss/forms'),
  ],
}
```
> purge is deprecated and use contents
> example
  [postcss.config.js]
  ```
  module.exports = {
    plugins: [
      require('tailwindcss')('./tailwind.config.js'),
      require('autoprefixer'),
    ]
  }
  ```
  
  [tailwindcss.config.js]
  ```
  module.exports = {
    content: [
      './public/**/*.html',
      './src/**/*.{js,jsx,ts,tsx}'
    ],
    darkMode: false, // or 'media' or 'class'
    theme: {
      extend:{
        colors: {
          navien: {
            blue: {
              light: '#1f57cb', // navienapp mobile theme primary.500
              DEFAULT: '#0E3173', // navienapp mobile theme primary.700
              dark: '#051d46', // navienapp mobile theme primary.800
            },
            orange: {
              light: '#',
              DEFAULT: '#FF730F',
              dark: '#',
            },
            gray: {
              light: '#e5e7eb', // nativebase coolGray.200
              DEFAULT: '#6b7280', // nativebase coolGray.500
              dark: '#1f2937' // nativebase coolGray.800
            }
          }
        },
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
  ```
  > if color is out of the 'extend object', default color theme cannot be inherited.

#### Include Tailwind in your CSS and apply it into the code
```
//index.css
@tailwind base;
@tailwind components;
@tailwind utilities;

body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, 'Courier New',
    monospace;
}
```

[app.tsx]
```
...
<h1 className="text-3xl font-bold underline">
  Hello world!
</h1>
...
```


### Add TypeScript to the Project
#### install dependencies
```
npm install --save typescript @types/node @types/react @types/react-dom @types/jest

// OR
yarn add -D typescript @types/node @types/react @types/react-dom @types/jest
```

#### typescript config by cli
```
npx tsc --init
```

#### change the extension to .tsx/ts
