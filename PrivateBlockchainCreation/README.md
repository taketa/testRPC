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
`geth --datadir PrivChain1 init genesis.json`  
--datadir specifies where we want the all the data for the blockchain to be located.
3) Creating another node
`geth --datadir PrivChain2 init genesis.json`
4) Log into the geth console for each node   
```
geth --datadir PrivChain1 --networkid 111 --rpc --port 30301 --rpcapi "eth,web3,personal,net,miner,admin,debug"  --nodiscover console
geth --datadir PrivChain1 --networkid 111 --rpc --port 30302 --rpcapi "eth,web3,personal,net,miner,admin,debug"  --mine --minerthreads=1 --nodiscover console
``` 
**--networkid** is similar to in the `genesis.json` file, where all we want here is to make sure we’re not using network ids 1-4.  
**--port** specifies which port our .ipc file will be using. That’s the way we’ll connect with the database using the web3.js library. The default port is 30303, so we’ll keep it in that area, but this is our first node, so 30301 it is.  
**--nodiscover** tells geth to not look for peers initially. This is actually important in our case. This is a private network. We don’t want nodes to try to connect to other nodes without me specifying, and we don’t want these nodes to be discovered without us telling them.  
**--mine** start it in mining mode  
5) Connecting Nodes as Peers  
  - check to see if we have peers  
    `admin.peers`
  - Get the enode address  
    `admin.nodeInfo.enode`  
    `"enode://13b835d68917bd4970502b53d8125db1e124b466f6473361c558ea481e31ce4197843ec7d8684011b15ce63def5eeb73982d04425af3a0b6f3437a030878c8a9@[::]:30301?discport=0"`
  - connect the nodes using url  
    `admin.addPeer("enode://13b835d68917bd4970502b53d8125db1e124b466f6473361c558ea481e31ce4197843ec7d8684011b15ce63def5eeb73982d04425af3a0b6f3437a030878c8a9@[::]:30301?discport=0")`
  - check that peer connects 
    `admin.peers`
```
[{
    caps: ["eth/63"],
    id: "99bf59fe629dbea3cb3da94be4a6cff625c40da21dfffacddc4f723661aa1aa77cd4fb7921eb437b0d5e9333c01ed57bfc0d433b9f718a2c95287d3542f2e9a8",
    name: "Geth/v1.7.1-stable-05101641/darwin-amd64/go1.9.1",
    network: {
        localAddress: "[::1]:30301",
        remoteAddress: "[::1]:50042"
    },
    protocols: {
        eth: {
            difficulty: 935232,
            head: "0x8dd2dc7968328c8bbd5aacc53f87e590a469e5bde3945bee0f6ae13392503d17",
            version: 63
        }
    }
}]
```
