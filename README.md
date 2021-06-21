# InitialTwitterOffering

## Introduction

Initial Twitter Offering (ITO) is a Dapplet based on the Mask broswer extension. It consists of two components, the browser extension and the Ethereum smart contract. This repo covers the design and the implementation of the smart contract only.

## Overview

ITO is basically a token swap pool that can be initiated by any Ethereum user. Users can transfer an amount of a target token (now supporting ETH and ERC20) into the pool and set the swap ratios, e.g. {1 ETH: 10000 TOKEN, 1 DAI: 10 TOKEN}. Users can also set the swap limit (ceiling) to control how many tokens to be swapped by a single address, e.g. 10000 TOKEN. After the pool is expired (also set on initiation) or the target token is out of stock, the pool creator can withdraw any target token left and all the swapped tokens. The pool will be destructed after the withdrawl.

Participants only need to approve one of the specified token according to the pool ratio and call the `swap()` function to swap the target token out. The swapped amount is automatically calculated by the smart contract and the users can received the target token instantly after a successful call of the `swap()`. Pool creator can also set an unlock time for ITO which means to receive the target token participants need to wait after that unlock time by calling `claim()` function.

## Getting Started

This is a standard truffle project.
To install:
```
npm i
```
To build the project:
```
npm run compile
```

To test the project:
```
npm test
```

To debug:
```solidity
//...
import "hardhat/console.sol";

function debug_param (address _token_addr) public {
    console.log('_token_addr', _token_addr);
}
```

## Contract Address

### ITO Contract

| Contract | [Mainnet](https://etherscan.io/) | [Ropsten](https://ropsten.etherscan.io/) | [BSC](https://bscscan.com/) |[BSC-testnet](https://testnet.bscscan.com/) | [Matic](https://matic.network/) | [Matic-mumbai](https://explorer-mumbai.maticvigil.com/) |
|---|---|---|---|---|---|---|
| [ITO](contracts/ito.sol) | [0xxxxxxxx](https://etherscan.io/address/0xxxxxxxx) | [0x6a240d07](https://ropsten.etherscan.io/address/0x6a240d07b1623080977487b45d340A0b6d221e93) | [0xxxxxxxx](https://bscscan.com/address/0xxxxxxxx) | [0x9Dc4cb68](https://testnet.bscscan.com/address/0x9Dc4cb6866c3DdCb66c6437c6d6FE0C0E1585229) | [0xxxxxxxx](https://polygonscan.com/address/0xxxxxxxx) | [0x5EbCEf0B](https://explorer-mumbai.maticvigil.com/address/0x5EbCEf0Bc8d51eBc5adEf6072Dd03C3aFCa7ACAB) |


## Qualification

Another smart contract interface `IQLF` ([source code](https://github.com/DimensionDev/InitialTwitterOffering/blob/master/contracts/IQLF.sol)) is also introduced to provide an API `ifQualified()` that takes an address as input and returns a boolean indicating if the given address is qualified. Custom qualification contract **SHOULD** implement contract `IQLF` rather than ERC-165 to further ensure required interface is implemented, since `IQLF` is compliant with ERC-165 and has implemented the details of `supportsInterface` function.

To prevent malicious attack, you can set a `swap_start_time` in your custom qualification contract, then add accounts who swap before that time to a black list, they will no longer be able to access your ITO. Please confirm the `swap_start_time` carefully, it must be less than the end time of ITO, otherwise nobody can access your ITO at all. To let Mask broswer extension to help you check the if `swap_start_time` is less than the end time of ITO. You need to append `interfaceId == this.get_start_time.selector;` to `supportsInterface()`(Notice the getter function **MUST** be named `get_start_time()` to keep the same with the broswer extension code), just copy the implemetation of [our default qualification contract](https://github.com/DimensionDev/InitialTwitterOffering/blob/master/contracts/qualification.sol).

### Empty Qualification Contract

| Contract | [Mainnet](https://etherscan.io/) | [Ropsten](https://ropsten.etherscan.io/) | [BSC](https://bscscan.com/) |[BSC-testnet](https://testnet.bscscan.com/) | [Matic](https://matic.network/) | [Matic-mumbai](https://explorer-mumbai.maticvigil.com/) |
|---|---|---|---|---|---|---|
| [qualification](contracts/qualification.sol) | [0xxxxxxxx](https://etherscan.io/address/0xxxxxxxx) | [0x9Dc4cb68](https://ropsten.etherscan.io/address/0x9Dc4cb6866c3DdCb66c6437c6d6FE0C0E1585229) | [0xxxxxxxx](https://bscscan.com/address/0xxxxxxxx) | [0x8824c33E](https://testnet.bscscan.com/address/0x8824c33Ed0603c5Cfd09726b7ef389c53E8ADc77) | [0xxxxxxxx](https://polygonscan.com/address/0xxxxxxxx) | [0x02923992](https://explorer-mumbai.maticvigil.com/address/0x029239920369C6866582F121F631022fb9F61202) |



## Contribute

Any contribution is welcomed to make it more secure and powerful. Had you any questions, please do not hesitate to create an issue to let us know.

## License
InitialTwitterOffering is released under the [MIT LICENSE](LICENSE).
