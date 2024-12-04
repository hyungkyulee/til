## 1. Core Node.js Concepts
### What is Node.js, and why is it used?
### What is the event loop in Node.js, and how does it work?
	The event loop is a core concept in Node.js that 
 enables non-blocking, asynchronous programming by managing the execution of multiple operations without creating new threads for each task. 
 It allows Node.js to handle many concurrent operations efficiently, even though it's single-threaded

### What is callback?
function in function as a parameter which is executed at  a later point
Node.js heavily relies on callbacks for non-blocking operations, like reading files or making HTTP requests

### Explain the difference between process.nextTick() and setImmediate().
	process.nextTick()
Schedules a callback to be executed immediately after the current operation completes and before the event loop continues to the next phase. before any I/O or timer callbacks
setImmediate()
Schedules a callback to be executed in the Check Phase of the event loop, after I/O events are processed.


### What is the purpose of the Buffer class in Node.js?
The Buffer class in Node.js is a core module designed to handle binary data efficiently. Unlike regular JavaScript strings, which are UTF-16 encoded, a Buffer is a raw sequence of bytes

### How does Node.js handle asynchronous operations?

### What is the difference between blocking and non-blocking code?
the execution of the current thread is paused (or blocked) until the operation completes. No other operations in that thread can proceed during this time. -> synchronous

the operation is executed asynchronously without pausing, and a callback, promise, or other mechanism is used to handle the result once it's ready.


## 2. Modules and Dependencies
### What is the difference between CommonJS and ES Modules in Node.js?
This is related with how they import/export modules.
CommonJS (initial version) : requires / module.exports
ES Module (standardised version) : import / export

### How does the require() function work in Node.js?
### What are some ways to share code between files in Node.js? 
### Explain the purpose of package.json and its key fields.
### What is the difference between dependencies and devDependencies?
### How can you resolve version conflicts in npm dependencies?
Inspect with npm ls: Understand the conflict.
Update dependencies: Manually or with npm update.
Force versions: Use npm-force-resolutions or Yarn’s resolutions field.
Dedupe dependencies: Use npm dedupe to flatten the dependency tree.
Peer dependencies: Install correct versions manually.
Clean installs: Run npm ci for a fresh installation.


## 3. HTTP and Express.js
### How do you create a simple HTTP server in Node.js?
### What is middleware in Express.js, and how does it work?
functions that have access to the request (req), response (res), and the next function in the application’s request-response cycle. These functions can modify the request or response, perform some logic (such as validation, authentication, or logging)

### How can you handle errors in Express applications?
// General error-handling middleware app.use((err, req, res, next) => { console.error(err.stack); // Log the error stack to the console res.status(err.status || 500).json({ message: err.message, // Send the error message to the client // Optionally, you can include other error details details: process.env.NODE_ENV === 'development' ? err.stack : {} // In development mode, include stack trace }); });

### What are the differences between app.get(), app.post(), and app.use()?
app.use() will register middleware that applies to all HTTP methods (GET, POST, etc.) and can be used for specific paths or globally.

### How do you implement route parameters and query parameters in Express?
### Explain the role of CORS and how to enable it in a Node.js application.
CORS (Cross-Origin Resource Sharing) is a security feature implemented by browsers that allows or restricts web applications running at one origin (domain) to make requests to a resource on another origin.
Without CORS, browsers block cross-origin requests for security reasons to prevent malicious websites from accessing sensitive data on another origin.


## 4. Asynchronous Programming
### What are Promises, and how do they work?
a JavaScript object, It provides a cleaner and more structured way to handle asynchronous tasks than traditional callback functions. 
Three states : pending, fulfilled, rejected
- Creating a promise with resolve/reject functions
- Consuming a promise with then/catch/finally 


### How is async/await used in Node.js?
a modern syntax in JavaScript that simplifies working with promises and asynchronous code

### What are callbacks, and how do they compare to Promises?
### What is the purpose of the util.promisify() function in Node.js?
### What are some ways to handle errors in asynchronous code?

## 5. Security and Performance
### How do you secure a Node.js application against common vulnerabilities (e.g., SQL Injection, XSS, CSRF)?
### What are some best practices for handling sensitive data in Node.js?
Use env var, encryption, Https, RBAC, secure in auth, cors

### How can you prevent memory leaks in Node.js?
Const, let(in blocked scope), close resource properly like fs.stream, http, db, 
global.gc() (only in debugging mode), node —expose-gc app.js

### What tools and techniques can you use to optimize the performance of a Node.js application?

## 6. Database Integration
### How do you connect a Node.js application to a database (e.g., MongoDB, MySQL)?
You need a driver or an ORM (Object-Relational Mapping) library for the database you're using.

For MySQL: mysql2 or sequelize
For PostgreSQL: pg or sequelize
For MongoDB: mongoose or mongodb
For SQLite: sqlite3

### Explain the difference between SQL and NoSQL databases.
relational vs non
fixed schema vs flexible (key/value pairs)
scale vertically(e.g. more RAM) vs horizontally



### How can you handle database transactions in Node.js?
executing multiple queries in a way that ensures all operations succeed or none of them are applied.
This is critical for maintaining data integrity,
especially in systems where related updates must occur together

SQL - begin trans -> queries -> commit/rollback
No SQL (mongodb) - ORM (session)


### What is an ORM, and why might you use one in a Node.js project?

## 7. Testing and Debugging
### What tools are commonly used for testing Node.js applications?

** for serverless handler
[package.js]
run-handler: "npm run build execHandler [handlerFilePath] [eventFilePath]
[even.js]
export.modules({
  params or body
}

[execHandler.js]
const { handler } = require(handlerFilePath);
const event = require(eventFilePath);

handler(event, {}).then(console.log).catch(console.error);

### How do you write unit tests for a Node.js application?
### What is the purpose of Mocha, Chai, or Jest in testing?
### How do you debug a Node.js application?
console.log() or structure logging (e.g. winston)
try/catch for async
node inspect [filename]
sourceMap: true at tscondig.js for better debugging

### What are some common debugging tools for Node.js?

## 8. Deployment
### How do you deploy a Node.js application?
### What is the purpose of PM2, and how is it used?
### What is the difference between clustering and load balancing in Node.js?
### How do you set environment variables in Node.js?
### What are some hosting platforms commonly used for Node.js applications (e.g., AWS, Heroku)?

## 9. Real-World Scenarios
### How would you design a RESTful API in Node.js?
### How do you handle file uploads in Node.js?
### What are WebSockets, and how are they used in Node.js?
### How would you implement authentication (e.g., JWT, OAuth) in a Node.js application?
### How do you implement rate-limiting in a Node.js API?

## 10. Advanced Topics
### What is a stream in Node.js, and how do you use it?
### How does clustering work in Node.js, and when would you use it?
### Wh at is the worker_threads module, and why is it used?
### Explain how Node.js handles child processes.
### What are some common design patterns used in Node.js development?

Behavioral and Problem-Solving Questions
### Can you describe a challenging bug you solved in a Node.js application?
### How do you ensure the scalability of a Node.js application?
### How do you manage large codebases or projects in Node.js?
