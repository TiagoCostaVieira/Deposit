# Bank Contract

## Overview

The `Bank` contract is a simple smart contract written in Solidity, designed to facilitate deposits and withdrawals of Ether. It allows users to deposit and withdraw Ether, while keeping track of individual balances. The contract is equipped with events for deposit and withdrawal actions, providing transparency for these transactions.

## Features

- **Deposit Ether**: Users can deposit Ether into the contract, which is tracked against their address.
- **Withdraw Ether**: Users can withdraw their deposited Ether.
- **Check Balance**: Users can check their own balance or the total balance of the contract.
- **Event Logging**: Deposits and withdrawals are logged using events for transparency.

## Contract Code

```solidity
// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v5.0.0) (token/ERC721/ERC721.sol)

pragma solidity ^0.8.20;

contract Bank {
    event Deposited(address indexed payee, uint256 weiAmount);
    event Withdrawn(address indexed payee, uint256 weiAmount);

    mapping(address => uint256) private _deposits;

    address owner;

    constructor() {
        owner = msg.sender;
    }

    function deposit() public payable {
        uint256 amount = msg.value;
        address payee = msg.sender;

        _deposits[payee] += amount;

        emit Deposited(payee, amount);
    }

    function withdraw() public {
        address payable payee = payable(msg.sender);
        uint256 payment = _deposits[payee];

        _deposits[payee] = 0;

        payee.transfer(payment);

        emit Withdrawn(payee, payment);
    }

    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }

    function depositOf(address payee) public view returns (uint256) {
        return _deposits[payee];
    }
}
