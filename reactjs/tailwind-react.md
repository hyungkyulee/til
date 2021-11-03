# Tailwind project on ReactJS webapp
ref: https://tailwindcss.com/docs/guides/create-react-app

### Setup Dev Environment
#### create an initial react app
```
// create a new project
npx create-react-app my-project
cd my-project

// install postcss and dependencies
npm install -D tailwindcss@npm:@tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9

// install CRACO to be able to configure Tailwind
npm install @craco/craco
```
> Create React App doesn’t support PostCSS 8 yet so you need to install the Tailwind CSS v2.0 PostCSS 7 compatibility build for now as we’ve shown above.

#### script change with Craco
```
  {
    // ...
    "scripts": {
     "start": "react-scripts start",
     "build": "react-scripts build",
     "test": "react-scripts test",
     "start": "craco start",
     "build": "craco build",
     "test": "craco test",
      "eject": "react-scripts eject"
    },
  }
```

#### create craco config script at root
```
// craco.config.js
module.exports = {
  style: {
    postcss: {
      plugins: [
        require('tailwindcss'),
        require('autoprefixer'),
      ],
    },
  },
}
```

#### create tailwind config script at root by cli
```
npx tailwindcss-cli@latest init

Need to install the following packages:
  tailwindcss-cli@latest
Ok to proceed? (y) y
...
```

#### Configure Tailwind to remove unused styles in production
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
  plugins: [],
}
```

#### Include Tailwind in your CSS
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

### 
