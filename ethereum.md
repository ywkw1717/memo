# make ethereum private network in ubuntu

[https://github.com/ethereum/go-ethereum/wiki/Installation-Instructions-for-Ubuntu](https://github.com/ethereum/go-ethereum/wiki/Installation-Instructions-for-Ubuntu)

## install geth from PPA
```shell
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
```

```shell
ubuntu@hogehoge:~$ tree .ethereum/
.ethereum/
├── geth
│   ├── chaindata
│   │   ├── 000001.log
│   │   ├── CURRENT
│   │   ├── LOCK
│   │   ├── LOG
│   │   └── MANIFEST-000000
│   ├── LOCK
│   ├── nodekey
│   └── transactions.rlp
└── keystore

3 directories, 8 files
```

install finished.

```shell
ubuntu@hogehoge:~$ geth version
Geth
Version: 1.7.3-stable
....................
```

## make a account
```shell
ubuntu@hogehoge:~$ geth account new
Your new account is locked with a password. Please give a password. Do not forget this password.
Passphrase:
Repeat passphrase:
Address: {efcc36522cd2a74d6c45089f76900ad455124124}
```

## make genesis.json

$HOME/.ethereum/genesis.json
```
{
  "nonce": "0x0000000000000042",
    "timestamp": "0x0",
    "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "extraData": "",
    "gasLimit": "0x2FEFD8",
    "difficulty": "0x200",
    "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "coinbase": "0x0000000000000000000000000000000000000000",
    "alloc": {}
}
```
上記だと `Fatal: Failed to write genesis block: genesis has no chain configuration` と言われてしまうので、

```
"config": {
  "chainId": 10,
    "homesteadBlock": 0,
    "eip155Block": 0,
    "eip158Block": 0
},
```
を先頭に追加

## initialize Geth

```shell
ubuntu@hogehoge:~$ geth --datadir=/home/ubuntu/.ethereum init /home/ubuntu/.ethereum/genesis.json
INFO [12-11|11:58:17] Allocated cache and file handles         database=/home/ubuntu/.ethereum/geth/chaindata cache=16 handles=16
INFO [12-11|11:58:17] Writing custom genesis block
INFO [12-11|11:58:17] Successfully wrote genesis state         database=chaindata                             hash=49c2dd…aff051
INFO [12-11|11:58:17] Allocated cache and file handles         database=/home/ubuntu/.ethereum/geth/lightchaindata cache=16 handles=16
INFO [12-11|11:58:17] Writing custom genesis block
INFO [12-11|11:58:17] Successfully wrote genesis state         database=lightchaindata                             hash=49c2dd…aff051
```


## reset of Ethereum
```shell
$ rm -rf .ethereum
```
してから、上記の手順を実行
