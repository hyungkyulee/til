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
