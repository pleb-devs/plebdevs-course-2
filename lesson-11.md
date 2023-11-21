# Lesson 11: Connecting

# the API and Database

## Create db models (functions that interact with our database) for our

## API to use to talk to the database


#### Full knex setup from a high level


#### What are db models in knex?

Database models in Knex.js are the defined structures and helper functions that you
will use to interact with your database. Essentially, they provide an interface for
querying and manipulating the data stored in a specific table of your database.
Here's how to think about it:
**Data Representation** : Each model usually represents a particular table in your
database. For example, a Users model would represent a users table.
**Querying and Manipulating Data** : Models provide methods for retrieving and
changing data in your database. For example, a findAllUsers function on your
Users model would use the Knex SDK to retrieve all users from your users

###### table.


#### Knex queries

Knex provides a straightforward way to build
SQL queries with JavaScript.
The knex cheatsheet has some common
examples of SELECT queries and WHERE
clauses in Knex.


#### Getting started on our knex queries

- Add a ‘models’ directory inside of your ‘db’ folder
    - Now your db folder should look like this:
- Create a new file called user.js
    - Import db from our dbConfig at the top of the file:


#### Remember dbConfig?

dbConfig imports the knex library, retrieves the appropriate database configuration from
the knexfile based on the current environment (defaulting to "development"), and
initializes a knex instance with this configuration. The initialized instance db is then
exported to be used in other files, particularly the models, to interact with the database.
const knex = require("knex");
const config = require("../knexfile");
const env = process.env.NODE_ENV || "development";
const db = knex(config[env]);
module.exports = db;


#### Creating our user models in user.js

_// First, we require our configured instance of knex from the dbConfig.js file._
const db = require("../dbConfig");
_// We then export an object with several methods, each representing a different database operation_
module.exports = {
_// The findAll method retrieves all records from the 'users' table_
findAll: () => {
return db("users");
},
_// The findByUsername method retrieves the first record in the 'users' table where the username matches the provided username_
findByUsername: ( _username_ ) => {
return db("users").where({ username }).first();
},
_// The create method inserts a new record (the 'user' object) into the 'users' table and returns the newly created user_
create: ( _user_ ) => {
return db("users").insert( _user_ ).returning("*");
},
_// The update method finds a user in the 'users' table with the matching id and updates their record with the new data contained in the
'user' object. It then returns the updated user_
update: ( _id_ , _user_ ) => {
return db("users").where({ id }).update( _user_ ).returning("*");
},
_// The delete method finds a user in the 'users' table with the matching id and removes their record from the table_
delete: ( _id_ ) => {
return db("users").where({ id }).del();
},
};


##### Update getUsers endpoint in routers/usersRouter

_// add imports for the user model and our two auth middlewares_
const User = require("../db/models/user");
const authenticate = require("./middleware/authenticate.js");
const authenticateAdmin = require("./middleware/authenticateAdmin.js");
_// GET all users
// Before processing the request, we apply the 'authenticateAdmin' middleware to protect the ability to see all users_
router.get("/", authenticateAdmin, ( _req_ , _res_ ) => {
_// Call the 'findAll' method from our User model. This method retrieves all user records from the database._
User.findAll()
.then(( _users_ ) => {
_// If the promise resolves (i.e., the operation was successful), we send back a response with a status of 200 (OK)
// and the list of users retrieved from the database.
res_ .status( 200 ).json( _users_ );
})
.catch(( _err_ ) => {
_// If the promise is rejected (i.e., the operation fails), we send back a response with a status of 500 (Internal Server
Error)
// and the error that occurred. This might be due to a database issue, a network issue, etc.
res_ .status( 500 ).json( _err_ );
});
});


##### Update getUserByUsername endpoint

_// GET user by their username
// Using 'authenticate' middleware to verify the client's authentication token._
router.get("/user", authenticate, async ( _req_ , _res_ ) => {
_// Get the JWT (JSON Web Token) from the 'authorization' header of the request._
const token = _req_ .headers.authorization;
_// Retrieve the secret key for JWT verification from environment variables._
const secret = process.env.JWT_SECRET;
_// Use the 'verify' method from the 'jsonwebtoken' library to decode the token._
jwt.verify(token, secret, ( _err_ , _decodedToken_ ) => {
_// If an error occurred during token decoding (perhaps because the token is invalid or the secret is incorrect),
// respond with a status of 401 (Unauthorized) and a message about the error._
if ( _err_ ) {
_res_ .status( 401 ).json({ message: "Error decoding token", Error: _err_ });
}
_// If the token was successfully decoded, find a user with a username that matches the username in the decoded token._
User.findByUsername( _decodedToken_ .username)
.then(( _user_ ) => {
_// If the promise resolves (i.e., the user was found), respond with a status of 200 (OK) and the user's data._
console.log( _user_ );
_res_ .status( 200 ).json( _user_ );
})
.catch(( _err_ ) => {
_// If the promise is rejected (i.e., an error occurred), respond with a status of 500 (Internal Server Error) and the error.
res_ .status( 500 ).json( _err_ );
});
});
});


##### Update register endpoint

_// POST a user to register_
router.post("/register", ( _req_ , _res_ ) => {
_// We are using the bcrypt library to hash the password provided in the request body.
// This enhances security by ensuring that the plain text password isn't stored directly in the database.
// The '14' here is the cost factor that determines the complexity of the hashing process._
const hash = bcrypt.hashSync( _req_ .body.password, 14 );
_// We then replace the plain text password in the request body with the hashed password.
req_ .body.password = hash;
_// The updated request body (which now includes the hashed password) is passed to the 'add' method from our User model.
// This method will create a new user record in the database._
User.create( _req_ .body)
.then(( _user_ ) => {
_// If the promise resolves (i.e., the operation was successful), we send back a response with a status of 200 (OK) and the newly created user.
res_ .status( 200 ).json({ data: _user_ });
})
.catch(( _err_ ) => {
_// If the promise is rejected (i.e., the operation fails), we send back a response with a status of 500 (Internal Server Error) and the error
that occurred.
res_ .status( 500 ).json({ error: _err_ });
});
});


##### Reviewing our login endpoint

_// POST a user to login_
router.post("/login", ( _req_ , _res_ ) => {
_// Extract the username and password from the request body_
const { username, password } = _req_ .body;
_// Placeholder user object - later we will fetch the real user from the database_
const DBuser = {
username: "test",
password: "pass1",
};
_// Hash the password from the request body using bcrypt
// Later we will compare this hash to the hash stored in the database
// But for now, we will just do it manually_
const hashedPassword = bcrypt.hashSync(DBuser.password, 14 );
_// Check if the user exists and the password matches using bcrypt_
if (DBuser && bcrypt.compareSync(password, hashedPassword)) {
_// Generate a JSON Web Token (JWT) for the user_
const token = generateToken(DBuser);
_// Send a success response with the JWT and user data
res_
.status( 200 )
.json({ message: `Welcome ${DBuser.username}!`, token, DBuser });
} else {
_// Send an error response if the credentials are invalid
res_ .status( 401 ).json({ message: "Invalid credentials" });
}
});


##### Update login endpoint

_// POST a user to login_
router.post("/login", ( _req_ , _res_ ) => {
_// Extract the 'username' and 'password' fields from the request body. These are provided by the client when they make the request._
const { username, password } = _req_ .body;
_// Call the 'findByUsername' method from our User model. This method retrieves the first user record from the database that matches the provided username._
User.findByUsername(username)
.then(( _user_ ) => {
_// Check if a user was found and if the provided password, when hashed, matches the hashed password stored in the database._
if ( _user_ && bcrypt.compareSync(password, _user_ .password)) {
_// If both conditions are met, the login is successful. We then generate a token for the user. This token will be used for subsequent authenticated requests._
const token = generateToken( _user_ );
_// Respond with a status of 200 (OK), a welcome message, the generated token, and user information.
res_
.status( 200 )
.json({ message: `Welcome ${ _user_ .username}!`, token, user });
} else {
_// If the user wasn't found or the password doesn't match, respond with a status of 401 (Unauthorized) and a message indicating the credentials were invalid.
res_ .status( 401 ).json({ message: "Invalid credentials" });
}
})
.catch(( _err_ ) => {
_// If an error occurs during the process, log the error and respond with a status of 500 (Internal Server Error) and the error._
console.log( _err_ );
_res_ .status( 500 ).json({ error: _err_ });
});
});


##### Update the update user endpoint

_// PUT a user to update them
// The 'authenticateAdmin' middleware function is used to ensure that only the admin is authorized to update the
user._
router.put("/:id", authenticateAdmin, ( _req_ , _res_ ) => {
_// Call the 'update' method from our User model with the provided id and body of the request.
// The 'update' method will update the user record in the database that matches the provided id with the data in the
request body._
User.update( _req_ .params.id, _req_ .body)
.then(( _user_ ) => {
_// If the promise resolves (i.e., the operation was successful), we send back a response with a status of 200
(OK)
// and the updated user record from the database.
res_ .status( 200 ).json( _user_ );
})
.catch(( _err_ ) => {
_// If the promise is rejected (i.e., the operation fails), we send back a response with a status of 500
(Internal Server Error)
// and the error that occurred. This might be due to a database issue, a network issue, etc.
res_ .status( 500 ).json( _err_ );
});
});


##### Update the delete user endpoint

_// DELETE a user_
router.delete("/:id", authenticateAdmin, ( _req_ , _res_ ) => {
_// Before running the delete operation, the `authenticateAdmin` middleware function is run.
// This function checks whether the user making the request has the appropriate admin privileges.
// Then we call the 'delete' method from our User model, passing in the user id extracted from the route parameters._
User.delete( _req_ .params.id)
.then(( _user_ ) => {
_// If the promise resolves (i.e., the operation was successful), we send back a response with a status of 200
(OK)
// and the user that was deleted from the database.
res_ .status( 200 ).json( _user_ );
})
.catch(( _err_ ) => {
_// If the promise is rejected (i.e., the operation fails), we send back a response with a status of 500
(Internal Server Error)
// and the error that occurred. This might be due to a database issue, a network issue, etc.
res_ .status( 500 ).json( _err_ );
});
});


##### Update authenticateAdmin middleware

const jwt = require("jsonwebtoken");
const User = require("../../db/models/user");
module.exports = ( _req_ , _res_ , _next_ ) => {
_// Extracting the token from the request header_
const token = _req_ .headers.authorization;
_// Setting up the JWT secret for token verification_
const secret = process.env.JWT_SECRET || "Satoshi Nakamoto";
_// Setting the admin key for admin verification_
const key = process.env.ADMIN_KEY || "1234";
_// If token is present, attempt to verify it using the JWT module_
if (token) {
jwt.verify(token, secret, async ( _err_ , _decodedToken_ ) => {
_// If token is not verified, return 401 error_
if ( _err_ ||! _decodedToken_ ) {
_res_ .status( 401 ).json({ message: "Error with your verification" });
} else {
_// If token is verified, find the user using their username from the database_
const user = await User.findByUsername( _decodedToken_ .username);
_// Extracting admin key from user object if it exists_
const adminKey = user?.adminKey?.toString() ?? "";
_// Checking if extracted admin key matches with the one in env variables_
if (adminKey !== key) {
_// If admin key does not match, return 401 error
res_ .status( 401 ).json({ message: "Must be an admin" });
} else {
_// If admin key matches, let the endpoint continue executing_
next();
}
}
});
} else {
_// If no token is present, return 401 error
res_ .status( 401 ).json({ message: "No token!" });
}
};


# Test the updated

# /users endpoints

## With Postman


#### Flow for testing all of the /users endpoints

- Call POST /users/register with a new user object containing username,
    password, and adminKey set to “1234”
- Now call POST /users/login with your new user’s username and password.
- Copy the JWT that’s generated on login
- Add the JWT to an “authorization” header in a call to GET /users/user and
    see that you can get your newly created user
- Add the JWT to an “authorization” header in a call to PUT /users/:id with a
    body that updates the username
- Login again since you updated your username
- Take the token and add it to the GET /users header call it to get all users and
    see that your username has been updated
- Finally add the token to the DELETE /users/:id and check that the user was
    successfully deleted


## Now we can create a

## similar db model for

## invoices

### First add an invoice.js file inside of db/models directory


#### db/models/invoice.js

_// First, we require our configured instance of knex from the dbConfig.js file._
const db = require("../dbConfig");
module.exports = {
_// The findAll method retrieves all records from the 'invoices' table_
findAll: () => {
return db("invoices");
},
_// The findOne method retrieves the first record in the 'invoices' table where the payment_request matches the provided payment_request_
findOne: ( _payment_request_ ) => {
return db("invoices").where({ payment_request }).first();
},
_// The create method inserts a new record (the 'invoice' object) into the 'invoices' table and returns the newly created invoice_
create: ( _invoice_ ) => {
return db("invoices").insert( _invoice_ ).returning("*");
},
_// The update method finds an invoice in the 'invoices' table with the matching payment_request and updates their record with the new data
contained in the 'invoice' object. It then returns the updated invoice_
update: ( _payment_request_ , _invoice_ ) => {
return db("invoices")
.where({ payment_request })
.update( _invoice_ )
.returning("*");
},
_// The delete method finds an invoice in the 'invoices' table with the matching id and removes their record from the table_
delete: ( _id_ ) => {
return db("invoices").where({ id }).del();
},
};


const Invoice = require("../db/models/invoice.js");
...
_// GET all invoices from the database_
router.get("/invoices", ( _req_ , _res_ ) => {
_// Call the 'findAll' method from our Invoice model. This method retrieves all invoice records from the database._
Invoice.findAll()
.then(( _invoices_ ) => {
_// If the promise resolves (i.e., the operation was successful), we send back a response with a status of 200
(OK)
// and the list of invoices retrieved from the database.
res_ .status( 200 ).json( _invoices_ );
})
.catch(( _err_ ) => {
_// If the promise is rejected (i.e., the operation fails), we send back a response with a status of 500
(Internal Server Error)
// and the error that occurred. This might be due to a database issue, a network issue, etc.
res_ .status( 500 ).json( _err_ );
});
});

#### Update get all invoices endpoint


#### Update create invoice endpoint

_// POST required info to create an invoice_
router.post("/invoice", authenticate, ( _req_ , _res_ ) => {
_// The 'authenticate' middleware function is called before the main handler.
// This function checks if the user making the request is authenticated.
// Extract 'value', 'memo', and 'user_id' properties from the body of the incoming request._
const { value, memo, user_id } = _req_ .body;
_// Call the 'createInvoice' function, passing the extracted properties as an object.
// This function creates a new invoice record in the database._
createInvoice({ value, memo, user_id })
.then(( _invoice_ ) => {
_// If the promise resolves (i.e., the operation was successful), we send back a response with a status of 200 (OK)
// and the invoice object retrieved from the database.
res_ .status( 200 ).json( _invoice_ );
})
.catch(( _err_ ) => {
_// If the promise is rejected (i.e., the operation fails), we send back a response with a status of 500 (Internal Server
Error)
// and the error that occurred. This could be due to a database issue, a network issue, etc.
res_ .status( 500 ).json( _err_ );
});
});


#### Update createInvoice function in lnd.js

const createInvoice = async ({ _value_ , _memo_ , _user_id_ }) => {
_// Use the 'addInvoice' method from the Lightning service of the 'grpc' module to create an invoice.
// This method requires an object parameter with 'value' and 'memo' properties.
// This method is asynchronous, so we use 'await' to pause execution until it completes._
const invoice = await lnd.services.Lightning.addInvoice({
value: _value_ ,
memo: _memo_ ,
});
_// After creating the invoice with the Lightning service, we create a record in our own database using the 'Invoice' model's 'create' method.
// This method requires an object parameter with properties for 'payment_request', 'value', 'memo', 'settled', 'send', and 'user_id'.
// Note that 'settled' is set to false (since the invoice has just been created and is not yet paid), and 'send' is also false (since we haven't
sent the invoice yet)._
await Invoice.create({
payment_request: invoice.payment_request,
value: _value_ ,
memo: _memo_ ,
settled: false,
send: false,
user_id: _user_id_ ,
});
_// Finally, the function returns the invoice that was created with the Lightning service._
return invoice;
};


#### Update invoiceEventStream in lnd.js

const invoiceEventStream = async () => {
await lnd.services.Lightning.subscribeInvoices({
add_index: 0 ,
settle_index: 0 ,
})
.on("data", async ( _data_ ) => {
if ( _data_ .settled) {
_// Check if the invoice exists in the database_
const existingInvoice = False;
_// If the invoice exists, update it in the database_
if (existingInvoice) {
_// update db_
} else {
console.log("Invoice not found in the database");
}
}
})
.on("error", ( _err_ ) => {
console.log( _err_ );
});
};
const invoiceEventStream = async () => {
await lnd.services.Lightning.subscribeInvoices({
add_index: 0 ,
settle_index: 0 ,
})
.on("data", async ( _data_ ) => {
if ( _data_ .settled) {
_// Check if the invoice exists in the database_
const existingInvoice = await Invoice.findOne( _data_ .payment_request);
_// If the invoice exists, update it in the database_
if (existingInvoice) {
await Invoice.update( _data_ .payment_request, {
settled: _data_ .settled,
settle_date: _data_ .settle_date,
});
} else {
console.log("Invoice not found in the database");
}
}
})
.on("error", ( _err_ ) => {
console.log( _err_ );
});
};


#### Update pay invoice endpoint

router.post("/pay", authenticateAdmin, async ( _req_ , _res_ ) => {
_// Extract the 'payment_request' property from the request body._
const { payment_request } = _req_ .body;
_// Use the 'payInvoice' function to attempt to pay the invoice. This function is asynchronous, so we use 'await' to pause execution until it completes._
const pay = await payInvoice({ payment_request });
_// If there was an error making the payment, we send back a response with a status of 500 (Internal Server Error) and the error message._
if (pay.payment_error) {
_res_ .status( 500 ).json(pay.payment_error);
}
_// If the payment was successful (indicated by the existence of 'pay.payment_route'), we create a new 'payment' record in the database._
if (pay?.payment_route) {
const payment = await Invoice.create({
_// The payment details include the original payment request, a flag indicating it was sent, the total amount, any fees, and the settlement details._
payment_request: payment_request,
send: true,
value: pay.payment_route.total_amt,
fees: pay.payment_route.total_fees,
settled: true,
settle_date: Date.now(),
});
_// After creating the new payment record, we send a response with a status of 200 (OK) and the details of the new payment record.
res_ .status( 200 ).json(payment);
}
});


# Test the updated

# /invoices endpoints

## With Postman / Polar


#### Creating an invoice through the API

- Startup polar just like we did in lesson 6 by opening Docker Desktop then Polar
- Click on your previously created network and start it up (you should still automatically
    connect to the Alice node configured in your .env)
- Login as {“username”: “Alice”, “password”: “pass1”} and grab the JWT
- Add the JWT to the authorization header for a POST /invoices/create call and we
    should be able to create a real invoice with our Alice node


#### Call the GET /invoices endpoint

You should see your newly created unpaid invoice (don’t forget the JWT!)


#### Now go into polar and have the Bob node pay

#### the created invoice


#### Call get all invoices endpoint again

You should now see that the invoice we created has been paid via the “settled” and
“settle_date” properties


#### Now lets have Bob create an invoice so we

#### can pay it


#### Send a request to pay an invoice

Add the invoice as the payment request and the user_id of the user paying (Alice)


## We have now fully

## connected the API and

## Database

And tested it!
