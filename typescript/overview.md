# Overview of Typescript

## Limitation of Javascript
- lacks some of the features of more mature languages
- relying on IDEs to manage lage code bases
- compromising the key value proposition

## Typescript?
- open-source
- by Microsoft
- a superset of javascript
- cross-platform/broswer/host

- type hint 
  - identify the data type of a variable or parameter
  - describe the shape of an object
  - provide better docs and validate works
  - supported by development tools (e.g. intelliSense, symbol-based navigation, go-to-definition, find references, completion, refectoring, etc)
 
- type can be optional and determined implicitly (e.g. var number = 10)
- typescript web playground : https://www.typescriptlang.org/play?#code/PTAEHUFMBsGMHsC2lQBd5oBYoCoE8AHSAZVgCcBLA1UABWgEM8BzM+AVwDsATAGiwoBnUENANQAd0gAjQRVSQAUCEmYKsTKGYUAbpGF4OY0BoadYKdJMoL+gzAzIoz3UNEiPOofEVKVqAHSKymAAmkYI7NCuqGqcANag8ABmIjQUXrFOKBJMggBcISGgoAC0oACCoASMFmgY7p7ehCTkVOle4jUMdRLYTqCc8LEZzCZmoNJODPHFZZXVtZYYkAAeRJTInDQS8po+rf40gnjbDKv8LqD2jpbYoACqAEoAMsK7sUmxkGSCc+VVQQuaTwVb1UBrDYULY7PagbgUZLJH6QbYmJAECjuMigZEMVDsJzCFLNXxtajBBCcQQ0MwAUVWDEQNUgADVHBQGNJ3KAALygABEAAkYNAMOB4GRogLFFTBPB3AExcwABT0xnM9zsyhc9wASmCKhwDQ8ZC8iElzhB7Bo3zcZmY7AYzEg-Fg0HUiS58D0Ii8AoZTJZggFSRxAvADlQAHJhAA5SASAVBFQAeW+ZF2gldWkgx1QjgUrmkeFATgtOlGWH0KAQiBhwiudokkuiIgMHBx3RYbC43CCJSAA

[e.g.]
```
// basic js style
function addNumbers(x, y) {
  return x + y;
}

console.log(addNumbers("three", 6));
```

// typed
function addNumbers(x: number, y: number) {
  return x + y;
}

console.log(addNumbers("three", 6));
```

addNumbers(x: number, y: number)
addNumberS(s: any, y: any)

