# Smartcontracts-Upgrade
Code here : https://github.com/Salmandabbakuti/Smartcontract-Upgrades

## Environment
gochain.io

## Pre-Requisites

1.gochain Web3 library
```
curl -LSs https://raw.githubusercontent.com/gochain-io/web3/master/install.sh | sh
```
2.go chain wallet and some GO-tokens.

go to https://explorer.gochain.io/wallet/create store your privatekey somewhere else safe.

to do any operations on network, you need tokens. so to get testnet tokens join their official telegram channel https://t.me/gochain_testnet and ask them to provide some testnet tokens by pasting your previously created wallet address**

## Steps

### 1. Environment Setup

 a). Export private key to your Env variables (require root user previligies ``` $ sudo -s ``` )
 ```
 export WEB3_PRIVATE_KEY=0x...
 ```
 Check if your private key is correct by this command ``` $ web3 myaddress``` if this command returns your wallet address that you created as in pre-requisites, you good to go.
 
 b). Use the GoChain testnet
 ```
$ export WEB3_NETWORK=testnet
```
### 2. Deploying an Upgradeable Contract

a). Compiling Contract

 copy greeter.sol file to your directory, then 
 
 ```
 web3 contract build greeter.sol
 ```
 b). Deploying contract
 
 ```
 web3 contract deploy --upgradeable greeter.bin
 ```
 once deployed, you will get contract address, add this address to env. variables
 ```
 export WEB3_ADDRESS=0x...
 ```
 deploying upgradable contract actually deploy two contracts original greeter contract and proxy contract which will redircts calls to upgraded contract. check where our proxy contract redirecting calls and storage by this command
 
 ```
 web3 contract target
 ```
 
 ### 3. Upgrading Contract
 
  Copy greeterv2.sol to your directory and then,
  ```
 web3 contract build greeterv2.sol
 web3 contract deploy greeter.bin
```
Once deployed, it will return newly deployed contract address. using this address we can upgraade our old contract ```greeter``` to ```greeterv2``` run below command to finish one last step
```
 web3 contract upgrade --to 0x.... //add your newly deployed contract address here
```
now you can call new contract with the same address. in old contract, there is no getter function for variable , in new contract, we added getter function ```getGreeting```. now you can call it using this command.

```
web3 contract call --abi greeter.abi --function getGreeting
```

### 4. Pausing and Resuming Contracts

Upgradeable contracts also include the ability to pause & resume execution. if you found any bug in contract you can simply pause (or resume later) the contract with these commands

```
web3 contract pause
web3 contract resume
```
once paused, if you call function you will get ```unmarshalling empty output``` string till you resume contract.

BOOM...

#### Salman Dabbakuti
