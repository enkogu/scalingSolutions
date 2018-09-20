# Parity bridge
the bridge connects two chains home and foreign.

when users deposit ether into the HomeBridge contract on home they get the same amount of ERC20 tokens on foreign.

they can use ForeignBridge as they would use any ERC20 token.

to convert their foreign ERC20 into ether on home users can always call ForeignBridge.transferHomeViaRelay(homeRecipientAddress, value, homeGasPrice).

**foreign is assumed to use PoA** (proof of authority) consensus. relays between the chains happen in a byzantine fault tolerant way using the authorities of foreign.
https://github.com/paritytech/parity-bridge

## high level explanation of home ether -> foreign ERC20 relay
sender deposits value into HomeBridge. the HomeBridge fallback function emits Deposit(sender, value).

for each Deposit event on HomeBridge every authority executes ForeignBridge.deposit(sender, value, transactionHash).

**Comment: HOW? Oracle?**

once there are ForeignBridge.requiredSignatures such transactions with identical arguments and from distinct authorities then ForeignBridge.balanceOf(sender) is increased by value.

## high level explanation of foreign ERC20 -> home ether relay
sender executes ForeignBridge.transferHomeViaRelay(recipient, value, homeGasPrice) which checks and reduces ForeignBridge.balances(sender) by value and emits ForeignBridge.Withdraw(recipient, value, homeGasPrice).

for every ForeignBridge.Withdraw, every bridge authority creates a message containing value, recipient and the transactionHash of the transaction referenced by the ForeignBridge.Withdraw event; signs that message and executes ForeignBridge.submitSignature(signature, message). this collection of signatures is on foreign because transactions are free for the authorities on foreign, but not free on home.

once ForeignBridge.requiredSignatures signatures by distinct authorities are collected a ForeignBridge.CollectedSignatures(authorityThatSubmittedLastSignature, messageHash) event is emitted.

everyone (usually authorityThatSubmittedLastSignature) can then call ForeignBridge.message(messageHash) and ForeignBridge.signature(messageHash, 0..requiredSignatures) to look up the message and signatures and execute HomeBridge.withdraw(vs, rs, ss, message) and complete the withdraw.

HomeBridge.withdraw(vs, rs, ss, message) recovers the addresses from the signatures, checks that enough authorities in its authority list have signed and finally transfers value ether (minus the relay gas costs) to recipient.

https://github.com/paritytech/parity-bridge

## deployment guide

https://github.com/paritytech/parity-bridge/blob/master/deployment_guide.md

## Что и как сделать, чтобы thetta работала в private PoA, но можно было бы выводить обмениваться деньгами с mainnet (testnet для начала)
1. Задеплоить сетку и настроить ее
2. Воспользоваться deployment guide 
3. Протестировать это

Кто-то наверняка уже это делал. Например, вот эти ребята: 
https://github.com/fernnetwork/fern-parity-bridge-scripts

### fern-parity-bridge-scripts
Fern uses the parity-bridge project to connect private PoA networks to a public network like ethereum. The bridge will enable token transfers between the networks.

This repository contains scripts and configuration files for testing parity bridge between a private PoA network and the Kovan network.

The bin directory contains parity bridge binaries built from bdc00d1 of parity-bridge.

To run and test the bridge locally, you will have to create an account and request some ethers from faucet.

### Что за fern? 
Ничего у них нет, только планируют
https://fern.network/

### poanetwork: parity-bridge-research
TODO
https://github.com/poanetwork/parity-bridge-research

## ПЛАН
### 1. Разворачиваю свою сеть по гайду
https://hackernoon.com/setup-your-own-private-proof-of-authority-ethereum-network-with-geth-9a0a3750cda8

FROM UPD: thx to Ivica Aracic for pointing out that clique PoA DOES WORK with a single node. For any reason I missed that and I apologize for the confusion. With a single node, we just need (A) create genesis file with only one sealer (only 1 address in extraData ) (B) create an account (C) init geth (D) run geth, unlock account and mine. No bootnode is required then.

```bash
brew tap ethereum/ethereum && brew install ethereum
mkdir devnet && cd devnet && mkdir node1 node2
geth --datadir node1/ account new
# passphrase: thetta-2018-enkogu
# address: 570921f0f58021d5421b8b565f8734221ae1813a
geth --datadir node2/ account new
# passphrase: thetta-2018-tony
# address: 4305338de1109625a378aea31a9e32699d095cd2
echo '570921f0f58021d5421b8b565f8734221ae1813a' >> accounts.txt
echo '4305338de1109625a378aea31a9e32699d095cd2' >> accounts.txt
echo 'thetta-2018-enkogu' > node1/password.txt
echo 'thetta-2018-tony' > node2/password.txt
puppeth
# devnet
# 2
# 2
# 5
# 570921f0f58021d5421b8b565f8734221ae1813a
# 4305338de1109625a378aea31a9e32699d095cd2
# 570921f0f58021d5421b8b565f8734221ae1813a
# 4305338de1109625a378aea31a9e32699d095cd2
# 1970 
# 2
# 2
# genesis.json
geth --datadir node1/ init genesis.json
geth --datadir node2/ init genesis.json
bootnode -genkey boot.key
bootnode -nodekey boot.key -verbosity 9 -addr :30310
# INFO [09-14|13:14:43.343] UDP listener up                          self=enode://9ed97c9e7478a9ecda8e507584539ed8156efad4fda36b52aed887aa4ad8b9d6bdfbc238897894e288aa129e54cc554fa94c1fd8ab37ecaaf6e5d0debb4340b1@[::]:30310

geth --datadir node1/ --syncmode 'full' --port 30311 --rpc --rpcaddr 'localhost' --rpcport 8501 --rpcapi 'personal,db,eth,net,web3,txpool,miner' --bootnodes 'enode://9ed97c9e7478a9ecda8e507584539ed8156efad4fda36b52aed887aa4ad8b9d6bdfbc238897894e288aa129e54cc554fa94c1fd8ab37ecaaf6e5d0debb4340b1@127.0.0.1:30310' --networkid 1970 --gasprice '1' -unlock '570921f0f58021d5421b8b565f8734221ae1813a' --password node1/password.txt --targetgaslimit 94000000 --mine

geth --datadir node2/ --syncmode 'full' --port 30312 --rpc --rpcaddr 'localhost' --rpcport 8502 --rpcapi 'personal,db,eth,net,web3,txpool,miner' --bootnodes 'enode://9ed97c9e7478a9ecda8e507584539ed8156efad4fda36b52aed887aa4ad8b9d6bdfbc238897894e288aa129e54cc554fa94c1fd8ab37ecaaf6e5d0debb4340b1@127.0.0.1:30310' --networkid 1970 --gasprice '0' --unlock '4305338de1109625a378aea31a9e32699d095cd2' --password node2/password.txt --targetgaslimit 94000000 --mine

# fix genesis
#     "chainId": 1970,
#     "gasLimit": "0x59A5380",
#     addresses

# REMOVE GETH FOLDERS

geth --datadir node1/ init genesis.json
geth --datadir node2/ init genesis.json
```

2. Сам настраиваю bridge на rinkeby (тут для mainnet и не очень подробно, но должен разобраться)
https://github.com/paritytech/parity-bridge/blob/master/deployment_guide.md

```bash
brew tap paritytech/paritytech
brew install parity --devel
brew tap ethereum/ethereum
brew install solidity
# install cargo && rast
curl -sSf https://static.rust-lang.org/rustup.sh | sh
# then https://github.com/paritytech/parity-bridge/#build
git clone https://github.com/paritytech/parity-bridge.git
cd parity-bridge 
cargo build -p parity-bridge --release

```

2.1. Если возникают затруднения, то я изучаю https://github.com/poanetwork/parity-bridge-research
2.2. Если не помогает, то я изучаю poanetwork/bridge-nodejs https://github.com/poanetwork/bridge-nodejs
3. Деплою токен-контракт в rinkeby и в чатную сетку. Пытаюсь перевести токены
4. Если получается, разворачиваю thetta в частной сети, делаю простой dao-client, в котором пара функций зависит от токенов. Какое-нибудь голосование, или что-то такое. Делаю контракт-обменник на dao-токены. Тестирую


```bash
git clone https://github.com/paritytech/parity-bridge.git
cd parity-bridge
cargo build -p parity-bridge --release
# copy target/release/parity-bridge into a folder that's in your $PATH.
# copy integration-tests/bridge_config.toml -> config.toml
# config.toml: "../compiled_contracts" -> "./compiled_contracts"
```

Постоянно проблема, что требуется db.toml, притом что бы не написал в созданный файл, всегда будет ошибка парсинга, притом он должен создаваться сам, если не указан, но ругается, если файл пустой/нет файла
