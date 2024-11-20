> [!IMPORTANT]  
> This repo is for demo purposes only. 

# Account Abstraction

- [Account Abstraction](#account-abstraction)
  - [What is Account Abstraction?](#what-is-account-abstraction)
  - [What's this repo show?](#whats-this-repo-show)
  - [What does this repo not show?](#what-does-this-repo-not-show)
- [Getting Started](#getting-started)
  - [Requirements](#requirements)
  - [Installation](#installation)
  - [Description](#description)
- [Quickstart](#quickstart)
  - [Running Tests](#running-tests)

- [FAQ](#faq)
  - [What if I don't add the contract hash to factory deps?](#what-if-i-dont-add-the-contract-hash-to-factory-deps)
  - [Why can't we do these deployments with foundry or cast?](#why-cant-we-do-these-deployments-with-foundry-or-cast)
  - [Why can I use `forge create --legacy` to deploy a regular contract?](#why-can-i-use-forge-create---legacy-to-deploy-a-regular-contract)
- [Acknowledgements](#acknowledgements)
- [Disclaimer](#disclaimer)

## What is Account Abstraction?

EoAs are now smart contracts. That's all account abstraction is.

But what does that mean?

Right now, every single transaction in web3 stems from a single private key. 

Account abstraction means that not only the execution of a transaction can be arbitrarily complex computation logic as specified by the EVM, but also the authorization logic.

- [Vitalik Buterin](https://ethereum-magicians.org/t/implementing-account-abstraction-as-part-of-eth1-x/4020)
- [EntryPoint Contract v0.6](https://etherscan.io/address/0x5ff137d4b0fdcd49dca30c7cf57e578a026d2789)
- [EntryPoint Contract v0.7](https://etherscan.io/address/0x0000000071727De22E5E9d8BAf0edAc6f37da032)
- [zkSync AA Transaction Flow](https://docs.zksync.io/build/developer-reference/account-abstraction.html#the-transaction-flow)

## What's this repo show?

1. A minimal EVM "Smart Wallet" using alt-mempool AA
   1. We even send a transactoin to the `EntryPoint.sol`
2. A minimal zkSync "Smart Wallet" using native AA
   1. [zkSync uses native AA, which is slightly different than ERC-4337](https://docs.zksync.io/build/developer-reference/account-abstraction.html#iaccount-interface)
   2. We *do* send our zkSync transaction to the alt-mempool

## What does this repo not show?

1. Sending your userop to the alt-mempool 
   1. You can learn how to do this via the [alchemy docs](https://alchemy.com/?a=673c802981)

# Getting Started 

## Requirements

- [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
  - You'll know you did it right if you can run `git --version` and you see a response like `git version x.x.x`
- [foundry](https://getfoundry.sh/)
  - You'll know you did it right if you can run `forge --version` and you see a response like `forge 0.2.0 (816e00b 2023-03-16T00:05:26.396218Z)`
- [foundry-zksync](https://github.com/matter-labs/foundry-zksync)
  - You'll know you did it right if you can run `forge-zksync --help` and you see `zksync` somewhere in the output

## Installation

```bash
git clone https://github.com/MihaiHng/foundry-account-abstraction-f24.git
cd foundry-account-abstraction-f24.git
make
```

## Description

The smart contracts in this codebase can be deployed and Acount Abstraction transactions can be sent, however, they will need dedicated deployment scripts, which are not provided in this repo.

# Quickstart 

## Running Tests

The created tests can be run to check the functionality of the Acount Abstraction powered smart contracts.

```bash
forge test or 
forge test --mt "test name" or
make test 

```
# FAQ

## What if I don't add the contract hash to factory deps? 
The transaction will revert. The `ContractDeployer` checks to see if it knows the hash, and if not, it will revert! The `ContractDeployer` calls the `KnownCodesStorage` contract, which keeps track of *every single contract hash deployed on the zkSync chain. Crazy right!*

## Why can't we do these deployments with foundry or cast? 
Foundry and cast don't have support for the `factoryDeps` transaction field, or support for type `113` transactions. 

## Why can I use `forge create --legacy` to deploy a regular contract?
`foundry-zksync` is smart enough to see a legacy deployment (when you send a transaction to the 0 address with data) and transform it into a contract call to the deployer. It's only smart enough for legacy deployments as of today, not the new `EIP-1559` type 2 transactions or account creation.

# Acknowledgements 
- [Types of AAs on different chains](https://www.bundlebear.com/factories/all)
- [eth-infinitism](https://github.com/eth-infinitism/account-abstraction/)
- [Dan Nolan](https://www.youtube.com/watch?v=b4KWkIAPa3U)
  - [Twitter Video](https://x.com/BeingDanNolan/status/1795848790043218029)
- [zerodevapp](https://github.com/zerodevapp/kernel/)
- [Alchemy LightAccount](https://github.com/alchemyplatform/light-account/)

# Disclaimer
*This codebase is for educational purposes only and has not undergone a security review.*