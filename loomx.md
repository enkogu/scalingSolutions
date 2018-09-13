## What is Loom Network?
Loom Network is a Layer 2 scaling solution for Ethereum that is live in production. It is a network of DPoS sidechains, which allows for highly-scalable games and user-facing DApps while still being backed by the security of Ethereum.

**Highly Scalable Sidechains**
Dubbed “EOS on Ethereum”, Loom’s DPoS sidechains provide the same high scalability and throughput promised by alternative platforms like EOS, while still being fully Ethereum-compatible and secure.

**Backed by Ethereum**
We built the world’s first Plasma Cash implementation, which allows Ethereum-based tokens to be used on Loom sidechains with the full security guarantees of Ethereum.

**Live in production**
Loom Network has been live in production since March 2018. Developers, start building on Loom today.

https://loomx.io

One of the first things newcomers discover about our project is that we don’t have a “whitepaper”—a document that has (rightfully or not) become somewhat of a requirement for companies building Blockchain tech.

Our core product is an SDK that allows developers to quickly build their own blockchains without having to understand blockchain infrastructure. Think of it like a “build your own blockchain” generator.

The Loom SDK generates what’s called a DAppChain — a layer-two blockchain that uses Ethereum as its base-layer.
Running each DApp on its own sidechain to Ethereum has a number of benefits, but most importantly:
DAppChains can use alternative consensus rulesets (like DPoS) that optimize for high scalability.
Using Ethereum as a base-layer means DAppChain-based assets (like ERC20 and ERC721 tokens) can have the security guarantees of Ethereum, especially when backed by Plasma.

Our SDK allows developers to choose their consensus algorithm and rulesets to customize the scalability / security tradeoffs to their DApps needs.
Out of the box we support DPoS (Delegated Proof of Stake), which enables large-scale online games and social apps — the 2 initial types of DApps we’re focusing on (though you can build any type of DApp on the Loom SDK).

**DelegateCall.com**
You can try it out at DelegateCall.com — it’s similar in architecture to Steemit, but instead of being a blogging platform, it’s a Q&A site related to the Blockchain and Ethereum programming.

Try it out yourself by signing up and asking a question to see an example of how performant DAppChain-based DApps can actually be in production — it feels just like a normal web 2.0 app even though it’s running 100% on the Blockchain.

https://medium.com/loom-network/everything-you-need-to-know-about-loom-network-all-in-one-place-updated-regularly-64742bd839fe


**DApps running on Loom DAppChains are democratic**
Users who want to support the DApp and have voting rights in its development can run their own nodes on the DAppChain. This means if the developers release an update that the users do not agree with, they can configure their nodes to reject the update and fork away.
Users are able to vote and express their opinions, compared to traditional games and web services where developers can issue a change that the community does not agree with.

**DApps running on Loom DAppChains are scalable**
Since all transactions happening on a DAppChain are specific to its DApp, it can run a consensus algorithm that optimizes for those particular types of transactions.
This is analogous to how ASICs are used in order to achieve very high performance for specific use-cases.

**Building Loom DAppChains is developer-friendly**
Developers will be able to use Loom’s Software Development Kit (SDK) to generate the foundation for a DAppChain.
Then, they can focus on writing the application logic while all the blockchain logic is handled for them.

**Summarizing, Loom’s DAppChains enable the following**
1. A user-friendly way for developers to spin up their own blockchain-based apps without having to know anything about implementing the actual blockchain logic, allowing them to focus on the core app logic.
2. Building full-scale apps such as MMORPGs and social media which are not limited by the high costs of gas on Ethereum and slow speeds.
3. Running the entire DApp on a decentralized blockchain, contrary to popular DApps which partly run on Ethereum, and the rest on a centralized web server.
4. DAppChains make DApps updatable, forkable, and have publicly shared data, allowing further experimenting and innovation.

https://medium.com/loom-network/dappchains-scaling-ethereum-dapps-through-sidechains-f99e51fff447


The term “sidechains” was first described in the paper “Enabling Blockchain Innovations with Pegged Sidechains”, circa 2014 by Adam Back et al. The paper describes “two-way pegged sidechains”, a mechanism where by proving that you had “locked” some coins that were previously in your posession, you were allowed to move some other coins within a sidechain.

Sidechains can increase scale but do not imply scalability. Sidechains are no better at providing scalability than increasing block size. What sidechains bring is the ability to experiment. To be able to build networks that run on different — and possibly better-scaling — technology.¹

https://medium.com/loom-network/million-user-dapps-on-ethereum-an-introduction-to-application-specific-sidechains-c0fdc288c5e5


DAppChains will periodically report Merkle Proofs acting as checkpoints to the Ethereum mainnet. The frequency would depend on the desired level of security and cost (as more proofs require more bandwith).
A Plasma Exit is the mechanism which allows a token holder to withdraw their funds from a Plasma Chain, in case of fraud or if they want to transact back on the mainnet.
While the current Plasma MVP prototype is a great reference, we will be building our own custom implementation specifically around its use case in games, in order to provide an improved user experience.

https://medium.com/loom-network/loom-network-plasma-5e86caaadef2

### Features of the Loom SDK
We’ll be slowly releasing more and more details and documentation around the SDK in the coming months, but here is a top-level overview:

#### Blockchain Features
1. Tokens — ERC20 compatible
1. Tokens — ERC721 Non-fungible compatible (Think CryptoKitties)
1. pBFT Practical Byzantine Fault Tolerance
1. DPoS Delegated Proof of Stake
1. Transactional DataStore
1. Protobuf support
1. Migration Framework
1. Hard Fork Manager
1. Golang Smart Contract support (Solidity coming soon, see below)
1. Unity client SDK (.NET)
1. Javascript client SDK
1. InterBlockchain Communication, see below

#### Cross-Blockchain Features
1. Ethereum Connectivity
1. Coin copying and transfering between chains
1. Allow assets like ERC721 tokens to be mirrored into the DAppChain
1. Pegging (this is in future releases)

#### Gaming Features
1. Matchmaking
1. Online Presence (Messaging in later releases)

https://medium.com/loom-network/loom-network-sdk-alpha-release-first-5-dappchains-announced-sdk-roadmap-1dddec789004


**Current Implementation and Next Steps**
Since our initial focus at Loom Network is building blockchain games like Zombie Battleground, our initial implementation of Plasma Cash is specifically for ERC721 non-fungible tokens.
Porting the code to handle ETH, ERC20s, and other types of tokens is fairly trivial, but will require more comprehensive tests.

### Technical Overview: Movement of a Token Between Mainnet and the Plasma Chain
The Plasma Cash implementation is made up of a Plasma smart contract that lives on Ethereum mainnet, and a Loom sidechain smart contract that communicates with the Plasma contract.
In order to use an ERC721 token on the sidechain, the user first sends their token to the Plasma contract.
After the token is received, the Plasma contract emits a Deposit event, which is picked up by the listening sidechain. The sidechain proceeds to create a block with a single transaction (this makes the exit process easier) that includes the deposited asset.
The user is then credited with a special Plasma Cash token on the sidechain that represents their ownership of the token on Mainnet. They’re free to transact and use that token on the sidechain in any way, including transferring it to other users on the sidechain (by providing a signed transaction to prove the other user is the new owner of the token).
The sidechain periodically “checkpoints” to Mainnet by committing the Merkle root of its blocks to the Plasma contract, showing any changes in ownership of the token.

#### Plasma Exits
When the user wants to exit their token from the sidechain, they submit an exit request directly to the Plasma contract on Mainnet (along with a signed transaction from the previous owner as proof if the token was transferred to them).
The token then enters a “challenge period”, in which a challenger can submit evidence of signed transactions that prove the user trying to exit the token is not its valid owner.
If the challenge period passes with no successful challenge, the user is able to withdraw their token from the Plasma Contract.
Thus, users are able to deposit and withdraw their tokens directly within the Mainnet Plasma contract, removing any risk of the token being stolen from them by the sidechain.

https://medium.com/loom-network/plasma-cash-initial-release-plasma-backed-nfts-now-available-on-loom-network-sidechains-37976d0cfccd

## Использование loom network для thetta
Ставлю приложение, все норм 
https://loomx.io/developers/docs/en/truffle-deploy.html
Тесты проходят, но с веба я так и не понял, почему кнопки не работают. Смотрел код, пытался приконнектится через метамаск, но без толку.

Чем особенно данная связка отличается от testrpc + truffle + react я так и не понял. Кажется, такое повторить легко и самим, во всяком случае с PoW.

Далее читаю про gateway.
https://loomx.io/developers/docs/en/transfer-gateway.html
Лочим в mainnet, через oracle и два gateway трансферим ERC721 токены.

Вот тут есть пример: 
https://github.com/loomnetwork/transfer-gateway-example

Как это можно использовать?

Если бы у нас был PoW, то сетку легко было бы захватить небольшой вычислительной мощностью. Но так как у нас DPoS, нужно просто чтобы...

### Как это может работать на примере некоторой организации?
Допустим, есть у нас та самая служба по доставке обедов.
Есть сайт-лендинг, юзер читает, ему все нравится.

Юзер качает некоторое приложение, например на electron, которое так же запускает ноду нашего DPoS чейна.

**MAINNET**
Есть контракт ERC721_LUNCH_tokens.
Есть контракт обмена LUNCH_tokens_exchanger, который меняет токены на эфиры.
Есть контракт LOOMX_gateway, который переводит токены в нашу сеть.
Есть контракт LUNCH_auto, на который можно перечислить эфиры, он их поменяет на токены и отправит их в gateway.

Например, 1 ether == 25 LUNCH tokens.

Юзер перечисляет эфир на LUNCH_auto, в итоге на LUNCH_tokens_exchanger залочена эфирка, на LOOMX_gateway залочено 25 LUNCH tokens.

В итоге в LUNCH chain'е выпускается 25 LUNCH tokens на ERC721_LUNCH_tokens.
Юзер перечисляет их в LUNCH_DAO_Client.

Далее в течение месяца юзер голосует за разные обеды, за поваров, получает обеды и ест их (или не ест), тратя при этом токены.

Юзер использует для этого приложение на electron. 

Далее у юзера осталось три токена, и он их хочет вывести. Он вызывает функцию transfer_to_gateway(3), gateway переводит в mainnet через oracle, теперь у него на ERC721_LUNCH_tokens в mainnet есть 3 токена. Юзер отправляет их на LUNCH_tokens_exchanger и получает в обмен деньги. 

То же самое делают повара, переводят 10000 полученных за месяц токенов и получают залоченные деньги. 

**Как происходит выбор валидаторов?**
По идее, стейком должен быть LUNCH_token. Можно сделать контракт выкупа LUNCH_token в DPoS-чейне. Или можно сделать так, что эфирка в DPoS-чейне и есть LUNCH_token, то есть на них все покупается. Но для gateway нужен именно ERC721-token. И можно сделать такой-же LUNCH_tokens_exchanger. 

В итоге, есть, например, у юзеров 70 местных эфиров, ими они и принимают решения о валидаторах. 

Но стоимость взлома всегда должна быть высокой, не менее стоимости бизнеса. А что если всего 10 юзеров, у них 10 эфирок, приходит злодей, купив 11 эфирок, и голосует за себя, и ведет себя при этом плохо как валидатор?

Например, у хозяев бизнеса может быть изначально 1000 эфирок, но при этом они их не могут вывести; например, можно выводить только юзерам по списку. Но тогда сетка будет централизованной. Но мотивации ее ломать у владельца бизнеса не будет. Все равно он скорее всего и так все деньги получит, которые залочены. Максимум, что он сможет сделать – кинуть на предоплаченные уже обеды.

А потом, когда клиентов станет больше, эти эфирки можно будет сжечь.

**Для чего тут нужна thetta?**
Во-первых, есть готовый фреймворк для работы бизнеса в DPoS-чейне.
Во-вторых, мы можем настроить инфраструктуру в mainnet, чтобы процессы размещения бизнеса и интеграции с DPoS-чейном были очень простыми, центральные контракты предоставим.

**Для чего тут может пригодиться loom (или что-то альтернативное)?**
Или по-другому:
**Что придется делать самим, без использования этих решений?**
Например, если сетка будет одна на все приложения, то можно будет сделать thetta-token универсальным платежным средством; все средства внутри thetta-chain будут обеспечены эфиром.
то нужно будет:
1. развернуть DPoS сеть, например BitShares/EOS, задеплоить оракула и gateway в mainnet; 
2. создать сборку для деплоя своего dao в thetta-chain.
3. создать базовое приложение с UX для демонстрации. 

А что вообще у loomx за чейн, на чем он работает, как его безопасность, EVM-ли у него, и если да, то как они сделали DPoS? Сообщество инспектировало? Об этом почему-то нигде нет. Запускается он через бинарник. Нигде в 73 репах нет сорцов, в статье и доках нет никакой инфы. Это закрытый код. 

https://loomx.io/developers/docs/en/delegated-proof-of-stake.html
