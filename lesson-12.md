# Lesson 12: Connecting

# to the Frontend App

###### Connecting our new backend to the pleb wallet frontend app and

###### testing locally


Pay Invoices Create Invoices^

```
Grab wallet balances
Grab and format
all Invoices
```

### pleb-wallet-frontend

If you aren’t interested in the details of how we update the frontend to work with
our new backend you can just grab the latest updated pleb-wallet-frontend
below

- Original frontend from course #
    https://github.com/AustinKelsay/pleb-wallet-react/tree/master
- Updated frontend for course #
    https://github.com/AustinKelsay/pleb-wallet-frontend/tree/main


## Walkthrough / Review of

## the pleb-wallet-frontend

##### A simple single page Lightning wallet UI written in React


### Running it all locally

- Open up Docker Desktop
- Open up Polar and start the Polar network you’ve been testing with thus far
- Start running the backend npm run start
- Clone down the new pleb-wallet-frontend git clone
    https://github.com/AustinKelsay/pleb-wallet-frontend.git
- Open it up in your text editor and open the terminal and run npm i to install
- Start the frontend with npm run start

#### The full stack should now be running locally!

#### Let’s check it out


### Full test of the frontend integration

```
Regular user:
```
- Create a new user
- Login
- Create and invoice (for the wallet)
- Pay that invoice (from ‘Bob’ in Polar)
- Try to pay an invoice from the wallet (should fail)

```
Admin user:
```
- Login as our seeded admin user { username: “Alice”, password: “pass1” }
- Create and invoice (for the wallet)
- Pay that invoice (from ‘Bob’ in Polar)
- Create an invoice (for ‘Bob’ in Polar)
- Pay that invoice from the wallet (should succeed)


