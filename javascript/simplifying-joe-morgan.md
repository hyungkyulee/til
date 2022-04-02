# Simplifying Javascript

## 1. Signal Intention with Variable Assignment
### Tip 1. Signal Unchanging Values with const
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
  
  
### Tip 2. Reduce Scope Conflicts with let and const
- comparison
|| var | let ||
| lexical | block |
| global | local {} |
| continue | end at block |

> "functions are executed using the scope chain that was in effect when they were defined" - according to JavaScript Definition Guide.
> e.g. 
  ```
  var scope = "I am global";
  function whatismyscope(){
     var scope = "I am just a local";
     function func() {return scope;}
     return func;
  }
  whatismyscope()()
  ```

### Tip 3. Isolate Information with Block Scoped Variables
- lexical variable inside {} = accessable from others
- hoisting : accessable before declaration due to compile process. Javascript has been designed with the hoisting issue of 'var' variables and has a potential bug. Any 'var' variable declared in any loction will be lifted up to the top of the scope so that the variable can be used in anywhere of the scope
  * REPL ( read evaluate print loop ) : cli which immediately evaluates it and return result

- e.g. variable scope
[an unexpected problem with 'var']
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
console.log(`result: ${clickSet[0].onClick()}, ${clickSet[1].onClick()})
```
> result: 2, 2
> 'i' always refers to the last value they’re assigned within a function. And it's return the last 'var i' value = 2 at a calling time of 'onClick'

[a combination of closures, higher-order functions, and self-invoking functions]
```
function addClick(items: any[]) {
    for (var i = 0; i < items.length; i++) {
        items[i].onClick = (function (i) { 
            return function () {
                return i; 
            };
        }(i));
    }
    return items;
}
```
> it has a closure of the function context and the value ('i') will be set the locked value at the time it's assigned.
> it's a solution, but it's too complicated.

[simplified with 'let']
```
function addClick(items: any[]) {
    for (let i = 0; i < items.length; i++) {
        items[i].onClick = function () { 
            return i; 
        }; 
    }
    return items; 
}
```
> 'let' locks the value during each iteration of the for loop

### Tip 4. Convert Variables to Readable Strings with Template Literals
Traditionally, you have to combine each piece with a + sign.
Template literals: ``` = `string ${variable}` ``` are such an improvement on string concatenation


## 2. Manage Data Collections with Arrays
array 는 바뀌지 않는다.
collection 을 핸들링하는 기본적이고 중요한 개념의 시작.
### Tip 5. Create Flexible Collections with Arrays
- collection's operation of add, remove, sort in array
- mutable array operation can be dangerous in 'sort'

### Tip 6. Check Existence in an Array with Includes()
- Object.keys 롤 object 들의 key들의 array를 리턴할수 있다. 
- .find 는 매칭되는 first item 만 리턴, 
  .filter 는 매칭되는 모든 item들의 콜렉션

- indexOf 대신 include를 써라. 
  이유는, indexOf 에서 0의 index를 리턴할때 이를 false 로 실수 할 가능성이 높아서 >-1 인 validation 을 반드시 해줘야 한다. 

### Tip 7. Mold Arrays with the Spread Operator
- spread operator 는 array 들의 list 를 리턴
> list =  element들만을 가지고 있는 collections
> memcopy가 일어나므로 mutation 을 막을 수있다

- e.g. 
  - loop를 통해서 원하는 조건의 값들을 찾아서 새로운 collections을 담아서 리턴 
  - splice를 통해서 사용하지만 mutation 할 경우에, 문제의 요소가 있어서 협업에 문제가 있다. > slice + concat + slice 의 조합 

### Tip 8. Avoid Push Mutations with the Spread Operator
```
// Copy
const toCopy = ['Moby Dick', 'White Teeth']; 
const copied = toCopy.slice();

const moreCopies = ['Moby Dick', 'White Teeth']; 
const addMore = [...moreCopies, 'More Ear'];

```

### Tip 9. Avoid Sort Confusion with the Spread Operator
[traditional way]
```
function sortByYears(a, b) { 
    if (a.years === b.years) {
        return 0;
    }
    return a.years - b.years;
}
```

[by spread operator]
```
[...staff].sort(sortByYears);
```
