##  Fall 2023 Blockchain and SQL Fundamentals    Lab 3
### Carnegie Mellon University MSCF
### Due: Wednesday, December 6, 2023 11:59 PM
### 10 Points
### Deliverable: A single .pdf file named Lab3.pdf with clearly labelled answers.

**Learning Objectives:**

In Part A, we will work **without a smart contract**. We will start the Hardhat
network in its own shell and interact with the shell via the MetaMask wallet.
Part A illustrates client server computing and a simple decentralized payment system.

In Part B, we will work with an **ERC20 token contract**. ERC20 tokens are at the heart
of many Decentralized Finance (DeFi) applications. We will interact with the contract using MetaMask.

In Part C, we will work with a **Uniswap contract**. Uniswap is an important
Decentralized Finance (DeFi) application.

**Deliverable:**

Please compile your answers to questions EXX through EXX into a single PDF file named ‘Lab3.pdf’. Ensure that the file is well-organized and properly labeled. Submit this single PDF file to Canvas.

## Part A. Using MetaMask to interact with a Hardhat network running locally.

1. In an empty directory named Lab3_PartA, begin by setting up an npm package. When prompted, hit return and select the defaults.
```
npm init     
```
2. We will use Hardhat for development.

```
npm install --save-dev hardhat
```
3. Create an empty Hardhat configuration.
```
npx hardhat init     
```
4. We will develop using the Hardhat plugins from the nomic foundation.
```
npm install --save-dev @nomicfoundation/hardhat-toolbox
```
5. In the hardhat.config.js file, use the following:

require("@nomicfoundation/hardhat-toolbox");
/** @type import('hardhat/config').HardhatUserConfig */
module.exports = {
  defaultNetwork: "localhost",
  networks: {
    hardhat: {
      chainId: 1337
    },
    localhost: {
      url: "http://localhost:8545"
    }
  },
  solidity: "0.5.14",
};


6. Within the directory Lab3_PartA, start a JSON-RPC server that will run on top of the Hardhat Ethereum Virtual Machine. After startup, this server will become available at https://127.0.0.1:8545.
```
npx hardhat node
```

7. Leave this server running. Our next activity is to connect to it using MetaMask.

8. Install MetaMask in the Chrome browser and read over the following brief guide to
MetaMask.

```
MetaMask Cheat Sheet

MetaMask is self-custodial (keeps private keys local) by design.

The Secret Recovery Phrase (SRP) is used to generate public and private key pairs.
A user might have many key pairs derived from the same recovery phrase.

Under three dots menu:
   Account details
      Change the name on the account
      View the public key
      Copy the public key
      View the private key if you have the password
   Settings
      Network
        Add a Network
           For Hardhat, set New RPC URL to: http://localhost:8545 and chain ID to 1337. The currency symbol is ETH.
      
   Snaps are third-party extensions to MetaMask

Left of three dots menu:
   Shows accounts connected
   These web sites will have access to your public key (only!) of your accounts.

Middle drop down arrow
      Select an account
          Buy & sell, send, swap, bridge, portfolio
          The bridge option requires that you have tokens on the source and destination network.
          To change tokens from one blockchain to another, we need to connect to a bridge service.
          MetaMask does not currently work with Bitcoin or non-ethereum blockchains.
          Many Bitcoin wallets exist, e.g. see BitCoinWallet.com.
          The portfolio option allows you to view several accounts at once.
          Three dot menu to Account Details and change the name on an account or access its public key via copy and paste or a QR code.

      Add account or hardware wallet
          Add a new account
          Import an account
          Add Hardware wallet

Top left drop down
      Add a network
          General
             Currency conversion
             Primary currency
          Advanced
             Clear activity and nonce data from current account and network
             Reveal secret recovery phrase (SRP)
             SRP is deterministic and may be used to generate many key pairs (without randomness)

          Show test networks
             Turn on when using Hardhat network for testing
```

9. Using MetaMask, select the Hardhat Network. You will need to point to the location where you started the server (https://127.0.0.1:8545).

10. From the shell where you started the server, copy the private key of the first account to the clipboard and import this key into MetaMask. Name this new account "Alice".

11. From the shell where you started the server, copy the private key of the second account to the clipboard and import this key into MetaMask. Name this new account "Bob".

E0. From Alice's account, send 20 ETH to BOB. Copy the transaction details as shown on the server's shell to your single, well labeled pdf document.

E1. From Bob's account, send 30 ETH to Carol. Carol's account is the third account and we are
not importing her private key into MetaMask. We need only her public key to perform
the transfer. Copy the transaction details as shown on the server's shell to your single, well labeled pdf document.
