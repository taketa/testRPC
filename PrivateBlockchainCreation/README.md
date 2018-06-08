## Private Blockchain Creation
1) To create a single node, we need the following genesis.json, which represents the initial block on the private blockchain.
``` 
//genesis.json
{
 "alloc": {},
 "config": {
   "chainID": 72,
   "homesteadBlock": 0,
   "eip155Block": 0,
   "eip158Block": 0
 },
 "nonce": "0x0000000000000000",
 "difficulty": "0x4000",
 "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
 "coinbase": "0x0000000000000000000000000000000000000000",
 "timestamp": "0x00",
 "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
 "extraData": "0x11bbe8db4e347b4e8c937c1c8370e4b5ed33adb3db69cbdb7a38e1e50b1b82fa",
 "gasLimit": "0xffffffff"
}
```
2) Initialize the blockchain for this node.  
`geth --datadir "/Users/USERNAME/PrivChain1" init genesis.json`
--datadir specifies where we want the all the data for the blockchain to be located.
3) Creating another node
`geth --datadir "/Users/USERNAME/PrivChain2" init genesis.json`
4) Log into the geth console for each node   
```geth --datadir PrivChain1 --networkid 111 --rpc --port 30301 --rpcapi "eth,web3,personal,net,miner,admin,debug"  --nodiscover console
geth --datadir PrivChain1 --networkid 111 --rpc --port 30302 --rpcapi "eth,web3,personal,net,miner,admin,debug"  --nodiscover console``` 
**--networkid** is similar to in the `genesis.json` file, where all we want here is to make sure we’re not using network ids 1-4.  
**--port** specifies which port our .ipc file will be using. That’s the way we’ll connect with the database using the web3.js library. The default port is 30303, so we’ll keep it in that area, but this is our first node, so 30301 it is.  
**--nodiscover** tells geth to not look for peers initially. This is actually important in our case. This is a private network. We don’t want nodes to try to connect to other nodes without me specifying, and we don’t want these nodes to be discovered without us telling them.  
