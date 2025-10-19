# Auto-gen Documentation

## TSDoc / TypeDoc (Most Popular for TypeScript)
TypeDoc uses JSDoc-style comments and is the de-facto standard for TypeScript projects.

Installation
```
npm install --save-dev typedoc
```

Code Comments (e.g. AWS lamba in nodejs project)
```
/**
 * Lambda Authorizer for API Gateway
 * 
 * @remarks
 * Authorization Flow:
 * 1. Client makes request → API Gateway
 * 2. API Gateway invokes authorizer → Lambda Authorizer checks credentials
 * 3. Authorizer returns policy → Allow or Deny
 * 4. If Deny → API Gateway returns 403 automatically (handler never invoked)
 * 5. If Allow → API Gateway forwards request to your handler
 * 
 * @module handlers/auth/authorizer
 */

import {
  APIGatewayAuthorizerResult,
  APIGatewayRequestAuthorizerEvent
} from "aws-lambda";
:
:


/**
 * Generates an IAM policy document for API Gateway authorization
 * 
 * @param principalId - Unique identifier for the principal (user/service)
 * @param effect - Whether to Allow or Deny the request
 * @param resource - The ARN of the API Gateway method being accessed
 * @param context - Optional additional context to pass to the handler
 * @returns IAM policy document in API Gateway format
 * 
 * @example
 * ```typescript
 * const policy = generatePolicy('user-123', 'Allow', 'arn:aws:execute-api:...');
 * ```
 */
<external methods>

/**
 * Lambda handler for API Gateway REQUEST authorizer
 * 
 * @param event - API Gateway authorizer event containing headers and request context
 * @returns Authorization policy document (Allow/Deny)
 * 
 * @throws Never throws - always returns a policy document
 * 
 * @remarks
 * Environment Variables Required:
 * - `AWSV2_API_KEY`: The valid API key for authentication
 * 
 * @example
 * Request with valid API key:
 * ```typescript
 * {
 *   headers: { 'x-api-key': 'valid-key' },
 *   methodArn: 'arn:aws:execute-api:...'
 * }
 * // Returns: Allow policy
 * ```
 * 
 * @example
 * Request without API key:
 * ```typescript
 * {
 *   headers: {},
 *   methodArn: 'arn:aws:execute-api:...'
 * }
 * // Returns: Deny policy with error context
 * ```
 */
export const handler = async (event: APIGatewayRequestAuthorizerEvent): Promise<APIGatewayAuthorizerResult> => {
  :
  <main function codes>
  :
};
```

Script on package.json
```
{
  "scripts": {
    "docs": "typedoc --out docs src",
    "docs:watch": "typedoc --watch --out docs src"
  }
}
```

Create typedoc.json
```
{
  "entryPoints": ["src"],
  "entryPointStrategy": "expand",
  "out": "docs",
  "exclude": [
    "**/node_modules/**",
    "**/*.spec.ts",
    "**/*.test.ts"
  ],
  "plugin": [],
  "excludePrivate": true,
  "excludeProtected": false,
  "excludeExternals": true,
  "readme": "README.md",
  "name": "Zionetra APIs Documentation",
  "includeVersion": true,
  "theme": "default",
  "hideGenerator": true,
  "searchInComments": true,
  "categorizeByGroup": true,
  "categoryOrder": [
    "Handlers",
    "Common",
    "Infrastructure",
    "*"
  ]
}
```

## JSDoc (Classic, Works Everywhere)
installation
```
npm install --save-dev jsdoc
```

jsdoc.json
```
// jsdoc.json
{
  "source": {
    "include": ["src"],
    "includePattern": ".+\\.ts$",
    "excludePattern": "(node_modules/|docs)"
  },
  "plugins": ["plugins/markdown"],
  "opts": {
    "template": "templates/default",
    "encoding": "utf8",
    "destination": "./docs/",
    "recurse": true,
    "verbose": true
  }
}
```

## Docusaurus (Popular for Full Documentation Sites)
Best for comprehensive documentation with tutorials, API refs, and guides.
```
npm install --save-dev @docusaurus/core @docusaurus/preset-classic
```

## API Extractor + API Documenter (Microsoft's Tool)
For enterprise-grade API documentation:
```
npm install --save-dev @microsoft/api-extractor @microsoft/api-documenter
```
