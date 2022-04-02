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
  ```
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


## 2. Array
array 는 바뀌지 않는다.
collection 을 핸들링하는 기본적이고 중요한 개념의 시작.
### T5. 
collection을 add, remove, sort 등등할때 array 가 최고다.ㅐ

ㅐㅠ
### 
Object.keys 롤 object 들의 key들의 array를 리턴할수 있다. 

.find 는 매칭되는 first item 만 리턴, 
.filter 는 매칭되는 모든 item들의 콜렉션

indexOf 대신 include를 써라. 
이유는, indexOf 에서 0의 index를 리턴할때 이를 false 로 실수 할 가능성이 높아서 >-1 인 validation 을 반드시 해줘야 한다. 

### Spread Operator
list =  element들만을 가지고 있는 collections
spread operator 는 array 들의 list 를 리턴
memcopy가 일어나므로 mutation 을 막을 수있다

e.g. 
 - loop를 통해서 원하는 조건의 값들을 찾아서 새로운 collections을 담아서 리턴 
 - splice를 통해서 사용하지만 mutation 할 경우에, 문제의 요소가 있어서 협업에 문제가 있다. > slice + concat + slice 의 조합 
