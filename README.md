# Intents Sports Ball Workshop

![Sporty losing it all](./assets/sporty-not-my-team.gif "Sporty Losing It All [2012]")

## Table of Contents
- [Background](#background)
  - [What Are Intents?](#what-are-intents)
  - [Goals](#goals)
- [What Are Phat Functions](#what-are-phat-functions)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
  - [Environment Variables](#environment-variables)
- [Deployment](#deployment)
  - [Deploy to Polygon Mumbai Testnet](#deploy-to-polygon-mumbai-testnet)
    - [Verify Contract on Polygon Mumabai Testnet](#verify-contract-on-polygon-mumbai-testnet)
    - [Interact with Consumer Contract on Polygon Mumbai](#interact-with-consumer-contract-on-polygon-mumbai)
  - [Deploy to Polygon Mainnet](#deploy-to-polygon-mainnet)
    - [Verify Contract on Polygon Mainnet](#verify-contract-on-polygon-mainnet)
    - [Interact with Consumer Contract on Polygon Mainnet](#interact-with-consumer-contract-on-polygon-mainnet)
- [Closing](#closing)

## Background
Intents Sports Ball is a Sportsbook for betting on outcomes for sporting events like baseball, football, basketball, etc. However, the origins start with our creator named Sporty. Sporty can be seen above in the GIF dating back to 2012 where he lost everything betting his life savings on his team to win the sports game. In Sporty's words, "Daniels, why'd you throw it away Daniels! Daniels had a second leg to knee and he didn't even throw it in." This troubling event led to Sporty realizing he could make good with the Sharks if he could regain his losses by opening his own sportsbook.

Sporty had recently been reading about Intents where a phrase stuck out, "A transaction specifies the journey, while an intent specifies the destination." A light shined from the clouds and Sporty knew that all he needed to do was slap the word "Intents" on the product with the meta-intent being that all winnings go to Sporty's wallet as the destination. Sporty had perfected intents in his mind, and he left breadcrumbs of how he built the multi-trillion dollar dApp that made him the richest degen on plant Earth.
### What Are Intents?
> "An intent is a signed set of declarative constraints which allow a user to outsource transaction creation to a third party without relinquishing full control to the transacting party." - [source](https://www.paradigm.xyz/2023/06/intents)

Intents Sports Ball is not a guide on how to build intents into your dApp, and all information around intents-based architecture in blockchain can be found in Banana SDK's [awesome-intents](https://github.com/Banana-Wallet/awesome-intents) repo.

### Goals
Goals will be to teach developers different ways to build a cross-chain dApp with Phala Network's [Phat Functions](todo-link). 

Goals include the following:
- Implement Nestable NFTs for a User Profile NFT with nested NFTs of a user's bets using [RMRK Nestable NFTs (ERC-6059)](https://eips.ethereum.org/EIPS/eip-6059).
- Update a [Telegram](https://telegram.org/) Group of latest info about sports game scores.
- Query multiple API Endpoints to get sportsbook odds and latest results of the games bet on.
- Connect to [off-chain storage](todo-link) to publish a user bet's metadata.
- Connect to an EVM Chain consumer contract and start making requests to the deployed Phat Function on Phala.

## What Are Phat Functions?

## Prerequisites
- Active deployed LensAPI Oracle Blueprint via [Phat Bricks](https://bricks.phala.network)
- Address of the "[Oracle Endpoint](https://docs.phala.network/developers/bricks-and-blueprints/featured-blueprints/lensapi-oracle#step-3-connect-your-smart-contract-to-the-oracle)" in deployed LensAPI Oracle
- [Hardhat](https://hardhat.org)
- For Polygon Mainnet deployments:
  - Polygonscan API Key that can be generated on [polygonscan](https://polygonscan.com)
- RPC Endpoint for Polygon Mainnet & Mumbai Testnet
  - [Alchemy](https://alchemy.com) - This repo example uses Alchemy's API Key.
  - [Infura](https://infura.io)
  - Personal RPC Node

## Getting Started
First you will need to run `cp .env.local .env` to copy over the local environment variables.
### Environment Variables:
- `MUMBAI_RPC_URL` - JSON-RPC URL with an API key for RPC endpoints on Polygon Mumbai Testnet (e.g. [Alchemy](https://alchemy.com) `https://polygon-mumbai.g.alchemy.com/v2/<api-key>`, [Infura](https://infura.io) `https://polygon.infura.io/v3/<api-key>`).
- `POLYGON_RPC_URL` - JSON-RPC URL with an API key for RPC endpoints on Polygon Mainnet (e.g. [Alchemy](https://alchemy.com) `https://polygon.g.alchemy.com/v2/<api-key>`, [Infura](https://infura.io) `https://polygon.infura.io/v3/<api-key>`).
- `DEPLOYER_PRIVATE_KEY` - Secret key for the deployer account that will deploy the Consumer Contract on either Polygon Mainnet or Polygon Mumbai Testnet.
- `POLYGONSCAN_API_KEY` - Polygonscan API Key that can be generated at [polygonscan](https://polygonscan.com).
- `MUMBAI_LENSAPI_ORACLE_ENDPOINT` - LensAPI Oracle Endpoint Address that can be found in the dashboard of the deployed LensAPI Oracle Blueprint at [Phala PoC5 Testnet](https://bricks-poc5.phala.network) for Polygon Mumbai Testnet.
- `POLYGON_LENSAPI_ORACLE_ENDPOINT` - LensAPI Oracle Endpoint Address that can be found in the dashboard of the deployed LensAPI Oracle Blueprint at [Phala Mainnet](https://bricks.phala.network) for Polygon Mainnet.

## Deployment
Now that you have the prerequisites to deploy a Polygon Consumer Contract on Polygon, lets begin with some initials tasks.
### Install Dependencies & Compile Contracts
```shell
# install dependencies
$ yarn

# compile contracts
$ yarn compile
```
### Deploy to Polygon Mumbai Testnet
With the contracts successfully compiled, now we can begin deploying first to Polygon Mumbai Testnet. If you have not gotten `MATIC` for Mumbai Testnet then get `MATIC` from a [faucet](https://mumbaifaucet.com/).
Ensure to save the address after deploying the Consumer Contract because this address will be use in the "[Configure Client](https://docs.phala.network/developers/bricks-and-blueprints/featured-blueprints/lensapi-oracle#step-4-configure-the-client-address)" section of Phat Bricks UI. The deployed address will also be set to the environment variable [`MUMBAI_CONSUMER_CONTRACT_ADDRESS`](./.env.local).
```shell
# deploy contracts to testnet mumbai
$ yarn test-deploy
# Deployed { consumer: '0x93891cb936B62806300aC687e12d112813b483C1' }

# Check our example deployment in <https://mumbai.polygonscan.com/address/0x93891cb936B62806300aC687e12d112813b483C1>
```
#### Verify Contract on Polygon Mumbai Testnet
Ensure to update the [`mumbai.arguments.ts`](./mumbai.arguments.ts) file with the constructor arguments used to instantiate the Consumer Contract. If you add additional parameters to the constructor function then make sure to update the `mumbai.arguments.ts` file.
> **Note**: Your contract address will be different than `0x93891cb936B62806300aC687e12d112813b483C1` when verifying your contract. Make sure to get your actual contract address from the console log output after executing `yarn test-deploy`. 
```shell
$ yarn test-verify 0x93891cb936B62806300aC687e12d112813b483C1
Nothing to compile
No need to generate any newer typings.
Successfully submitted source code for contract
contracts/TestLensApiConsumerContract.sol.sol.sol:TestLensApiConsumerContract.sol at 0x93891cb936B62806300aC687e12d112813b483C1
for verification on the block explorer. Waiting for verification result...

Successfully verified contract TestLensApiConsumerContract.sol on Etherscan.
https://mumbai.polygonscan.com/address/0x93891cb936B62806300aC687e12d112813b483C1#code
Done in 8.88s.
```
#### Interact with Consumer Contract on Polygon Mumbai
Test Consumer Contract on Mumbai with a few tests to check for malformed requests failures, successful requests, and set the attestor.
```shell
# set the attestor to the Oracle Endpoint in Phat Bricks UI
$ yarn test-set-attestor
Setting attestor...
🚨NOTE🚨
Make sure to go to your Phat Bricks 🧱 UI dashboard (https://bricks-poc5.phala.network)
- Go to 'Configure Client' section where a text box reads 'Add Consumer Smart Contract'
- Set value to 0x93891cb936B62806300aC687e12d112813b483C1
Done
✨  Done in 1.56s.
# execute push-malformed-request
$ yarn test-push-malformed-request
Pushing a malformed request...
Done
# execute push-request
$ yarn test-push-request
Pushing a request...
Done
```

### Deploy to Polygon Mainnet
Ensure to save the address after deploying the Consumer Contract because this address will be used in the "[Configure Client](https://docs.phala.network/developers/bricks-and-blueprints/featured-blueprints/lensapi-oracle#step-4-configure-the-client-address)" section of Phat Bricks UI. The deployed address will also be set to the environment variable [`POLYGON_CONSUMER_CONTRACT_ADDRESS`](./.env.local).
> **Note**: Your contract address will be different than `0xbb0d733BDBe151dae3cEf8D7D63cBF74cCbf04C4` when verifying your contract. Make sure to get your actual contract address from the console log output after executing `yarn main-deploy`.
```shell
# deploy contracts to polygon mainnet
$ yarn main-deploy
Deploying...
Deployed { consumer: '0xbb0d733BDBe151dae3cEf8D7D63cBF74cCbf04C4' }
Configuring...
Done

# Check our example deployment in <https://polygonscan.com/address/0xbb0d733BDBe151dae3cEf8D7D63cBF74cCbf04C4>
```
#### Verify Contract on Polygon Mainnet
Ensure to update the [`polygon.arguments.ts`](./polygon.arguments.ts) file with the constructor arguments used to instantiate the Consumer Contract. If you add additional parameters to the constructor function then make sure to update the `polygon.arguments.ts` file.
```shell
$ yarn main-verify 0xbb0d733BDBe151dae3cEf8D7D63cBF74cCbf04C4
Nothing to compile
No need to generate any newer typings.
Successfully submitted source code for contract
contracts/TestLensApiConsumerContract.sol.sol:TestLensApiConsumerContract.sol.sol at 0xbb0d733BDBe151dae3cEf8D7D63cBF74cCbf04C4
for verification on the block explorer. Waiting for verification result...

Successfully verified contract TestLensApiConsumerContract.sol on Etherscan.
https://polygonscan.com/address/0xbb0d733BDBe151dae3cEf8D7D63cBF74cCbf04C4#code
Done in 8.88s.
```

#### Interact with Consumer Contract on Polygon Mainnet
Execute Scripts to Consumer Contract on Polygon Mainnet. The Consumer Contract on Polygon Mainnet with a few actions to mimic a malformed request, successful requests, and set the attestor.
```shell
# set the attestor to the Oracle Endpoint in Phat Bricks UI
$ yarn main-set-attestor
Setting attestor...
🚨NOTE🚨
Make sure to set the Consumer Contract Address in your Phat Bricks 🧱 UI dashboard (https://bricks-poc5.phala.network)
- Go to 'Configure Client' section where a text box reads 'Add Consumer Smart Contract'
- Set value to 0xbb0d733BDBe151dae3cEf8D7D63cBF74cCbf04C4
Done
✨  Done in 1.56s.
# execute push-malformed-request
$ yarn main-push-malformed-request
Pushing a malformed request...
Done
# execute push-request
$ yarn main-push-request
Pushing a request...
Done
```

## Closing
Once you have stored, the deployed address of the Consumer Contract and set the value in the "Configure Client" section of the deployed LensAPI Oracle, you will now have a basic boilerplate example of how to connect your Polygon dApp to a LensAPI Oracle Blueprint. Execute a new requests and check if your configuration is correct like below:
![](./assets/polygonscan-ex.png)
