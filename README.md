# Multichain Crypto Wallet

A Multichain crypto wallet library that supports Ethereum, Solana and other EVM compatible blockchains.

[![Build](https://img.shields.io/github/workflow/status/iamnotstatic/multichain-crypto-wallet/CI)](https://github.com/iamnotstatic/multichain-crypto-wallet)
[![Version](https://img.shields.io/npm/v/multichain-crypto-wallet)](https://github.com/iamnotstatic/multichain-crypto-wallet)
[![GitHub issues](https://img.shields.io/github/issues/iamnotstatic/multichain-crypto-wallet)](https://github.com/iamnotstatic/multichain-crypto-wallet/issues)
[![GitHub stars](https://img.shields.io/github/stars/iamnotstatic/multichain-crypto-wallet)](https://github.com/iamnotstatic/multichain-crypto-wallet/stargazers)
[![GitHub license](https://img.shields.io/github/license/iamnotstatic/multichain-crypto-wallet)](https://github.com/iamnotstatic/multichain-crypto-wallet)
[![Total Downloads](https://img.shields.io/npm/dm/multichain-crypto-wallet)](https://github.com/iamnotstatic/multichain-crypto-wallet)

## Installation

```bash
npm install multichain-crypto-wallet
```

Using yarn,

```bash
yarn add multichain-crypto-wallet
```

## Usage

### Javascript

```javascript
const multichainWallet = require('multichain-crypto-wallet');
```

### TypeScript

```typescript
import multichainWallet from 'multichain-crypto-wallet';
```

## Methods

The following methods are available with this SDK:

- [Multichain Crypto Wallet](#multichain-crypto-wallet)
  - [Installation](#installation)
  - [Usage](#usage)
    - [Javascript](#javascript)
    - [TypeScript](#typescript)
  - [Methods](#methods)
    - [Create Wallet](#create-wallet)
      - [Response](#response)
    - [Get Balance](#get-balance)
      - [Native coins](#native-coins)
      - [Tokens](#tokens)
      - [Response](#response-1)
    - [Generate Wallet from Mnemonic](#generate-wallet-from-mnemonic)
      - [Response](#response-2)
    - [Get Address from Private Key](#get-address-from-private-key)
      - [Response](#response-3)
    - [Get Transaction](#get-transaction)
      - [Response](#response-4)
    - [Transfer](#transfer)
      - [Ethereum Network](#ethereum-network)
      - [Response](#response-5)
      - [Solana Network](#solana-network)
      - [Response](#response-9)
    - [Encryptions](#encryptions)
      - [Encrypt Private Key](#encrypt-private-key)
      - [Response](#response-6)
      - [Decrypt Encrypted JSON](#decrypt-encrypted-json)
      - [Response](#response-7)
    - [Token Info](#token-info)
      - [Get ERC20 Token Info](#get-erc20-token-info)
      - [Response](#response-8)
      - [Get SPL Token Info](#get-spl-token-info)
      - [Response](#response-10)

### Create Wallet

This method creates a new wallet. The method accepts a payload object as the parameter. The parameter of this payload is:

```javascript
// Creating an Ethereum wallet.
const wallet = await multichainWallet.createWallet({
  derivationPath: "m/44'/60'/0'/0/0", // Leave empty to use default derivation path
  network: 'ethereum',
});

// Creating a Solana wallet.
const wallet = await multichainWallet.createWallet({
  derivationPath: "m/44'/501'/0'/0'", // Leave empty to use default derivation path
  network: 'solana',
});
```

#### Response

```javascript
{
  address: '0xfBE11AC0258cc8288cA24E818691Eb062f7042E9',
  privateKey: '0xfdf745f45d1942feea79b4c0a3fc1ca67da366899f7e6cebaa06496806ca8127',
  mnemonic: 'net idle lava mango another capable inhale portion blossom fluid discover cruise'
}
```

### Get Balance

This gets the balance of the address passed in. The method accepts an object as the parameter.
The parameters for this object depeding on the kind of balance to be gotten is in the form:

#### Native coins

```javascript
// Get the ETH balance of an address
const data = await multichainWallet.getBalance({
  address: '0x2455eC6700092991Ce0782365A89d5Cd89c8Fa22',
  network: 'ethereum',
  rpcUrl: 'https://rinkeby-light.eth.linkpool.io',
});

// Get the SOL balance of an address
const data = await multichainWallet.getBalance({
  address: 'DYgLjazTY6kMqagbDMsNttRHKQj9o6pNS8D6jMjWbmP7',
  network: 'solana',
  rpcUrl: 'https://api.devnet.solana.com',
});
```

#### Tokens

```javascript
// Get the balance of an ERC20 token.
const data = await multichainWallet.getBalance({
  address: '0x2455eC6700092991Ce0782365A89d5Cd89c8Fa22',
  network: 'ethereum',
  rpcUrl: 'https://rpc.ankr.com/eth',
  tokenAddress: '0xdac17f958d2ee523a2206206994597c13d831ec7',
});

// Get the balance of a token on Solana.
const data = await multichainWallet.getBalance({
  address: '5PwN5k7hin2XxUUaXveur7jSe5qt2mkWinp1JEiv8xYu',
  tokenAddress: 'Es9vMFrzaCERmJfrF4H2FYD4KCoNkY11McCe8BenwNYB',
  network: 'solana',
  rpcUrl: 'https://rpc.ankr.com/solana',
});
```

#### Response

```javascript
{
  balance: 2;
}
```

### Generate Wallet from Mnemonic

This generates a wallet from Mnemonic phrase. The method accepts an object as the parameter. The parameters that this object takes are:

```javascript
// Generate an Ethereum wallet from mnemonic.
const wallet = await multichainWallet.generateWalletFromMnemonic({
  mnemonic:
    'candy maple cake sugar pudding cream honey rich smooth crumble sweet treat',
  derivationPath: "m/44'/60'/0'/0/0", // Leave empty to use default derivation path
  network: 'ethereum',
});

// Generate a Solana wallet from mnemonic.
const wallet = await multichainWallet.generateWalletFromMnemonic({
  mnemonic:
    'base dry mango subject neither labor portion weekend range couple right document',
  derivationPath: "m/44'/501'/0'/0'", // Leave empty to use default derivation path
  network: 'solana',
});
```

#### Response

```javascript
{
  address: '0x627306090abaB3A6e1400e9345bC60c78a8BEf57',
  privateKey: '0xc87509a1c067bbde78beb793e6fa76530b6382a4c0241e5e4a9ec0a0f44dc0d3',
  mnemonic: 'candy maple cake sugar pudding cream honey rich smooth crumble sweet treat'
}
```

### Get Address from Private Key

This gets the address from the private key passed in. The method accepts an object as the parameter. The parameters that this object takes are:

```javascript
// Get the address from the private key on the Ethereum network.
const address = await multichainWallet.getAddressFromPrivateKey({
  privateKey:
    '0f9e5c0bee6c7d06b95204ca22dea8d7f89bb04e8527a2c59e134d185d9af8ad',
  network: 'ethereum',
});

// Get the address from the private key on the Solana network.
const address = await multichainWallet.getAddressFromPrivateKey({
  privateKey:
    'bXXgTj2cgXMFAGpLHkF5GhnoNeUpmcJDsxXDhXQhQhL2BDpJumdwMGeC5Cs66stsN3GfkMH8oyHu24dnojKbtfp',
  network: 'solana',
});
```

#### Response

```javascript
{
  address: '0x1C082D1052fb44134a408651c01148aDBFcCe7Fe';
}
```

### Get Transaction

This gets the transcation receipt of a transaction from the transaction hash. The method accepts an object as the parameter. The parameters that this object takes are:

```javascript
// Get the transaction receipt on Ethereum network.
const receipt = await multichainWallet.getTransaction({
  hash: '0x5a90cea37e3a5dbee6e10190ff5a3769ad27a0c6f625458682104e26e0491055',
  network: 'ethereum',
  rpcUrl: 'https://rinkeby-light.eth.linkpool.io',
});

// Get the transaction receipt on Solana network.
const receipt = await multichainWallet.getTransaction({
  rpcUrl: 'https://api.devnet.solana.com',
  hash:
    'CkG1ynQ2vN8bmNsBUKG8ix3moUUfELWwd8K2f7mmqDd7LifFFfgyFhBux6t22AncbY4NR3PsEU3DbH7mDBMXWk7',
  network: 'solana',
});
```

#### Response

```javascript
{
  receipt: {
    object;
  }
}
```

### Transfer

This transfers the amount of tokens from the source address to the destination address It takes in an object as the parameter. It allows for the transfer of the following:

#### Ethereum Network

Allows for the transfer of ETH, and overriding of transactions.

```javascript
// Transferring ETH from one address to another.
const transfer = await multichainWallet.transfer({
  recipientAddress: '0x2455eC6700092991Ce0782365A89d5Cd89c8Fa22',
  amount: 1,
  network: 'ethereum',
  rpcUrl: 'https://rinkeby-light.eth.linkpool.io',
  privateKey:
    '0f9e5c0bee6c7d06b95204ca22dea8d7f89bb04e8527a2c59e134d185d9af8ad',
  gasPrice: '10', // Gas price is in Gwei. Leave empty to use default gas price
});

// Transferring ERC20 tokens from one address to another.
const transfer = await multichainWallet.transfer({
  recipientAddress: '0x2455eC6700092991Ce0782365A89d5Cd89c8Fa22',
  amount: 10,
  network: 'ethereum',
  rpcUrl: 'https://rinkeby-light.eth.linkpool.io',
  privateKey:
    '0f9e5c0bee6c7d06b95204ca22dea8d7f89bb04e8527a2c59e134d185d9af8ad',
  gasPrice: '10', // Gas price is in Gwei. leave empty to use default gas price
  tokenAddress: '0xdac17f958d2ee523a2206206994597c13d831ec7',
});
```

The optional parameters that the object takes in are: gas price and nonce.

- The gas price is the price of gas in Gwei. The higher the gas price, the faster the transaction will be. It's best to use a higher gas price than the default.
- The nonce is the number of transactions that have been sent from the source address and is used to ensure that the transaction is unique. The transaction is unique because the nonce is incremented each time a transaction is sent.

```javascript
// Overriding pending ETH transaction.
const transfer = await multichainWallet.transfer({
  recipientAddress: '0x2455eC6700092991Ce0782365A89d5Cd89c8Fa22',
  amount: 0,
  network: 'ethereum',
  rpcUrl: 'https://rinkeby-light.eth.linkpool.io',
  privateKey:
    '0f9e5c0bee6c7d06b95204ca22dea8d7f89bb04e8527a2c59e134d185d9af8ad',
  gasPrice: '10',
  nonce: 1, // The pending transaction nonce
});

// Overriding ERC20 token pending transaction.
const transfer = await multichainWallet.transfer({
  recipientAddress: '0x2455eC6700092991Ce0782365A89d5Cd89c8Fa22',
  amount: 0,
  network: 'ethereum',
  rpcUrl: 'https://rinkeby-light.eth.linkpool.io',
  privateKey:
    '0f9e5c0bee6c7d06b95204ca22dea8d7f89bb04e8527a2c59e134d185d9af8ad',
  gasPrice: '10',
  tokenAddress: '0xdac17f958d2ee523a2206206994597c13d831ec7',
  nonce: 1, // The pending transaction nonce
});
```

#### Response

```javascript
{
  hash: '0xf2978fe918f3e28b7f57101bbc99aa9d7d2d71507ca4a08bac6dd09575527502';
}
```

### Encryptions

#### Encrypt Private Key

It supports encryption of ethereum and other EVM compatible chains private keys.

```javascript
// encrypt private key.

const encrypted = await multichainWallet.getEncryptedJsonFromPrivateKey({
  network: 'ethereum',
  privateKey:
    '0f9e5c0bee6c7d06b95204ca22dea8d7f89bb04e8527a2c59e134d185d9af8ad',
  password: 'walletpassword',
});
```

#### Response

```javascript
{
  json: '{"address":"1c082d1052fb44134a408651c01148adbfcce7fe","id":"abfb9f10-165a-4b7a-935d-51729f10c310","version":3,"Crypto":{"cipher":"aes-128-ctr","cipherparams":{"iv":"f3fac53ee2d76c293977d1af3a7d73bb"},"ciphertext":"c5034579cdf32d7a612c9a83801aad899abfebb7436712f363ecf89546bbcbce","kdf":"scrypt","kdfparams":{"salt":"78ff80ece5d681b1aecd829526388472d1889da233229fa5c1416e8f2035b7a8","n":131072,"dklen":32,"p":1,"r":8},"mac":"0f70eca6138ffe60b174308b6ab7a8a81a0d2b662e2cf5d8727443cf12af766c"}}';
}
```

#### Decrypt Encrypted JSON

It supports decryption of encrypted JSONs (A.K.A keystore).

```javascript
// decrypting encrypted JSON.

const decrypted = await multichainWallet.getWalletFromEncryptedJson({
  network: 'ethereum',
  json:
    '{"address":"1c082d1052fb44134a408651c01148adbfcce7fe","id":"abfb9f10-165a-4b7a-935d-51729f10c310","version":3,"Crypto":{"cipher":"aes-128-ctr","cipherparams":{"iv":"f3fac53ee2d76c293977d1af3a7d73bb"},"ciphertext":"c5034579cdf32d7a612c9a83801aad899abfebb7436712f363ecf89546bbcbce","kdf":"scrypt","kdfparams":{"salt":"78ff80ece5d681b1aecd829526388472d1889da233229fa5c1416e8f2035b7a8","n":131072,"dklen":32,"p":1,"r":8},"mac":"0f70eca6138ffe60b174308b6ab7a8a81a0d2b662e2cf5d8727443cf12af766c"}}',
  password: 'walletpassword',
});
```

#### Response

```javascript
{
  privateKey: '0x0f9e5c0bee6c7d06b95204ca22dea8d7f89bb04e8527a2c59e134d185d9af8ad',
  address: '0x1C082D1052fb44134a408651c01148aDBFcCe7Fe'
}

```

### Token Info

#### Get ERC20 Token Info

Allows for fetching ERC20 tokens info from compatible blockchains by the token address

```javascript
// getting token info.

const info = await multichainWallet.getTokenInfo({
  address: '0x7fe03a082fd18a80a7dbd55e9b216bcf540557e4',
  network: 'ethereum',
  rpcUrl: 'https://rinkeby-light.eth.linkpool.io',
});
```

#### Response

```javascript
{
  name: 'Mocked USDT',
  symbol: 'USDT',
  decimals: 6,
  address: '0x7fe03a082fd18a80a7dbd55e9b216bcf540557e4',
  totalSupply: 1000000000000
}
```

#### Solana Network

Allows for the transfer of SOL and tokens.

```javascript
// Transferring SOL from one address to another.
const transfer = await MultichainCryptoWallet.transfer({
  recipientAddress: '9DSRMyr3EfxPzxZo9wMBPku7mvcazHTHfyjhcfw5yucA',
  amount: 1,
  network: 'solana',
  rpcUrl: 'https://api.devnet.solana.com',
  privateKey:
    'bXXgTj2cgXMFAGpLHkF5GhnoNeUpmcJDsxXDhXQhQhL2BDpJumdwMGeC5Cs66stsN3GfkMH8oyHu24dnojKbtfp',
});

// Transferring a token from one address to another.
const transfer = await MultichainCryptoWallet.transfer({
  recipientAddress: '9DSRMyr3EfxPzxZo9wMBPku7mvcazHTHfyjhcfw5yucA',
  tokenAddress: 'DV2exYApRFWEVb9oQkedLRYeSm8ccxNReLfEksEE5FZm',
  amount: 1,
  network: 'solana',
  rpcUrl: 'https://api.devnet.solana.com',
  privateKey:
    'h5KUPKU4z8c9nhMCQsvCLq4q6Xn9XK1B1cKjC9bJVLQLgJDvknKCBtZdHKDoKBHuATnSYaHRvjJSDdBWN8P67hh',
});
```

#### Response

```javascript
{
  hash: '3nGq2yczqCpm8bF2dyvdPtXpnFLJ1oGWkDfD6neLbRay8SjNqYNhWQBKE1ZFunxvFhJ47FyT6igNpYPP293jXCZk';
}
```

### Token Info

#### Get SPL Token Info

Allows for fetching SPL tokens info on the solana by the token address.
Note: Token has to be available on the solana token list registry

```javascript
// getting token info.

const info = await multichainWallet.getTokenInfo({
  address: '7Xn4mM868daxsGVJmaGrYxg8CZiuqBnDwUse66s5ALmr',
  network: 'solana',
  rpcUrl: 'https://api.devnet.solana.com',
  cluster: 'devnet',
});
```

#### Response

```javascript
{
  name: 'SimpiansDEV',
  symbol: 'SIMPDEV',
  address: '7Xn4mM868daxsGVJmaGrYxg8CZiuqBnDwUse66s5ALmr',
  decimals: 0,
  logoUrl: 'https://raw.githubusercontent.com/solana-labs/token-list/main/assets/mainnet/7Xn4mM868daxsGVJmaGrYxg8CZiuqBnDwUse66s5ALmr/logo.png',
  totalSupply: 10000000
}
```

Contributions are welcome! Kindly refer to the [contribution guidelines](CONTRIBUTING.md).
