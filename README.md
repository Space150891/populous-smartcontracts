# [Truffle](https://github.com/trufflesuite/truffle) instance with system's smart contracts.
Smart contracts for platform

### If you need to set up new local or remote geth instance, use this instructions:
**Use a remote `geth` server instead of setting up a local instance. It takes a long time for set up it on local (4-6 hours).**
***

### Setup geth instance from scratch

1. Install geth on your machine click [here](https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum) for the installation docs.
When running `geth` commands, use the `--testnet` flag when connecting to the Ropsten test blockchain.

2. To create local Ethereum account (wallet), run the command: `$ geth --testnet account new`
Use any password, but make sure that you remember it

3. So you have installed geth and created account under `testnet`. 
Start to sync you local instance with ethereum.
For this run the command 
```
$ geth --fast --cache=1048 --testnet --unlock 0 --rpc --rpcapi "eth,net,web3,personal" --rpccorsdomain="*" --wsport 8546 --ws --wsapi "eth,net,web3,personal" --wsorigins="*" console
``` 
This command set up rpcapi for syncing and websoket connection in parallel
This will start up the node and attach to the console. Note it will ask you for the passphrase for your wallet, simply type your password for previously created wallet (notice that type password process not displyed on console) and hit `Enter`.


4. Wait for sync is finished, this can be checked by command `eth.syncing` should return `false`, if sync is in progress it output something like this
```
{
  currentBlock: 2338011, // current progress
  highestBlock: 2345015, // last block in current chain 
  knownStates: 0,
  pulledStates: 0,
  startingBlock: 2337813
}
```

5. Once `eth.syncing` return `false` setup is complete

6. Leave terminal open with launched geth command

So now you have set up geth instance and need to prepare your wallet for contracts deploy.

**1. run the command `geth --testnet account list`**
it's show something like this
`Account #0: {e0d535161b38457e409dc9d27598d9f7c3c509ee} keystore:///Users/apple/Library/Ethereum/testnet/keystore/UTC--2017-12-19T08-39-17.126278733Z--e0d535161b38457e409dc9d27598d9f7c3c509ee`

here we can see
- `Account #0` - account index in accounts list
- `e0d535161b38457e409dc9d27598d9f7c3c509ee` - account address, for use this address with blockchain you should add prefix `0x`. It's strangely but take it as a feature. 
- `///Users/apple/Library/Ethereum/testnet/keystore/UTC--2017-12-19T08-39-17.126278733Z--e0d535161b38457e409dc9d27598d9f7c3c509ee` - most important part, your local key file it's your login in `Ethereum` system

2. To deploy contacts to the network you need some Ether. For this we can use service like [EtherTools faucet](https://ethtools.com/ropsten/tools/faucet/). Please use Google chrome for actions listed above

3. You need to login in the system, go to ethTools login. we can use key file and password for login
 
You can get keyfile content with one of the commands
- `$ cat <path-to-your-keyfile> | pbcopy` - copy file content to buffer
- `$ cat <path-to-your-keyfile>` - output file content, you have to copy it manualy

Output should be valid  JSON with structure like this
```
{"address":"e0d535161b38457e409dc9d27598d9f7c3c509ee","crypto":{"cipher":"aes-128-ctr","ciphertext":"0f832ca28b97aa2b8f7ec1532d38a236805ec4ffd10b02c2b896b045850cb227","cipherparams":{"iv":"038f75c6b6b4dbc326bb66d9aa80f531"},"kdf":"scrypt","kdfparams":{"dklen":32,"n":262144,"p":1,"r":8,"salt":"ddeda050a5466d57fea152cbcf92dc99aae31456be2b3850c57f6514d9db8ecf"},"mac":"fa9591a546dd1516dfa62419514b3c4c9c6b576e1f297bb30c82a47d0f1f39f2"},"id":"6356b5a6-c395-4341-b78c-36b5606cda93","version":3}
```

4. After successful login go to faucet `Main navigation > Tools > faucet`

5. Here click "Choose account" button and chose your account using info from point #3

6. After account is chosen type required amount of test ether, notice that amount can't be more then `5`, so recommend to request `5`

7. wait until your request will be processed by the system and miners. I takes approx one minute  

8. After request has been processed, you can check your account balance, sync your request with you local instance will take approx 1-2 minutes, only if you have fully synced instance

9. Go to geth console, it should be launched all this time. Check your account balance with command 
`web3.fromWei(eth.getBalance("{your account address with '0x' prefix}"), "ether")`
It should output `5`

Now you are ready for deploy contracts

“Deploying” the contacts means compile and send them to the Ethereum network. During testing we use the “Ropsten” test network so we’re not trading with real money. There is also a package called TestRPC which allows for local testing. This guide will focus on using Ropsten though as it’s well tested.

### Step by step instructions

1. Install Truffle globally with `npm install -g truffle`
2. Clone the repo and cd to repo dir
3. Edit `truffle.js` to setup your node's configuration. It's not required for remote instance, only if plan to use local one
4. `truffle compile` - this command compile all `.sol` contracts from `contracts` directory to `build/contracts` directory, as output you get `.json` representation for each `.sol` contract. **Notice: on this stage `networks` object is empty and can't be used by eth packages**
5. `truffle deploy --network ropsten` - will deploy compiled contracts to the system and fill in `networks` object in `json` file with deploy info

For default truffle config and ropsten network, networks entry should looks like this
```
 "3": {
      "events": {},
      "links": {},
      "address": "0xc0cdc8d81dd1ff9c650a00fa93f165ab0a7c9053" // address should be another
    }
```


## Ropsten Test Network Smart Contract Addresses

AccessManager.sol - `0x0ebbaf0c3794ed23a0871e411a34be3a1679753a`   
PopulousToken.sol - `0x0ff72e24af7c09a647865820d4477f98fcb72a2c`      
SafeMath.sol- `0xe53d0a5d27984c3144286a2973e1534deafc5070`          
Populous.sol- `0x48739e5161eeb3b1edf7402bbce7da0ffaf7ce65`           
Utils.sol- `0xd44422fe990fe98ea0f97b77b45e4c92be16bf40`

Platform Admin/Server Address - `0xf8b3d742b245ec366288160488a12e7a2f1d720d`

## Live Network Smart Contract Addresses

AccessManager.sol - `0x98ca4bf7e522cd6d2f69cf843dfab327a1e26497`   
PopulousToken.sol - `0xd4fa1460f537bb9085d22c7bccb5dd450ef28e3a`      
SafeMath.sol- `0x99cd218aa0f946b41404d737f4a83ec8614cc105`          
Populous.sol- `0x5ead05ffbc2aaa119a90d150f0a058a8e6eb945b`           
Utils.sol- `0x25dc9b7068d23bab9cb0f198117617da44167cc9`

Platform Admin/Server Address - `0x63d509f7152769ddf162ed048b83719fe1e31080`

## Using technologies

JavaScript, Blockchain