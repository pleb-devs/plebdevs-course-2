# Lesson 2: Learn Express Basics

## Servers / HTTP API / CRUD

### What are HTTP APIs?
- An HTTP API is a web-based interface that allows communication between different systems or applications over the internet.
- API stands for "Application Programming Interface".
- It uses the HTTP protocol to define a set of rules and standards for sending and receiving messages between client and server.
- The API provides a standardized way for different applications to interact with each other, without requiring direct access to the source code or internal workings of the application.
- At a high level, the API is just an interface for talking to our server using the HTTP protocol.

### What we know about API calls from course 1
- We made API calls "get requests" to request data from a few different APIs.
- We posted data to legend.lnbits using "post requests".

### API endpoints and CRUD operations
- HTTP APIs typically expose a set of 'endpoints' that can be accessed by clients using HTTP methods such as GET, POST, PUT, and DELETE.
- These endpoints provide access to specific resources or functionalities of the application and can be used to create, retrieve, update, or delete data (CRUD).
- HTTP APIs can be used for a variety of purposes, such as integrating different systems, enabling cross-platform communication, and building web applications or services.
- An open API is the way to have an open database.

> "Building an Express server is like riding a bike - except the bike is on fire, and the road is also on fire, and everything is on fire because you're using JavaScript" - probably Steve Jobs

# Let’s build our first Express Server!

To set up a new Node.js project, follow these steps:
- Create a new directory for your project by running the following command in the terminal: `mkdir pleb-wallet-backend`
- Navigate into the new directory by running: `cd pleb-wallet-backend`
- Initialize a new Node.js project by running: `npm init -y`
- Create a file called "index.js" by running: `touch index.js`
- Install express by running: `npm i express`

## Setting up our local environment

```javascript
// Import the Express library
const express = require("express");
// Create a new instance of the Express server
const server = express();
// Use the built-in JSON middleware to parse incoming JSON requests
server.use(express.json());
// Set up a route to handle GET requests to the root path
server.get("/", (req, res) => {
    // Send a JSON response with a "message" property set to "I'm alive!"
    res.status(200).json({ message: "I'm alive!" });
});
// Set the server to listen on the provided port, or 5500 if no port is specified
const PORT = process.env.PORT || 5500;
server.listen(PORT, () => {
    // Log a message to the console when the server starts listening
    console.log(`Server listening on port ${PORT}`);
});
```

### Calling our new API endpoint
- To start your server, run node index.js in your terminal.
- You should see the message "Server listening on port 5500".
- Now you can go into either Insomnia or Postman and make a GET request to "http://localhost:5500".

Success!

## The elements of an Express endpoint
- The req and res parameters in the route handler function represent the request and response objects, respectively.
- The req object contains information about the incoming request, such as the URL, HTTP headers, and any data sent in the request body.
- The res object is used to send a response back to the client, such as setting the response status code and sending data in the response body.

## Review
- Express is a popular Node.js web application framework that allows us to build powerful, scalable, and modular web applications.
- HTTP API stands for Hypertext Transfer Protocol Application Programming Interface; it is a set of guidelines for building web applications and provides a way for different software applications to communicate with each other.
- CRUD stands for Create, Read, Update, and Delete, which are the four basic functions that we can perform on a database.
- In Express, the req and res objects are used to handle HTTP requests and responses respectively. The req object represents the incoming HTTP request from the client, and the res object represents the outgoing HTTP response from the server.
