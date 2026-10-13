# NodeJS Interview Prep

## Nodejs Key Tech Knowledge
### Overview
- Javascript Runtime
  javascript on browser, web server, API sever, CLI tools, IoT, etc

- nodejs runtime environment 
  - v8 engine used on chrome
  - libuv - support async I/O, Event-driven (file I/O, network management, etc)
  - APIs - control system resources (e.g. fs, http, crypto, etc)
  - module system
  - event loop - callback, promise, timer, etc
  - CLI tools - e.g. node, npm, npx, etc

- highlights
  - efficient for concurrency
  - optimal for server development (e.g. http.createServer())
  - backend development written in only javascript
 
- 3 types of async tasks
  - async fn() without await -> immediately callback
  - await Promise.resolve() -> microtask, request to regist callback
  - await setTimeout(...) -> libuv timer -> event queue -> callback
 

## Code snipets
### Credit Card Validation
- Algorithm
```
 * start) for loop from 15 to 0
 * 1) to Number : "4242 4242 4242 4242" -> 4242424242424242
 * 2) if shouldDouble -> *2, if not -> itself
 * 3) if 2) > 9 -> -9
 * 4) sum
 * 5) toggle shouldDouble
 * return) if sum % 10 = 0 -> valid
 */
export function validateCardNumber(_input: string): CardValidationResult {

  let sum = 0
  let shouldDouble = false;

  for (let index=_input.length-1; index>=0;index--) {
    let digit = Number(_input[index])
    if (shouldDouble) {
      digit *= 2 
    }

    if (digit > 9) {
      digit -= 9
    }

    sum += digit
    shouldDouble = !shouldDouble
  }

  return (sum % 10 === 0)
    ? { valid: true, reason: "OK" }
    : { valid: false, reason: "LUHN_FAILED" }
}
```

- Test Code
```
describe("Card Validation Tests", () => {
  it("should validate card", () => {
    const cardNumber = '4242424242424242'

    const result = validateCardNumber(cardNumber)
    expect(result.valid).toBe(true)
  })
})
```

### 


