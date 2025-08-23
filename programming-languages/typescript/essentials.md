# Overview
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

// typed
function addNumbers(x: number, y: number) {
  return x + y;
}

console.log(addNumbers("three", 6));


addNumbers(x: number, y: number)
addNumberS(s: any, y: any)
```

## How is typescript transpiling and working (by sample project)
- install typescript compiler
```
npm install -g typescript
tsc --version
```

- create any ts file with typescript code
[smaple.ts]
```
var message: string = 'This is a sample of typescript'
console.log(message)
```

- build/run it by tsc and node
```
tsc sample.ts
node sample.js
```

- set typscript config file to debug and to do others
[tsconfig.json]
```
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "outDir": "out",
    "sourceMap": true
  }
}
```
> module: Specifies the module code generation method. Node uses commonjs.
> target: Specifies the output language level.
> moduleResolution: This helps the compiler figure out what an import refers to. The value node mimics the Node module resolution mechanism.
> outDir: This is the location to output .js files after transpilation. In this tutorial you will save it as dist.
> by the F5 button, you can select Node.js to runtime compiler (F9 to toggle breakpoints)
>  * An alternative to create and populate a 'tsconfig.json' file manually by ``` tsc --init ```
> ![image](https://user-images.githubusercontent.com/59367560/135723205-ced26e2a-d994-42f7-874a-b7d9bee88f1f.png)

# Setup

# Syntaxes
## Variables
## Interface
- naming convension : The TypeScript coding guidelines suggest interfaces should not start with the letter I
- type of properties(or members)
  - Required - e.g. firstName: string;
  - Optional - e.g. firstName?: string;
  - Read only - e.g. readonly firstName: string;
- indexable interface vs interface array
  - interface Student { [index: number]: string; } -> let students: Student; students = ['Tom', 'Ammy', 'Sara']; let name: string = students[1]; concole.log(name);
  - interface Student { name: string; } -> let students: Student[{'Tom'}, {'Ammy'}, {'Sara'}]; console.log(students[1].name)
- usage : basic example
  ```
  interface Student {
    name: string;
    age: number;
    male?: boolean;
  }
  ...
  function checkYearGroup(student: Student) {
    let yearGroup: string = 'y1';
    switch (student.age) {
      case 5: 
        yearGroup = 'y1';
        break;
      case 6:
        yearGroup = 'y2';
        break;
        ...
      default:
        yearGroup = 'y1';
        break;
    }
  }
  
  let londonStudent: Student = {
    name: 'Graham',
    age: 10
  }
  console.log(checkYearGroup(londonStudent));
  ```

- usage: fetch API response DTO
  ```
  const fetchURL = 'https://jsonplaceholder.typicode.com/posts'
  // Interface describing the shape of our json data
  interface Post {
     userId: number;
     id: number;
     title: string;
     body: string;
  }
  async function fetchPosts(url: string) {
      let response = await fetch(url);
      let body = await response.json();
      return body as Post[];
  }
  async function showPost() {
      let posts = await fetchPosts(fetchURL);
      // Display the contents of the first item in the response
      let post = posts[0];
      console.log('Post #' + post.id)
      // If the userId is 1, then display a note that it's an administrator
      console.log('Author: ' + (post.userId === 1 ? "Administrator" : post.userId.toString()))
      console.log('Title: ' + post.title)
      console.log('Body: ' + post.body)
  }

  showPost();
  ```

- inheritence by 'extends' keyword
  ```
  interface Student {
    name: string;
    age: number;
    address: string;
  }
  ...
  interface LondonStudent extends Student {
    postcode: string
  }
  interface UsStudent extends Student {
    zipcode: string
  }
  ...
  // implementation
  function GetLondonStudentFullAddress(student: LondonStudent): string {
    let address = student.address;
    return 'Address in London: ' + address + ', ' + student.postcode;
  }
  ```
  
## Destructuring in Parameter
Destructuring in Function Parameter was introduced at ECMAScript2015, thought. Typescript cannot transfile the destrucrued paramter.
This is because a type annonation is not a type, but a name of variable under the specification of a superset of javascript. 
so, the below will not work
```
function Called(param: any, {name: string}): number {
  return 100
}
```

Solutions are ...
```
// typing destructured parameter
function Called(param: any, {name}: {name: string}): number {
...

// (optional) for a default value
function Called(param: any, {name = ''}: {name?: string}): number {
...

// by interface
interface NameProp {
  name?: string;
}

function Called(param: any, {name = ''}: NameProp = {}): number {
...
```
