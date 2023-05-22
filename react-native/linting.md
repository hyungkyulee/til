
### Overall
- Dev Dependencies
"@typescript-eslint/eslint-plugin": "^5.43.0",
"eslint": "^8.0.1",
"eslint-config-prettier": "^8.8.0",
"eslint-config-standard-with-typescript": "latest",
"eslint-plugin-import": "^2.25.2",
"eslint-plugin-n": "^15.0.0",
"eslint-plugin-promise": "^6.0.0",
"eslint-plugin-react": "latest",
"eslint-plugin-react-native": "^4.0.0",
"prettier": "^2.8.8",

### Setup eslint
- install packages and run tht init configuration
```
yarn add -D eslint
npm init @eslint/config
// select the options step-by-step : 
// 1) To check syntax, find problems, and enforce code style
// 2) js modules
// 3) React
// 4) Node
// 5) Use a popular style guide
// 6) Standard
// 7) json for config file
// 8) install the peer-dependencies now by yarn
```

- .eslintrc.json (example)
```
{
  "env": {
    "node": true,
    "es2021": true
  },
  "extends": [
    "plugin:react/recommended",
    "standard-with-typescript",
    "prettier"
  ],
  "overrides": [
    {
      "files": ["*.jsx", "*.tsx", "*.ts"],
      "rules": {
        "@typescript-eslint/explicit-function-return-type": ["off"],
        "@typescript-eslint/consistent-type-definitions": ["off"],
        "@typescript-eslint/consistent-type-imports": ["off"],
        "@typescript-eslint/strict-boolean-expressions": ["off"],
        "@typescript-eslint/no-confusing-void-expression": ["off"]
      }
    }
  ],
  "parserOptions": {
    "tsconfigRootDir": ".",
    "project": ["./tsconfig.json"]
  },
  "plugins": ["react", "react-native"],
  "rules": {
    "semi": ["error"],
    "quotes": ["error", "single", { "avoidEscape": true }],
    "comma-dangle": ["error", "always-multiline"],
    "no-console": "off"
  },
  "ignorePatterns": ["**/*.test.tsx"]
}
```
> overrides rule is optional to ignore some rules declared here

- ignore the lint rules on node_modules
create .eslintignore file
```
node_modules/
```

- add lint script on package.json
```
"scripts": {
    "start": "expo start --dev-client",
    ...
    "lint": "eslint ."
  },
```

### Setup prettier
- install packages
```
yarn add -D prettier eslint-config-prettier
```

- .prettierrc.json (example)
```
{
  "printWidth": 80,
  "tabWidth": 2,
  "semi": false,
  "singleQuote": true,
  "jsxSingleQuote": true,
  "bracketSpacing": true,
  "arrowParens": "avoid"
}

```

### vscode extention config with prettier
- settings.json (example)
  > /users > [user] > library > application support > code > user > settings.json
set some rule for vs-code action
```
{
	"security.workspace.trust.untrustedFiles": "open",
	"javascript.format.semicolons": "remove",
	"[json]": {
		"editor.defaultFormatter": "esbenp.prettier-vscode"
	},
	"editor.tabSize": 2,
	"javascript.preferences.importModuleSpecifier": "relative",
	"typescript.preferences.importModuleSpecifier": "relative",
	"javascript.updateImportsOnFileMove.enabled": "always",
	"redhat.telemetry.enabled": true,
	"diffEditor.ignoreTrimWhitespace": false,
	"[python]": {
		"editor.formatOnType": true
	},
	"editor.codeActionsOnSave": {
		"source.fixAll.eslint": true
	},
	"editor.defaultFormatter": "esbenp.prettier-vscode",
	"editor.formatOnSave": true,
	"prettier.semi": false,
	"prettier.jsxSingleQuote": true,
	"prettier.singleQuote": true,
	"prettier.bracketSameLine": true,
	"prettier.useTabs": true,
	"prettier.trailingComma": "all",
	"eslint.validate": [
		"javascript",
		"javascriptreact",
		"typescript",
		"typescriptreact"
	]
}
```

