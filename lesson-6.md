# Lesson 6: Intro to the

# Lightning Network

##### Theory / Implementations / protocol vs app dev / Lightning

##### Development environments


#### The Lighting Network

● **Second-layer solution:** Built on top of
Bitcoin's blockchain for faster, more
scalable transactions

● **Off-chain transactions:** Enables
payments between users without
recording every transaction on the
blockchain

● **Micropayments:** Allows for small,
instant transactions with very minimal
fees, expanding the range of possible
use cases for Bitcoin


● **Network participants:** Lightning nodes

```
are computers that participate in the
Lightning Network by running compatible
software
```
● **Routing payments:** Nodes help route

```
transactions through the network by
forwarding payments between channels
```
● **Decentralization:** A large number of

```
nodes ensures the network remains
decentralized and resistant to censorship
or control by a single entity
```
#### Lightning Nodes


#### Lightning Channels

● **Payment channels:** Temporary, private
channels between users allow for
multiple transactions without requiring
on-chain confirmations

● **Multi-signature wallets:** Both parties in
a channel have control over funds,
ensuring security and trust

● **Network of channels:** Users can route
payments through multiple channels,
even if they don't have a direct channel
with the recipient


### Lets rip it on the whiteboard


```
A Lightning Network implementation is a software package that implements the
```
Lightning spec, allows nodes to participate in the Lightning Network, and provides the

```
necessary functionalities for a Lightning node
```
#### What is a Lightning implementation


#### Protocol dev VS App dev

```
PlebDevs course #
Frontend
```
**PlebDevs course #
Backend**

```
Protocol Developer
Lightning
```
```
Protocol Developer
Bitcoin
```

#### The different Lightning Implementations

**- LND:**
    - Stands for “Lightning Network Daemon”
    - The most popular / widely used LN implementation
    - Developed by Lightning Labs, written in Go
    - Rich feature set and extensive documentation
**- CoreLightning:**
    - Developed by Blockstream
    - Written in C, optimized for performance and reliability
    - Flexible plugin system for extended functionality
    - Feature rich
**- Eclair:**
    - Developed by ACINQ
    - Written in Scala, Highly scalable
    - User-friendly wallet app and mobile SDK for app development
    - Offers additional features such as channel management tools
**- LDK:**
    - Created by Spiral (formally Square crypto)
    - Written in Rust, focusing on safety and performance
    - Modular, customizable toolkit for building Lightning implementations and applications
    - Enables seamless integration with various Bitcoin wallets and applications


## Setting up a local

## Lightning Development

## Environment

##### With https://polarlightning.com


#### Install Docker Desktop and Polar

- Docker Desktop
- Polar


#### 1. Open Docker Desktop, wait for it to start


#### 1. Open Docker Desktop, wait for it to start


#### 2. Open Polar, Create a Lightning network


#### 3. Create your network

- Give it a name
- You can use the default network or customize it / add nodes
- Make sure that you have at least 1 Bitcoin Core node though


#### 4. Click Start


#### Let’s create some channels / invoices in Polar


#### Resources

- Bitcoin's Lightning Network, Simply Explained! -

###### https://www.youtube.com/watch?v=rrr_zPmEiME

- A Technical Introduction to The Lightning Network -

###### https://www.youtube.com/watch?v=E1n3sKKPD_k&t=330s

- The Lightning whitepaper - https://lightning.network/lightning-network-paper.pdf
- Lightning Series: Mastering Lightning with Andreas M. Antonopoulos & René

###### Pickhardt - https://www.youtube.com/watch?v=zG8PZsHLung

- Understanding the Lightning Network (series) -

###### https://bitcoinmagazine.com/technical/understanding-the-lightning-network-part-

###### building-a-bidirectional-payment-channel-

- Mastering the Lightning Network Book (free) - https://github.com/lnbook/lnbook


