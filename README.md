# Bancor Compound Pool

## Description

This project aims to implement a new Bancor pool design that interacts with compound platform automatically, when deployed the pool will hold only compound tokens internally (other tokens can be added also), however, when the compound token is added the underlying token has to be specified.

When funding the pool (converter), liquidity providers will be able to use either cToken or its underlying equivalent. The converter contract will automatically mint the cTokens, if the underlying token is deposited, otherwise the token is directly added to the pool.

The same goes for users converting tokens, even if the pool contains only cToken, they will be able to choose which token to receive or to deposit (cToken or its underlying token) just by changing the conversion path.

Other than the possibility of converting from/to any token, the liquidity providers will benefit from compound earnings since all deposited underlying token will be sent to Compound.

## Setup

### Dependencies

* node v10.18.0
* npm 6.13.4
* truffle
* ganache-cli

### Installation

```console
$ git clone https://github.com/ridesolo/BancorCompoundPool
$ cd BancorCompoundPool
$ git submodule update --init
$ npm install
```
### Verification

```console
$ npm run test 
```
Please note that the values generated by the eth-gas-reporter are not accurate since the cToken contract behavior was emulated using a mock contract (extra gas will be consumed by compound contract platform).

## Deployment

Deployment can be performed automatically, the script can deploy only dual token pool (cToken/BNT), please note that the underlying token has also to be specified when creating the pool. Pool with multiple reserve tokens (>2) can also be created, however the deployment script does not handle it for now.

### Options:

* `--bancorContractRegistry`: The address of Bancor Contract Registry deployed in the chosen network.
* `--uToken`: Underlying token address of the chosen deployment network.
* `--cToken`: Compound token address of the chosen deployment network.
* `--bntReserveBalance`: BNT token balance to be deposited as inital reserve (do not forget the decimals).
* `--bntReserveRatio`: BNT token ratio (the cumulative value of both ratios should be less than 1000000).
* `--underlyingReserveBalance`: Underlying token balance to be deposited as inital reserve (do not forget the decimals).
* `--underlyingReserveRatio`: The ratio of the underlying asset (same as the compound token, since they represent a single asset internally).
* `--conversionFee`: Pool conversion fee.
* `--initialSmarTokenBalance`: Initial smart token balance (don't forget the decimals, set to 18 by default).
* `--privateKey`: Wallet private key to the account holding the necessary balances for the deployment.
* `--httpProvider`: Http provider URL.

The example below shows how to deploy the converter to the live ethreum network using DAI/cDAI and BNT. Please note that the provided addresses can be subject to change by Bancor or Compound.

```console
truffle migrate --reset --network external  \
--bancorContractRegistry "0x52Ae12ABe5D8BD778BD5397F99cA900624CfADD4" \
--uToken "0x6b175474e89094c44da98b954eedeac495271d0f" \
--cToken "0x5d3a536e4d6dbd6114cc1ead35777bab948e3643" \
--bntReserveBalance "1000000000000000000" \
--bntReserveRatio "500000" \
--underlyingReserveBalance "1000000000000000000" \
--underlyingReserveRatio "500000" \
--conversionFee "30000" \
--initialSmarTokenBalance "2000000000000000000" \
--privateKey "Set the private key to the account holding the necessary balances" \
--httpProvider "https://cloudflare-eth.com"
```
## Example 

A test example was deployed in Ropsten network, under the following addresses:

* Converter address: [0x5C7fF8e8866C4Cff86c8A2b0D40c5A49259Ca314](https://ropsten.etherscan.io/address/0x5C7fF8e8866C4Cff86c8A2b0D40c5A49259Ca314)
* Smart Token address: [0xe577C01B95B74deDf20a1E5F53Cf8F78b9EbCAF3](https://ropsten.etherscan.io/address/0xe577C01B95B74deDf20a1E5F53Cf8F78b9EbCAF3)

Path allowed by the converter and successful transactions example, are listed below:

- [BNT,SMARTTOKEN,DAI]:(https://ropsten.etherscan.io/tx/0x358110ffa5babe668db6bc74f35d188c0e06b0ab87fc06185aadd71018b3c21d)
- [BNT,SMARTTOKEN,cDAI](https://ropsten.etherscan.io/tx/0xa2088c24e87cb2a8253e7c2adf0dee1e8abed16e3b3b5576d4e3d5a511d7d207)
- [BNT,SMARTTOKEN,SMARTTOKEN](https://ropsten.etherscan.io/tx/0xa4fec89f4d0d5564057f34cd7c99b29bdf8bdcd758dc826196cd90ac62c8b76b)

- [DAI,SMARTTOKEN,BNT](https://ropsten.etherscan.io/tx/0x75628f253b100a8809dc59676a6dbdc9183cf030cb4d0e40ef7998aa7f502d2d)
- [DAI,SMARTTOKEN,cDAI](https://ropsten.etherscan.io/tx/0x0202601bb2a3c20248e46cd6fb314ae283569354462102a0f6b5f83b466c4047)
- [DAI,SMARTTOKEN,SMARTTOKEN](https://ropsten.etherscan.io/tx/0xc9e494903b4f5ac3e245a9fcff1bbc05c39cff2114f557d344ea2a336b82afa1)

- [cDAI,SMARTTOKEN,BNT](https://ropsten.etherscan.io/tx/0x8c8f41610d77cd6912678e0424afa2e615dcaa60565822217dc673d7f5376587)
- [cDAI,SMARTTOKEN,DAI](https://ropsten.etherscan.io/tx/0x0f7ff181735c5b4523a4a5fe647296547bb7965636420e57e13b8160268ec2c9)
- [CDAI,SMARTTOKEN,SMARTTOKEN](https://ropsten.etherscan.io/tx/0x8ea74f4ec7f5d16927ff0b9d9c6ea7cda91106f7ad56273aa9a80f361ef3932f)

- [SMARTTOKEN,SMARTTOKEN,BNT](https://ropsten.etherscan.io/tx/0x30f40e9032716120061b6abbf17cb239e26766eceec2a72b1e55ce7dbd0b6552)
- [SMARTTOKEN,SMARTTOKEN,DAI](https://ropsten.etherscan.io/tx/0x19b284d8a62e8a3aa4d3f21a2bcd7ea8ee2e4dc60d474943d820def25a5780a9)
- [SMARTTOKEN,SMARTTOKEN,cDAI](https://ropsten.etherscan.io/tx/0xa925d275515a9da769203641be5044aa4c942d83418bcef1f582636858bf418f)

Token addresses:

- Smart Token: [0xe577C01B95B74deDf20a1E5F53Cf8F78b9EbCAF3](https://ropsten.etherscan.io/address/0xe577C01B95B74deDf20a1E5F53Cf8F78b9EbCAF3)
- cDai: [0x6CE27497A64fFFb5517AA4aeE908b1E7EB63B9fF](https://ropsten.etherscan.io/address/0x6CE27497A64fFFb5517AA4aeE908b1E7EB63B9fF)
- DAI: [0xB5E5D0F8C0cbA267CD3D7035d6AdC8eBA7Df7Cdd](https://ropsten.etherscan.io/address/0xB5E5D0F8C0cbA267CD3D7035d6AdC8eBA7Df7Cdd)
- BNT: [0x62bd9D98d4E188e281D7B78e29334969bbE1053c](https://ropsten.etherscan.io/address/0x62bd9D98d4E188e281D7B78e29334969bbE1053c)
