# Frontend Web Dev with ReactJS

---
## Dev Environment Overview
---
### Packages installed
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
brew upgrade (or update)
brew doctor
brew --version

export PATH="/usr/local/bin:$PATH"
```

```bash
brew install node
brew cask install adoptopenjdk8
java -version
```


---
## Basic Reactjs Webapp development environment
---
### Get started
```bash
$ cd webapp 
$ npx create-react-app ./
$ npm i
$ npm start
```

### .gitignore for ReactJS project
```
# See https://help.github.com/articles/ignoring-files/ for more about ignoring files.

# dependencies
/node_modules
/.pnp
.pnp.js

# testing
/coverage

# production
/build

# misc
.DS_Store
.env.local
.env.development.local
.env.test.local
.env.production.local

npm-debug.log*
yarn-debug.log*
yarn-error.log*
```

### Basic packages (React bootstrap, sass
```bash
npm install react-bootstrap bootstrap --save
npm install node-sass --save
```
> error: Node Sass version 6.0.1 is incompatible with ^4.0.0 || ^5.0.0 -> install sass-loader package ``` npm i sass-loader@latest --save ```
> issue keeps going on : 1) remove package-lock.json and node_modules, 2) yarn (rather than npm i), 3) yarn start. ( * sometimes npm is suffering from the known issue on packages/dependencies)

- name src/App.css to src/App.scss 
- update src/App.js to import src/App.scss 
  > This file and any other file will be automatically compiled if imported with the extension .scss or .sass.

To share variables between Sass files, use Sass imports. 
For example, src/App.scss and other component style files could include @import "./shared.scss"; with variable definitions.
```
@import 'styles/_colors.scss'; // assuming a styles directory under src/
@import '~nprogress/nprogress'; // importing a css file from the nprogress node module
```
> Note: You must prefix imports from node_modules with ~ as displayed above.

- import bootstrap scss
  > main bootstrap scss should be located on a later position than the other react basic styles to customisation. 
  > If not, bootstrap will overwrite your customised style

[App.scss]
```
@import '~bootstrap/scss/_functions';
@import '~bootstrap/scss/_variables';
@import '~bootstrap/scss/_mixins';
```

- font setup (e.g. google font)
```
@import url('https://fonts.googleapis.com/css2?family=Jua&family=Roboto:wght@100;300;400;500;700;900&display=swap');
$font-family-base: "Roboto", "Open Sans", sans-serif;
```

- responsive display arrangement
- theme color setting
```
@import '~bootstrap/scss/_functions';
@import '~bootstrap/scss/_variables';
@import '~bootstrap/scss/_mixins';

@import url('https://fonts.googleapis.com/css2?family=Jua&family=Roboto:wght@100;300;400;500;700;900&display=swap');
$font-family-base: "Roboto", "Open Sans", sans-serif;

$container-max-widths: (
  sm: 540px,
  md: 720px,
  lg: 960px,
  xl: 1440px,
  xxl: 2000px
);

$theme-colors: (
    "btm-red": #F23D6D,
    "btm-orange": #F2A649,
    // "btm-yellow":​ #ffed5d,
    "btm-green": #7EBF60,
    // "btm-purple":​ #532A8C, ​
    "btm-blue": #4AB0D9,

    "primary": #F23D6D,
    "secondary": #252624,
    "footer": #F2F7FD,
    "section": #F2F7FD,
    "button": #F2F7FD,
    "button-hover": #F2F7FD,
    "link": #FF3156,
    "link-hover": #F2A649,
    "title": #252624,
    "info": #4AB0D9,
    "border": #F23D6D,
    "divider": #F23D6D,
    "warning": #F2A649,
    "inactive": #F2F7FD
);

@import "~bootstrap/scss/bootstrap";

...
# other project speicified styles classes from here
.App {
  text-align: center;
}

```


#### Basic packages (Router)
```bash
npm install react-router-dom --save

basic example of App.js
```
[app.js]
```js
import './App.scss';
import React from "react"
import {
  BrowserRouter as Router,
  Route,
  Switch,
} from "react-router-dom"
import {
  Container
} from 'react-bootstrap'

function App() {
  return (
    <Router>
      <div>App</div>
    </Router>
  );
}

export default App;
:
```
