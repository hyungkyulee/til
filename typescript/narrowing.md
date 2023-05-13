## What does narrowing mean?
> narrowing = type guards, assignment and a process of refining types to more specific types than declared

## type guards
### typeof
- "string"
- "number"
- "bigint"
- "boolean"
- "symbol"
- "undefined"
- "object"
- "function"

### how to check
```
if (typeof padding === "number") {
...
}
```

### remarks
```
if (typeof strs === "object") {
  for (const s of strs) {   //'strs' is possibly 'null'.
    ...
  }
}
```
> typeof null is actually "object", so it needs to do more narrowing

## truthiness narrowing
### falsy in javascript
- 0
- NaN
- "" (the empty string)
- 0n (the bigint version of zero)
- null
- undefined

### how to check
- Boolean()
- !! (narrow literal): shorter double-boolean negation

### remarks
different type in typescript
```
Boolean("hello"); // type: boolean, value: true
!!"world"; // type: true,    value: true
```

this has a subtle downside: we may no longer be handling the empty string case correctly.
```
function printAll(strs: string | string[] | null) {
  if (strs) {
    if (typeof strs === "object") {
      for (const s of strs) {
        console.log(s);
      }
    } else if (typeof strs === "string") {
      console.log(strs);
    }
  }
}
```

Boolean negations with ! filter out from negated branches.
```
function multiplyAll(
  values: number[] | undefined,
  factor: number
): number[] | undefined {
  if (!values) {
    return values;
  } else {
    return values.map((x) => x * factor);
  }
}
```

## equality narrowing
switch statements and equality checks like ===, !==, ==, and != to narrow types

### value-comparison
JavaScript provides three different value-comparison operations:

- === — strict equality : 1) type conversion, and 2) handle 'NaN, -0 and +0' 
- == — loose equality : 1) without a type conversion, and 2) handle 'NaN, -0 and +0'
- Object.is() : 1) without a type conversion, and 2) won't handle 'NaN, -0 and +0'

| x | y | == | === | Object.is | SameValueZero |
|---|---|---|---|---|---|
| undefined | undefined | ✅ true | ✅ true | ✅ true | ✅ true |
| null | null | ✅ true | ✅ true | ✅ true | ✅ true |
| true | true | ✅ true | ✅ true | ✅ true | ✅ true |
| false | false | ✅ true | ✅ true | ✅ true | ✅ true |
| 'foo' | 'foo' | ✅ true | ✅ true | ✅ true | ✅ true |
| 0 | 0 | ✅ true | ✅ true | ✅ true | ✅ true |
| +0 | -0 | ✅ true | ✅ true | ❌ false | ✅ true |
| +0 | 0 | ✅ true | ✅ true | ✅ true | ✅ true |
| -0 | 0 | ✅ true | ✅ true | ❌ false | ✅ true |
| 0n | -0n | ✅ true | ✅ true | ✅ true | ✅ true |
| 0 | false | ✅ true | ❌ false | ❌ false | ❌ false |
| "" | false | ✅ true | ❌ false | ❌ false | ❌ false |
| "" | 0 | ✅ true | ❌ false | ❌ false | ❌ false |
| '0' | 0 | ✅ true | ❌ false | ❌ false | ❌ false |
| '17' | 17 | ✅ true | ❌ false | ❌ false | ❌ false |
| [1, 2] | '1,2' | ✅ true | ❌ false | ❌ false | ❌ false |
| new String('foo') | 'foo' | ✅ true | ❌ false | ❌ false | ❌ false |
| null | undefined | ✅ true | ❌ false | ❌ false | ❌ false |
| null | false | ❌ false | ❌ false | ❌ false | ❌ false |
| undefined | false | ❌ false | ❌ false | ❌ false | ❌ false |
| { foo: 'bar' } | { foo: 'bar' } | ❌ false | ❌ false | ❌ false | ❌ false |
| new String('foo') | new String('foo') | ❌ false | ❌ false | ❌ false | ❌ false |
| 0 | null | ❌ false | ❌ false | ❌ false | ❌ false |
| 0 | NaN | ❌ false | ❌ false | ❌ false | ❌ false |
| 'foo' | NaN | ❌ false | ❌ false | ❌ false | ❌ false |
| NaN | NaN | ❌ false | ❌ false | ✅ true | ✅ true |

### remarks
JavaScript’s looser equality checks with == and != also get narrowed correctly.
Checking whether 'something == null' actually not only checks whether it is specifically the value null, but it also checks whether it’s potentially undefined. 
The same applies to == undefined: it checks whether a value is either null or undefined.
```
interface Container {
  value: number | null | undefined;
}
 
function multiplyValue(container: Container, factor: number) {
  // Remove both 'null' and 'undefined' from the type.
  if (container.value != null) {
    console.log(container.value);
                           
(property) Container.value: number
 
    // Now we can safely multiply 'container.value'.
    container.value *= factor;
  }
}
```

## 'in' operation narrowing
an operator for determining if an object has a property with a name

### remarks
example
```
type Fish = { swim: () => void };
type Bird = { fly: () => void };
type Human = { swim?: () => void; fly?: () => void };
 
function move(animal: Fish | Bird | Human) {
  if ("swim" in animal) {
    animal;
      
(parameter) animal: Fish | Human
  } else {
    animal;
      
(parameter) animal: Bird | Human
  }
}
```

## instanceof narrowing
an operator for checking whether or not a value is an “instance” of another valu

### remarks
instanceof vs typeof
> both are similar, returning type information, but 
> 1) as instanceof is comparing with actual types rather than strings, it might be less prone to human error
> 2) as it's comparing pointers (address of memory), it might show better perfomance
> use 
> - instanceof at custom types or complex built-in types
> - typeof at simple buil-in types
> * typeof null is object

## Assignments
TypeScript looks at the right side of the assignment and narrows the left side appropriately.

### remarks
still able to assign a string to x. This is because assignability is always checked against the declared type.
```
let x = Math.random() < 0.5 ? 10 : "hello world!";
   
let x: string | number
x = 1;
 
console.log(x);
           
let x: number
x = "goodbye!";
 
console.log(x);
           
let x: string
```

## Control flow analysis
This analysis of code based on reachability is called control flow analysis, and 
TypeScript uses this flow analysis to narrow types as it encounters type guards and assignments

```
function padLeft(padding: number | string, input: string) {
  if (typeof padding === "number") {
    return " ".repeat(padding) + input;
  }
  return padding + input; // unreachable in case where padding is number
}
```

## Using type predicates
```
const zoo: (Fish | Bird)[] = [getSmallPet(), getSmallPet(), getSmallPet()];
const underWater1: Fish[] = zoo.filter(isFish);
// or, equivalently
const underWater2: Fish[] = zoo.filter(isFish) as Fish[];
```

## Discriminated unions
a union of string literal types
> By using "circle" | "square" instead of string, we can avoid misspelling issues.

### remarks
the union encoding of Shape will cause an error regardless of how strictNullChecks is configured.
```
interface Circle {
  kind: "circle";
  radius: number;
}
 
interface Square {
  kind: "square";
  sideLength: number;
}
 
type Shape = Circle | Square;

function getArea(shape: Shape) {
  return Math.PI * shape.radius ** 2;
}
```
> Property 'radius' does not exist on type 'Shape'.
> Property 'radius' does not exist on type 'Square'.

## never type
TypeScript will use a never type to represent a state which shouldn’t exist.

### remarks 
to reduce the options of a union to a point where you have removed all possibilities and have nothing left

The never type is assignable to every type; however, no type is assignable to never (except never itself)
```
interface Triangle {
  kind: "triangle";
  sideLength: number;
}
 
type Shape = Circle | Square | Triangle;
 
function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
    default:
      const _exhaustiveCheck: never = shape;
Type 'Triangle' is not assignable to type 'never'.
      return _exhaustiveCheck;
  }
}
```
