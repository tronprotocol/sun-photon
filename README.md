# sun-photon

## Introduction

sun-photon is a development framework for smart contract development on SUN Network (Layer 2 of TRON Network). Core functionality includes: 

* smart contract compiling
* smart contract deployment
* smart contract testing

## Installation

npm install -g sun-photon

## System requirement

* NodeJS 8.0+
* Windows, Linux, or Mac OS X

## Initalize a project

Create project in an empty directory:

```bash
sun-photon init
```

* Shell

```bash
➜  new-project sun-photon init
Downloading...
Unpacking...
Setting up...
Unbox successful. Sweet!

Commands:

  Compile:        sun-photon compile
  Migrate:        sun-photon migrate
  Test contracts: sun-photon test
```

## Directory

* ./contracts     smart contract files
* ./migrations    smart contract migration scripts
* ./test          smart contract tests

## Commands

```bash
➜  new-project sun-photon --help
sun-photon v0.2.1 - a development framework for sunweb

Usage: sun-photon <command> [options]

Commands
  init     Initialize new and empty sun-photon project
  compile  Compile contract source files
  migrate  Run migrations to deploy contracts
  deploy   (alias for migrate)
  build    Execute build pipeline (if configuration present)
  test     Run JavaScript and Solidity tests
  console  Run a console with contract abstractions and commands available
  create   Helper to create new contracts, migrations and tests
  watch    Watch filesystem for changes and rebuild the project automatically
  serve    Serve the build directory on localhost and watch for changes
  exec     Execute a JS module within this sun-photon environment
  unbox    Download a sun-photon Box, a pre-built sun-photon project
  version  Show version number and exit

See more at https://developers.tron.network/docs/tron-box-user-guide
```

## Configuration files

Define settings in sun-photon.js. Use --network mainnet to set different network。

* sun-photon.js

```js
module.exports = {
  networks: {
    dappchain: {
      // Don't put your private key here:
      privateKey: process.env.PRIVATE_KEY_DAPPCHAIN,
      /*
Create a .env file (it must be gitignored) containing something like

  export PRIVATE_KEY_DAPPCHAIN=4E7FECCB71207B867C495B51A9758B104B1D4422088A87F4978BE64636656243

Then, run the migration with:

  source .env && npx sun-photon migrate --network dappchain

*/
      userFeePercentage: 100,
      feeLimit: 1e8,
      mainFullHost: 'https://api.trongrid.io',
      sideFullHost: 'https://sun.tronex.io',
      mainGateway: 'TWaPZru6PR5VjgT4sJrrZ481Zgp3iJ8Rfo',
      sideGateway: 'TGKotco6YoULzbYisTBuP6DWXDjEgJSpYz',
      chainId: '41E209E4DE650F0150788E8EC5CAFA240A23EB8EB7',
      network_id: '1'
    },
    testnet: {
      privateKey: process.env.PRIVATE_KEY_TESTNET,
      userFeePercentage: 50,
      feeLimit: 1e8,
      mainFullHost: 'https://testhttpapi.tronex.io',
      sideFullHost: 'https://suntest.tronex.io',
      mainGateway: 'TFLtPoEtVJBMcj6kZPrQrwEdM3W3shxsBU',
      sideGateway: 'TRDepx5KoQ8oNbFVZ5sogwUxtdYmATDRgX',
      chainId: '413AF23F37DA0D48234FDD43D89931E98E1144481B',
      network_id: '2'
    },
    compilers: {
      solc: {
        version: '0.5.4'
      }
    }
  }
}
```

## Contract compilation

Set correct solc version in tronbox.js and run command:

```bash
sun-photon compile
```

## Contract deployment

Create a .env file to import private key and run command:

```bash
source .env && sun-photon migrate --network mainnet
```

## Contract testing

Testing file should be in  ./test directory
Test demo:

* test.js

```js
var Test = artifacts.require('./test.sol');
contract('Test', function(accounts) {
  it('call method g', function() {
    Test.deployed()
      .then(function(instance) {
        return instance.call('g');
      })
      .then(function(result) {
        assert.equal('method g()', result, 'is not call method g');
      });
  });
  it('call method f', function() {
    Test.deployed()
      .then(function(instance) {
        return instance.call('f');
      })
      .then(function(result) {
        assert.equal('method f()', result, 'is not call method f');
      });
  });
});
```

Run below command:

```bash
source .env && sun-photon test ./test/test.js --network dappchain
```
