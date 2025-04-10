##  Spring 2025 Developing Blockchain Use Cases
### Carnegie Mellon University
### Due to Canvas: Wednesday, April 23, 2025
### Assigned: Monday, April 7, 2025
### 10 Points
### Deliverable: A single .pdf file named Lab2.pdf with clearly labelled answers.

**Learning Objectives:**

Our first major objective in this lab is to show how the MetaMask wallet can interact directly with the Ethereum blockchain. Note, we are using Hardhat as our blockchain emulator.

Second, we will demonstrate how the MetaMask wallet can interact with an ERC-20 token contract deployed to Hardhat.

Third, we will explore the application programmer interface (API) provided by DeFiLlama. DeFiLlama is a leading analytics platform for the decentralized finance (DeFi) ecosystem.

**Notes on Part A:**

A wallet like MetaMask has a limited user interface. If we need
to generate arbitrary transactions to various smart contracts, we would do
that using web3 middleware embedded in an application. Transaction creation is done
using such middleware but MetaMask still has a role to play. It has the keys
needed for signing and will ask the user for permissions. Here we explore
some of the features that MetMask provides.

In Part A, we will work **without a smart contract**. We will start the Hardhat
network in its own shell and interact with the shell via the MetaMask wallet.
Part A illustrates client server computing and a simple decentralized payment system.

**Notes on Part B:**

In Part B, we will work with an **ERC20 token contract**. ERC20 tokens are at the heart
of many Decentralized Finance (DeFi) applications. We will interact with the contract using MetaMask. This ERC20 token contract has been slightly modified from Lab 2. It contains a
fallback function that is called from MetaMask.

**Notes on Part C:**

In Part C, we will experiment with the DeFiLlama application programming interface (API). You are encouraged to use Copilot as a tool to help you develop an interesting query. Additionally, you will use Copilot to generate code to execute that query on the DeFiLlama API.

**Deliverable:**

This lab has 15 small exercises.

Please compile your answers to questions E0 through E14 into a single PDF file named ‘Lab3.pdf’. Ensure that the file is well-organized and properly labeled. Submit this single PDF file to Canvas.

## Part A. 3 Points. Using MetaMask to interact with a Hardhat network running locally. We are avoiding any use of the Ethereum Mainnet.

1. In an empty directory named Lab3_MetaMask, begin by setting up an npm package. When prompted, hit return and select the defaults.
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
5. Copy the following to the hardhat.config.js file. Note that we are using
31337 as the chain ID and http://127.0.0.1:8545 as the URL. Note too that this
is http and NOT https. The initialBaseFeePerGas is set to 0 for testing with
MetaMask and Hardhat.

```
require("@nomicfoundation/hardhat-toolbox");
/** @type import('hardhat/config').HardhatUserConfig */
module.exports = {
  defaultNetwork: "localhost",
  networks: {
    hardhat: {
      initialBaseFeePerGas: 0,
      chainId: 31337
    },
    localhost: {
      url: "http://127.0.0.1:8545"
    }
  },
  solidity: "0.5.14",
};

```


6. Within the directory Lab3_MetaMask, start a JSON-RPC server that will run on top of the Hardhat Ethereum Virtual Machine. After startup, this server will become available at http://127.0.0.1:8545.
```
npx hardhat node
```

7. This server will display 20 accounts. These are "Account 0" through "Account 19".
You should be able to see the public and private keys associated withe each account.
Leave this server running. Our next activity is to connect to it using MetaMask.

8. Install MetaMask in the Chrome browser and read over the following brief guide to
MetaMask. Note that you may need to reset the nonce before retrying after a
failed transaction.

9. Browse around MetaMask to get used to the interface. It can be a bit confusing. Below is a description of its interface.

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
      General
      Advanced
         Clear Activity Tab is useful when debugging. You may need to clear
         the nonce if you get a failed transaction. Each time you start the server, it wants
         the first nonce to be zero.
      Contacts
      Security and Privacy
      Experimental
      About
   Snaps are third-party extensions to MetaMask

Just left of three dots menu:
   Shows accounts connected
   These web sites will have access to your public key (only!) of your accounts.
   We are not connecting our account in this lab.

Main page
   Account name
   Public key (with a copy icon)
   Balance in ETH
   Balance in USD
   Under tokens (Notice ETH counts as "Tokens")
      Import Tokens (custom token)
         Alice Coin deployed to Hardhat counts as a "custom token".
         Enter contract address (the address of the token)
         Enter token symbol
         Enter token decimal
   Under Activity
      Recent activities on this account

Middle drop down arrow
      Select an account
          Buy & sell, swap, bridge, send, receive
          The bridge option requires that you have tokens on the source and
          destination network.
          To change tokens from one blockchain to another, we need to connect
          to a bridge service.
          MetaMask does not currently work with Bitcoin or non-ethereum blockchains. You could buy ERC-20 WBTC (wrapped bitcoin) with bitcoin and use WBTC on the wallet.
          Many Bitcoin wallets exist, e.g. see BitCoinWallet.com.
          The portfolio option allows you to view several accounts at once.

      Select account, note three dot drop down:
          The "Select an Account" page has a three dot menu next to each account.
          Select Account Details and change the name on an account or access its public key
          via copy and paste or a QR code.
          Remove account is an option for accounts that were imported with private keys.
          Remove account is not an option for accounts created by the recovery phrase.

      Add account or hardware wallet
          Add a new account (based on your secret phrase)
          Import an account
             You will need the account's private key.
          Add Hardware wallet

Top left drop down
      Add a custom network
      Network name is Hardhat
      For Hardhat, set Default RPC URL to: http://127.0.0.1:8545 and chain
      ID to 31337. The chain ID is used to distinguish between chains, is included
      in signatures, and provides replay protection. The currency symbol is ETH.
      If MetaMask complains about the network name or the chain ID or the currency symbol, just ignore the complaints and try to establish a network connection to Hardhat.
          General
             Currency conversion
             Primary currency
          Select a Network
             In order to add Harhat:
             Add a Network/Add a network manually  

          Security and Privacy
             Reveal secret recovery phrase (SRP)
             SRP is deterministic and may be used to generate many key pairs (without randomness).

          Show test networks
             Turn on when using Hardhat network for testing

```
Like Bitcoin, Ethereum can be used as a simple payment network. In this part of the lab,
we get some exposure to using MetaMask for sending ETH from one account (we are in
possession of the sender's private key for signing) to another account (with the address of the receiver).

10. Using MetaMask, add a network manually. We want to select the Hardhat Network.
You will need to point to the location where you started the server (http://127.0.0.1:8545)
and will need to set the chain ID to 31337.

11. From the shell where you started the server, copy the private key of the
first account to the clipboard and import this key into MetaMask. Name this new
account "Alice".

12. From the shell where you started the server, copy the private key of the
second account to the clipboard and import this key into MetaMask. Name this
new account "Bob". Alice and Bob should each show 10,000 ETH. These account
balances are provided by Hardhat.

E0. From Alice's account, send 20 ETH to BOB. You will need Bob's public key.
Copy the transaction details as shown on the server's shell to your single,
well-labeled pdf document.

E1. From Bob's account, send 30 ETH to Carol. Carol's account is the third
account and we are not importing her private key into MetaMask. We need only
her public key to perform the transfer. Copy the transaction details as shown
on the server's shell to your single, well-labeled pdf document.

E2. From Alice's account, send 123 ETH to Donna. Donna's account is the fourth
account and we are not importing her private key into MetaMask. We need only
her public key to perform the transfer. Copy the transaction details as shown
on the server's shell to your single, well-labeled pdf document.

## Part B. 5 Points. Leave the server running and deploy an ERC-20 token contract and interact with it via MetaMask.

Note: In this part, we are leaving the server from Part A. running.

Ethereum goes beyond a simple payment network and provides smart contracts. Smart contracts
are Turing complete and so we must pay for any gas that is used.

MetaMask allows us to interact (in a limited way) with an ERC-20 token service.

13. Within the directory Lab3_MetaMask, create a new subdirectory named contracts. You will need to open a new shell.

Within contracts, [create this smart contract named MyAdvancedToken.sol](../../blob/master/Lab3PartB/MyAdvancedToken.sol). This contract has a fallback function to handle ETH being sent by MetaMask.

14. Aside from the fallback function, this is the same contract that we explored in Lab2.

15. Using Node Package Execute (npx), compile the code with the following command. We do
this in the directory just above the contracts directory.

Note that this command will download the appropriate compiler.

```
npx hardhat compile

```

16. Run the console.

```
npx hardhat console
```
17. Within the console, access the ethers library.

```
const { ethers } = require("hardhat");
```
18. Within the console, deploy the contract to Hardhat.
```
const Token  = await ethers.getContractFactory("MyAdvancedToken");
```

```
const token = await Token.deploy(1300,"Alice Coin","AC");
```

E3. Copy the deployment transaction from the server shell and paste it into
your single, well-labeled pdf document. The server shell is where the Ethereum
test accounts are being displayed.

19. Copy the contract address and paste it into MetaMask as a new token for Alice. You can
revisit Lab 2 to learn how to find the contract's address or you can get it from the
the Hardhat console where the server is running.

E4. In two or three lines of text, describe in your own words, how MetaMask was
able to determine the symbol (AC) associated with the contract. Copy your brief
explanation into your single, well-labeled pdf document.

20. Using the console, establish the sell price at 1 eth and the buy price at 2 eth.
There is an example of such an assignment in the contract itself. This is not a transaction
that we can do through MetaMask.

E5. Show the console commands that you used to set these prices. Copy
these commands into your single, well-labeled pdf document.

21. Alice wants to sell her coins. Currently, the contract has no
tokens. Show the console commands that can be used to give the
contract 50 Alice coins. Alice will need to mint these coins
and give them to the contract.

E6. Copy these commands into your single, well-labeled pdf document.

E7. Bob imports the Alice Coin token into his account using MetaMask. He
then sends 5 ETH to the contract in exchange for tokens. Copy this transaction
from the server shell and paste into your single, well-labeled pdf document.

22. Now that Bob has tokens, he would like to view them in MetaMask.

E8. Show a screenshot of these tokens in Bob's MetaMask wallet. Copy this
screen shot into your single, well-labeled pdf document.

23. Bob uses his MetaMask wallet to transfer 1 Alice Coin token to Donna.

E9. Copy the transfer transaction from the server shell and paste it into
your single, well-labeled pdf document.

E10. Use the console to examine Donna's Alice Coin balance. Show the command
that you use and take a screen shot showing Donna's balance as returned by
Hardhat.

E11. In one very short paragraph, explain why we are not viewing Donna's token
balance using MetaMask.

## Part C. 2 Points. DeFiLlama.com is like a Bloomberg or Yahoo Finance for the decentralized finance (DeFi) world. It provides comprehensive data and analytics on various DeFi protocols across multiple blockchains, similar to how traditional financial websites offer insights into stocks, bonds, and other financial instruments. It categorizes protocols into different types, such as decentralized exchanges (DEXs), lending platforms, and yield aggregators, akin to how traditional sites categorize financial instruments into sectors or asset classes.

24. Visit the [DefLlama API](https://defillama.com/docs/api). Using [Copilot](https://www.cmu.edu/computing/services/ai/copilot/index.html), develop a program (in JavaScript or Python) that uses the API to answer some interesting question. In your well labelled PDF, include the following three parts:

E12. Provide the question that your program finds the answer to. Feel free to consult with Copilot when developing the question. It must be a question that can be answered by using the DeFiLlama API.

E13. Show the program (in JavaScript or Python) that answers the question by making call(s) on the Defi Llama API. Feel free to use Copilot to help create the code.

E14. Show the output of the program when it executes. This program may or may not be interactive.

## Grading rubric for Lab 3

One pdf file named Lab3.pdf will be submitted on Canvas for grading.

+ 3 points for successful completion of Part A
+ 5 points for successful completion of Part B
+ 2 points for successful completion of Part C

Penalty of the labelling on the PDF is not clear
==========================
 + -1/2 point

Penalty for any late  work
==========================
+ -1/2 points per day late
