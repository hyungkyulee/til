# Deep diving into closures

### Definition
closure is to preserve a data (e.g. variables, funtions, etc) from outer scope to inner scope.

### Background 
closure is not special on javascript because js is referencing a lexical scope up to the end of app scope from the caller point.

### Usages 
- basic example
```
var add = (outter) => {
  var inner = 3;
  return (outter + inner);
}

console.log(add(2));
```

- on functions
```
var addOperator = (outter) => {
  var add = (inner) => {
    return (outter + inner);
  }
  return add;
}

var add10Operator = addOperator(10);
var add100Operator = addOperator(100);

console.dir(add10Operator);

console.log(add10Operator(5));
console.log(add100Operator(5));
```

> like below, chrome inspector says it's closure concept
![image](https://user-images.githubusercontent.com/59367560/136694440-55d910a3-3ece-4fac-a1fb-da484f040921.png)


