# Lesson 4: Learn

# Express Middleware

## - What is middleware

## - Using middleware packages

## - Securing our server with middleware


### What is middleware in Express

```
● Middleware is a function that sits between the request and response objects
and can modify or intercept either one before they reach their destination.
● Middleware can be used to handle common tasks, such as logging, parsing,
and authentication, among others.
```

### What do we use middleware for?

1. **Authentication** : Middleware can be used to verify the user's identity and ensure they have the proper
    permissions to access certain routes or resources.
2. **Logging** : Middleware can be used to log incoming requests and outgoing responses, providing insight
into application usage and error tracking.
3. **Error handling** : Middleware can be used to handle errors that occur during the application's runtime,
such as server errors or client-side validation errors.
4. **Compression** : Middleware can be used to compress the response body, reducing the size of the data
sent back to the client and improving application performance.
5. **CORS** : Middleware can be used to handle Cross-Origin Resource Sharing (CORS) requests, allowing
clients from different domains to access your application's resources.
6. **Rate limiting** : Middleware can be used to limit the rate at which clients can access certain resources,
preventing excessive traffic and potential server overload.
7. **Caching** : Middleware can be used to cache frequently requested data, reducing the number of
database queries and improving application performance.
8. **CSRF protection** : Middleware can be used to protect against Cross-Site Request Forgery (CSRF)
attacks, where a malicious client attempts to execute unauthorized requests on behalf of the user.
9. **Custom functionality** : Middleware can be used to add custom functionality to your application, such
as processing form data or performing database updates before a response is sent back to the client.


### How Express middleware works

```
● Middleware functions in Express take three arguments: req (the request object), res (the
response object), and next (a function that passes control to the next middleware
function in the stack).
```

### Middleware can handle responses or pass

### them on

_// Middleware function 1_
app.use(( _req_ , _res_ , _next_ ) => {
console.log('Middleware 1');
_// Pass control to the next middleware function_
next();
});
_// Middleware function 2_
app.use(( _req_ , _res_ , _next_ ) => {
console.log('Middleware 2');
_// Terminate the request-response cycle by sending a response to the client
res_ .send('Hello World!');
});
_// Middleware function 3_
app.use(( _req_ , _res_ , _next_ ) => {
console.log('Middleware 3');
_// This middleware function won't be executed, as the previous middleware function
// has already sent a response to the client and terminated the request-response cycle_
});


### The middleware flow

```
● The order in which middleware functions are declared matters, as they are
executed in the order they are defined.
```

## Let’s set up some

## middleware in our

## backend

###### Middleware > Middlemen


### Lets install the middleware packages we will

### be using

- In your terminal run the install command for our middleware packages: npm install
    helmet morgan cors express-rate-limit
       **The Middleware we are adding**
● **helmet** : Helps secure your Express app by setting various HTTP headers. For example, it can
help protect against cross-site scripting (XSS) attacks by setting the X-XSS-Protection header, or
help prevent clickjacking attacks by setting the X-Frame-Options header.
● **morgan** : Logs HTTP request/response information to the console. This can be useful for
debugging and monitoring your application.
● **cors** : Enables Cross-Origin Resource Sharing (CORS), which allows web pages from different
domains to make requests to your Express app. CORS can help improve security and allow for
more flexible access controls.
● **express-rate-limit** : Helps protect against brute-force attacks and other forms of abuse by
limiting the number of requests a client can make to your Express app in a given time period.
You can configure the rate limit based on factors such as IP address, request method, and more.


_// Our updated imports_
const express = require("express");
const helmet = require("helmet");
const morgan = require("morgan");
const cors = require("cors");
const rateLimit = require("express-rate-limit");
const usersRouter = require("./routers/usersRouter");

##### const lightningRouter = require("./routers/lightningRouter");

### Importing our new middleware in index.js


### Initializing helmet morgan and CORS

Be sure to initialize each of these before server.use(express.json());
_// Create a new instance of the Express server_
const server = express();
_// Use helmet middleware for security_
server.use(helmet());
_// Use morgan middleware for logging using the 'common' logging format_
server.use(morgan("common"));
_// Use cors middleware to enable cross-origin requests_
server.use(cors());


### Add our rate limiting middleware

_// Use rate limiting middleware to limit the number of requests from a single IP_
server.use(
rateLimit({
windowMs: 15 * 60 * 1000 , _// 15 minutes_
max: 100 , _// limit each IP to 100 requests per windowMs_
})
);


const express = require("express");
const helmet = require("helmet");
const morgan = require("morgan");
const cors = require("cors");
const rateLimit = require("express-rate-limit");
const usersRouter = require("./routers/usersRouter");
const lightningRouter = require("./routers/lightningRouter");
const server = express();
server.use(helmet());
server.use(morgan("common"));
server.use(cors());
server.use(
rateLimit({
windowMs: 15 * 60 * 1000 , _// 15 minutes_
max: 100 , _// limit each IP to 100 requests per windowMs_
})
);
server.use(express.json());
server.get("/", ( _req_ , _res_ ) => {
_res_ .status( 200 ).json({ message: "I'm alive!" });
});
server.use("/users", usersRouter);
server.use("/lightning", lightningRouter);
const PORT = process.env.PORT || 5500 ;
server.listen(PORT, () => {
console.log(`Server listening on port ${PORT}`);
});


### Let’s add one more tool before we test!

```
Nodemon
Nodemon is a utility tool for Node.js that automatically restarts the server whenever a change
is detected in the code. This saves us the hassle of having to manually restart the server after
each change we make
```
#### Run npm install nodemon in your terminal


### Update scripts in package.json to use nodemon

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
Update the scripts property to use nodemon whenever we run npm run start


### Run the server and test our new middleware

- Now after running `npm run start` we should see the server starting up
    with nodemon
- In the screenshot below I made a request which was logged by morgan
    on the bottom line of the console


### Testing Helmet

##### We can now check

##### the response from

##### our last request and

##### verify that helmet is

##### working by checking

##### for all the additional

##### headers that should

##### be added to the

##### response


### A bit more on rate limiting and Denial of

### Service attacks

Denial of Service (DoS) attacks are a common type of cyber attack where the
attacker tries to overwhelm a server or network with traffic, making it unavailable to
legitimate users. One common method of DoS attacks is to flood the server with a
high volume of requests, causing the server to crash or slow down to the point
where it is unusable.
In fact, many types of cyber attacks boil down to some form of repetitive or looping
behavior. Attackers often use automated tools or scripts that repeatedly perform
certain actions or send a high volume of requests to a server or network, making it
difficult for the system to keep up with the incoming traffic. Understanding this basic
mechanism of attacks can help in developing countermeasures to mitigate or

##### prevent such attacks.


const http = require('http');
function sendRequest() {
const options = {
host: 'localhost',
port: 5500 ,
path: '/',
method: 'GET',
};
const req = http.request(options, ( _res_ ) => {
console.log(`Response status code: ${ _res_ .statusCode}`);
});
req.on('error', ( _e_ ) => {
console.error(`Request error: ${ _e_ .message}`);
});
req.end();
}
setInterval(sendRequest, 100 );

### Now let’s attack our own server to test our

### rate limiting middleware!


### We can run this code in the terminal

- Open up a new terminal session
- Run ‘node’
- Paste in the code from the previous slide
- Now you should see requests flying by and the status code will change from
    200 to 429 after 100 requests


### Review

● Middleware is any process or function that runs in between the request and
response in our API
● Securing our server with middleware: using helmet for security, morgan for logging,
cors for enabling cross-origin requests, and express-rate-limit for limiting requests
per IP
● The order in which middleware functions are declared matters
● Middleware functions can terminate the request-response cycle by sending a
response to the client or passing control to the next middleware function
By using middleware, we can modularize our server logic and add additional functionality
to our server. We learned how to use several popular middleware packages to enhance
our server's security and performance. Remember to always consider security when
building an Express server and to use middleware packages that can help protect against
common attacks like DoS attacks.


### Resources

- Official Express Middleware documentation:
    https://expressjs.com/en/guide/using-middleware.html
- A tutorial on using middleware in Express:

#### https://www.tutorialspoint.com/expressjs/expressjs_middleware.htm

- Express.js Fundamentals - 6 - Middleware Explained:
    https://www.youtube.com/watch?v=9HOem0amlyg
- A guide to using helmet middleware for securing Express apps:
    https://github.com/helmetjs/helmet#how-it-works
- A tutorial on using morgan middleware for logging in Express:
    https://expressjs.com/en/resources/middleware/cors.html


