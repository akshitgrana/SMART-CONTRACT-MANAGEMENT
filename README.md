# Smart Contract Management

This repository contains a simple Ethereum smart contract and a frontend interface for managing bank-like functionalities on the blockchain.

## Table of Contents

- [Overview](#overview)
- [Application Preview](#application-preview)
- [Smart Contract](#smart-contract)
  - [Bank.sol](#banksol)
  - [Description](#description)
  - [Functions](#functions)
- [Frontend](#frontend)
  - [index.html](#indexhtml)
  - [Description](#description-1)
  - [Features](#features)
- [Technologies Used](#technologies-used)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Usage](#usage)
- [Contributing](#contributing)

## Overview

This project showcases a basic implementation of a decentralized bank using Ethereum smart contracts. Users can interact with the smart contract to set account names, check balances, and transfer funds. The frontend provides a user-friendly interface for these operations.

### Application Preview 

![image](https://github.com/user-attachments/assets/fbb45861-66b0-497e-83a5-135908071d62)


![image](https://github.com/user-attachments/assets/689b3705-340c-4fcf-8de0-28b5319e2daa)




## Smart Contract

### Bank.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Bank {
    mapping(address => string) accountNames;

    event Transfer(uint256 amount);
    event NameUpdate(string value);

    function setAccountName(string memory _name) public {
        require(
            keccak256(abi.encodePacked(accountNames[msg.sender])) !=
                keccak256(abi.encodePacked(_name)),
            "New name is same as old name"
        );
        accountNames[msg.sender] = _name;

        emit NameUpdate(accountNames[msg.sender]);
    }

    function getAccountName() public view returns (string memory) {
        return accountNames[msg.sender];
    }

    error InsufficientBalance(uint256 balance, uint256 withdrawAmount);

    function transferFunds(address payable _to) public payable {
        _to.transfer(msg.value);
        if (msg.sender.balance < msg.value)
            revert
                InsufficientBalance({
                    balance: msg.sender.balance,
                    withdrawAmount: msg.value
                });
        emit Transfer(msg.value);
    }

    function getBalance() public view returns (uint256) {
        return msg.sender.balance;
    }
}
```

### Description

The `Bank` smart contract allows users to set their account names, transfer funds to other addresses, and check their account balances on the Ethereum blockchain.

### Functions

- `setAccountName`: Allows users to set their account names.
- `getAccountName`: Retrieves the account name of the caller.
- `transferFunds`: Transfers Ether to another address.
- `getBalance`: Retrieves the Ether balance of the caller.

## Frontend

### index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Metacrafters Local Bank</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="./public/css/styles.css">
    <link rel="icon" href="./public/images/favicon.jpg" type="image/x-icon"/>
</head>
<body>
    <!-- Your HTML content here -->
</body>
</html>
```

### Description

`index.html` provides a frontend interface built with Bootstrap and JavaScript to interact with the `Bank` smart contract deployed on Ethereum.

### Features

- Connect wallet to interact with the blockchain.
- Display account name, address, and balance.
- Change account name and transfer funds functionalities.

## Technologies Used

- **Solidity**: Programming language for writing smart contracts.
- **Web3.js**: Library for interacting with Ethereum blockchain.
- **Bootstrap**: CSS framework for frontend design.
- **Ethers.js**: JavaScript library for Ethereum interaction.

## Getting Started

To run this project locally or deploy it:

### Prerequisites

- Install MetaMask or any Ethereum-compatible wallet extension in your browser.
- Connect to an Ethereum network (Mainnet, Ropsten, Rinkeby, etc.) using MetaMask.

### Installation

1. Clone this repository:
   ```bash
   https://github.com/akshitgrana/SMART-CONTRACT-MANAGEMENTt
   ```
2. Deploy `Bank.sol` on an Ethereum-compatible blockchain (e.g., Remix IDE, Truffle).
3. Update `index.html` with the deployed smart contract address.

**Follow these steps to get the project up and running**
  1. Download or clone the project.
  2. Install the dependencies by running `npm install`.
  3. Start the local blockchain using Hardhat by running `npx hardhat node`.
  4. Open new terminal and deploy the Bank contract `npx hardhat run --network localhost scripts/deploy.js`
  5. Start the development server by running `npm run dev`.

**Configure MetaMask to use the Hardhat node**
  1. Open the MetaMask extension in your browser.
  2. Click on the account icon in the top right corner and select "Settings".
  3. In the "Networks" tab, click on "Add Network".
  4. Fill in the following details:
     1. Network Name: hardhat-test-network
     2. RPC URL: http://127.0.0.1:8545/
     3. Chain ID: 31337
     4. Currency Symbol: ETH
  6. Click on "Save" to add the Hardhat network to MetaMask.

**Add accounts using private keys by Hardhat for testing**
  1. In the MetaMask extension, click on the account icon in the top right corner.
  2. Select "Import Account" or "Import Account using Private Key" (depending on your version of MetaMask).
  3. In the "Private Key" field, enter one of the private keys provided by Hardhat.
     1. To access the list of private keys, open the terminal where you started the Hardhat local network.
     2. The private keys are displayed as part of the accounts generated by Hardhat.
  5. Click on "Import" to import the account into MetaMask.
  6. Repeat the above steps to add more accounts for testing purposes.
     
## Usage

1. Open `index.html` in a web browser with MetaMask connected to your preferred Ethereum network.
2. Connect your wallet and interact with the provided functionalities.

## Contributing

Contributions are welcome! Please fork this repository, make your changes, and submit a pull request. For major changes, please open an issue first to discuss what you would like to change.
