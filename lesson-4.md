# Lesson 4: Learn

## Express Middleware

### What is middleware in Express

Middleware is a function that sits between the request and response objects and can modify or intercept either one before they reach their destination. Middleware can be used to handle common tasks, such as logging, parsing, and authentication, among others.

### What do we use middleware for?

1. **Authentication**: Middleware can be used to verify the user's identity and ensure they have the proper permissions to access certain routes or resources.
2. **Logging**: Middleware can be used to log incoming requests and outgoing responses, providing insight into application usage and error tracking.
3. **Error handling**: Middleware can be used to handle errors that occur during the application's runtime, such as server errors or client-side validation errors.
4. **Compression**: Middleware can be used to compress the response body, reducing the size of the data sent back to the client and improving application performance.
5. **CORS**: Middleware can be used to handle Cross-Origin Resource Sharing (CORS) requests, allowing clients from different domains to access your application's resources.
6. **Rate limiting**: Middleware can be used to limit the rate at which clients can access certain resources, preventing excessive traffic and potential server overload.
7. **Caching**: Middleware can be used to cache frequently requested data, reducing the number of database queries and improving application performance.
8. **CSRF protection**: Middleware can be used to protect against Cross-Site Request Forgery (CSRF) attacks, where a malicious client attempts to execute unauthorized requests on behalf of the user.
9. **Custom functionality**: Middleware can be used to add custom functionality to your application, such as processing form data or performing database updates before a response is sent back to the client.

### How Express middleware works

Middleware functions in Express take three arguments: req (the request object), res (the response object), and next (a function that passes control to the next middleware function in the stack).

### Middleware can handle responses or pass them on

```javascript
// Middleware function 1
app.use((req, res, next) => {
  console.log('Middleware 1');
  // Pass control to the next middleware function
  next();
});

// Middleware function 2
app.use((req, res, next) => {
  console.log('Middleware 2');
  // Terminate the request-response cycle by sending a response to the client
  res.send('Hello World!');
});

// Middleware function 3
// This middleware function won't be executed, as the previous middleware function
// has already sent a response to the client and terminated the request-response cycle
app.use((req, res, next) => {
  console.log('Middleware 3');
});
```

### The middleware flow

The order in which middleware functions are declared matters, as they are executed in the order they are defined.
Let’s set up some middleware in our backend
Lets install the middleware packages we will be using

In your terminal run the install command for our middleware packages: `npm install helmet morgan cors express-rate-limit`

### The Middleware we are adding
- helmet: Helps secure your Express app by setting various HTTP headers.
- morgan: Logs HTTP request/response information to the console.
- cors: Enables Cross-Origin Resource Sharing (CORS).
- express-rate-limit: Helps protect against brute-force attacks by limiting the number of requests a client can make.

### Importing our new middleware in index.js:
```javascript
const express = require("express");
const helmet = require("helmet");
const morgan = require("morgan");
const cors = require("cors");
const rateLimit = require("express-rate-limit");
const usersRouter = require("./routers/usersRouter");
const lightningRouter = require("./routers/lightningRouter");

// Create a new instance of the Express server
const server = express();
// Use helmet middleware for security
server.use(helmet());
// Use morgan middleware for logging using the 'common' logging format
server.use(morgan("common"));
// Use cors middleware to enable cross-origin requests
server.use(cors());

// Use rate limiting middleware to limit the number of requests from a single IP
server.use(rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per windowMs
}));
```

### Let’s add one more tool before we test!
Nodemon

Nodemon is a utility tool for Node.js that automatically restarts the server whenever a change is detected in the code. This saves us the hassle of having to manually restart the server after each change we make.

Run `npm install nodemon` in your terminal.

### Update scripts in package.json to use nodemon:
```json
{
  "name": "backend-course-walkthrough",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "nodemon index.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

Update the scripts property to use nodemon whenever we run npm run start.

### Run the server and test our new middleware

After running npm run start, the server should start up with nodemon.


### Testing Helmet

Check the response from our last request and verify that helmet is working by checking for all the additional headers that should be added to the response.
A bit more on rate limiting and Denial of Service attacks

Denial of Service (DoS) attacks are a common type of cyber attack. Understanding the basic mechanism of attacks can help in developing countermeasures.

### Now let’s attack our own server to test our rate limiting middleware!
```javascript
const http = require('http');

function sendRequest() {
  const options = {
    host: 'localhost',
    port: 5500,
    path: '/',
    method: 'GET',
  };
  const req = http.request(options, (res) => {
    console.log(`Response status code: ${res.statusCode}`);
  });
  req.on('error', (e) => {
    console.error(`Request error: ${e.message}`);
  });
  req.end();
}

setInterval(sendRequest, 100);
```

### Review

Middleware is any process or function that runs in between the request and response in our API. By using middleware, we can modularize our server logic and add additional functionality to our server. We learned how to use several popular middleware packages to enhance our server's security and performance.
