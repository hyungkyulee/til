# Simplifying Javascript

## 1. Signal Intention with Variable Assignment
### T1. Signal Unchanging Values with const
- var vs more options (var, let, const)
- most cases with 'const', cannot reassign ( = A value assigned to const is not immutable)
- 'const' benefits
  - readible of intention
  - error protection of unitended reassignment
  - 
- Note:
  For objects, arrays, or other collections, you’ll need to be more disciplined.
  e.g. const arrays can be updated with collection methods such as 'push'.
  > minimise mutation as much as you can
  > e.g. link query expressions
  ```
  const discountable = [];
  ...
    for (let i = 0; i < cart.length; i++) { if (cart[i].discountAvailable) {
      discountable.push(cart[i]);
    }
  }
  ```
  
  [best way]
  ```
  const discountable = cart.filter(item => item.discountAvailable);
  ```
  
  
### T2. Reduce Scope Conflicts with let and const
- comparison
|| var | let ||
| lexical | block |
| global | local {} |
| continue | end at block |

> "functions are executed using the scope chain that was in effect when they were defined" - according to JavaScript Definition Guide.
> e.g. 
> ```
  var scope = "I am global";
  function whatismyscope(){
     var scope = "I am just a local";
     function func() {return scope;}
     return func;
  }
  whatismyscope()()
  ```

### T3. Isolate Information with Block Scoped Variables
- lexical variable inside {} = accessable from others
- hoisting : accessable before declaration due to compile process (?)
- REPL ( read evaluate print loop ) : cli which immediately evaluates it and return result

```
function addClick(items: any) { 
    for (var i = 0; i < items.length; i++) { 
        items[i].onClick = function () { 
            return i; 
        }; 
    } 
    return items; 
} 

const example = [{}, {}]
const clickSet = addClick(example) 
console.log(clickSet[0].onClick())
```

```
function addClick(items) {
for (var i = 0; i < items.length; i++) {
items[i].onClick = (function (i) { return function () {
return i; };
 }(i));
}
 return items;
}
const example = [{}, {}];
const clickSet = addClick(example);  clickSet[0].onClick();
```

```
function addClick(items) {
for (let i = 0; i < items.length; i++) {
items[i].onClick = function () { return i; }; }
return items; }
const example = [{}, {}];
const clickSet = addClick(example); clickSet[0].onClick();
```

### T4. Convert Variables to Readable Strings with Template Literals


