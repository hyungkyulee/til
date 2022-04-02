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
|-|-|
| lexical | block |
  
### T3. Isolate Information with Block Scoped Variables
### T4. Convert Variables to Readable Strings with Template Literals


