## Lesson 13: Deploy your

## production Database /

## Node

#### With Heroku and Voltage


### Considerations before deploying

- Security
    - The node/app is susceptible to attacks
    - The app has ADMIN rights to your node (your funds could be stolen)
    - The activity of the wallet is not private (unless you update it to be so)
- Cost
    - There is a cost hosting the server
    - There is a cost hosting the database
    - There is a cost hosting a node
- What are your goals?
    - Are you trying to show off the mainnet wallet
    - Does testnet suffice?
    - Does it need to be live and deployed?


### What is Heroku?

**Heroku** is a cloud platform service that allows developers to deploy, manage, and scale
applications without a lot of overhead. It abstracts away the complexities of managing

#### servers, infrastructure, and databases, allowing developers to focus purely on the code.

- Heroku is made specifically for
    deploying servers / databases
- Heroku has consistent performance,
    it’s easy to setup, and easy to scale.
- Heroku used to have a basic free plan
    but now you need to pay a small fee
    for both the server and the postgres
    instance


**Platform as a Service (PaaS)** : Heroku is a PaaS, which means it provides both the environment to run applications and the
infrastructure to host them. Think of it like a combination of a playground and the land it's on. You bring the toys (your
application), and Heroku provides the place and ensures the ground is always ready for play (auto-maintenance, updates,
etc.).

**Simple Deployment** : Deploying an application to Heroku is often as simple as pushing your code using Git. You can think of
it like uploading your photos to a cloud service, but in this case, it's your application's code.

**Supports Multiple Languages** : Heroku supports several programming languages like Ruby, Java, Python, Node.js, and
more. It's like a multi-cuisine restaurant where you can bring recipes (code) from different cuisines (programming languages)
and expect them to be cooked (run) perfectly.

**Dynos** : Heroku uses containers called "dynos" to run applications. Imagine these as individual mini-computers within a big
computer. Each one can run your application independently, and you can have many of them to handle more users.

**Add-ons** : Heroku provides a marketplace of add-ons (services and tools) that you can attach to your applications. It's like
adding extra toppings on a pizza. Need a database? Add it! Need email services? Add that too!

**Scalability** : If your application becomes popular and receives more traffic, Heroku can easily scale up to handle the load. It's
like having a magic room that expands when more guests arrive.

### What is Heroku?


### Steps to deploying our server / database

1. Create a new Heroku ‘App’
2. Connect our backend repo
3. Add a web ‘Dyno’
4. Add Postgres Buildpack
5. Setup secret ENV variables
6. Deploy server
7. Test calling the root endpoint
8. Test creating an admin user
9. Test getting all users


### Creating our Heroku app


### Connecting Heroku to our backend code

Go to the ‘Deploy’ tab and select Github as your deployment method and add the
pleb-wallet-be as the repo.

Now hit deploy!


### Deploying our backend code


### Watching the logs and testing the root endpoint


### Watching the logs and testing the root endpoint


### Express rate limit error

If you get this error:

Update index.js like so:

```
The error message you're seeing is due to the fact that
you're deploying your application behind a reverse proxy
(like many cloud hosting solutions do), and
express-rate-limit is warning you that it might not
be getting the correct client IP addresses.
```
```
The above tells Express to trust the first proxy it
connects to. If your app is behind multiple proxies,
you can set 'trust proxy' to the number of proxy
servers or 'loopback' to trust all proxies.
```

### Invoice datetime error

While we’re already fixing bugs let’s look into this one I got when testing the
deployment: error: update "invoices" set "settled" = $1, "settle_date" = $2 where "payment_request" = $3 returning * - date/time
field value out of range: "1698189002"

Our db schema is expecting a longer ‘UNIX’ timestamp:

Fix in lnd.js: (^) // If the invoice exists, update it in the database
if (existingInvoice) {
const settleDate = new Date(data.settle_data *
1000 ).toISOString();
await Invoice.update(data.payment_request, {
settled: data.settled,
settle_date: settleDate,
});
} else {
console.log("Invoice not found in the database");
}


### Enabling a basic web ‘Dyno’

```
What are Dynos on Heroku? https://www.heroku.com/dynos
```
Put simply Dynos are the isolated instances of your server that run on hardware in the
cloud


### Adding ENV secrets

Click “Reveal Config Vars” and add new secure secrets for both ADMIN_KEY and
JWT_SECRET

Later we will add more config Vars for our Lightning node once we deploy it


### Create a new user


### Test the login endpoint with a “seeded” user


### Get all users and see our newly created user

```
The database is working!
```

### Adding Postgres Buildpack

##### Click on “Resources” tab and search for Heroku Postgres

##### (Sadly Heroku has raised prices so it’s 5$ a month for a pg database)


### Database url env variable should be added


### Redeploy


### Run migration


### Create a admin user in your production db

### then login and check all users again


### Now update your adminKey and JWT if you

### haven’t to be secure secrets


# Deploy a Production

# Lightning node

#### With Voltage


### Create a node

- Visit https://nodes.voltage.cloud
- Click ‘Create Node’


### Choose LND


### Choose your node type

You can pick a Lite Node to start and use testnet if you want to do further testing
but we will use mainnet to start ripping some real invoices


### Create a username/password for your node

```
And be sure to write them down!
```

### Get free inbound channel

After your node is done initializing you should see this message on your node's
home page to request a free inbound channel! This will allow us to instantly start
receiving sats into our pleb-wallet from the wider network.


### Update our connection options to use

### LNDConnect

Lightning Node Connect (LNC) is a novel mechanism to create an end-to-end
encrypted connection between a Lightning Network node (initially just LND, other
implementations might follow) and a web browser.

This will replace our HOST, CERT, and MACAROON env variables and combine all
of that information into a single URI


### Grab a new LNDConnect URI from the

### Voltage connections dropdown


### Add LND_CONNECT_URI to ENV variables

```
This should be the full set off all of your ENV variables now
```

### Redeploy and test a lightning endpoint

After redeploying we should now be able to call our /lightning/channelBalance
endpoint


## Our production Server /

## DB / Node are deployed!

#### Everything is deployed in production and working!


### Final step testing with the deployed frontend

- Deploy https://github.com/AustinKelsay/pleb-wallet-frontend to vercel
- Add the REACT_APP_BACKEND_URL env variable on the vercel deployment
    and put it as the deployed backend
- Go through the full flow testing an admin and non admin user on the deployed
    frontend


