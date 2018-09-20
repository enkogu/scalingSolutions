# POA Network
POA Network is an Ethereum-based platform that offers an open-source framework for smart contracts. POA Network is a sidechain to Ethereum utilizing Proof of Authority as its consensus mechanism.

POA20 Tokens were minted to prove cross chain bridges can work in a safe and secure manner allowing 2 standalone blockchains to interact with each other. We believe the interoperability technology we’ve been able to develop helps open up a whole new avenue of solutions that can be offered to our users. By connecting blockchains with these bridges you allow for a variety of new use cases that never existed before. POA Network truly believes cross chain bridges is a critical step in the right direction as we aim to solve the problem of scalability and connectivity.

https://poa.network/bridge

### On-Chain Governance
At its core, on-chain governance is a group of individuals coming together to make decisions on the blockchain. POA Network aims to improve the governance process by making it more secure, efficient and most importantly transparent. 

POA Network remains transparent by using US public notaries, known as validators, to govern the network. Validators make all governance decisions through exclusive Distributed Applications (DApps) and record all decisions on the POA blockchain. This process creates an effective self-governed system where changes can be made quickly and efficiently to better serve the POA Network.

**What is on-chain governance? What are its benefits?**
On-chain governance is the process by which validators coordinate and manage themselves within a blockchain. Some benefits include an increase of coordination and better transparency of the entire decision making process. 
In on-chain governance, the process is defined in the code and consistently followed. The governance proposals and vote decisions are recorded on blockchain.

**What is a validator?**
A Validator is an independent individual who stakes their identity and is entrusted to maintain a node on the network that validates transactions and commits new blocks to the blockchain. Validators receive a reward in POA token for provisioning blocks.

**What is the reasoning behind enforcing validators to be notaries public?**
This is a way to verify one’s identity and it puts the validators Identity at Stake to encourage the validators to not act maliciously against the network in any way.

**How are validators selected?**
Validators are selected through various considerations such as knowledge about Blockchain, background, merit, etc but most importantly, evaluated by their public reputation how they can potentially help improve the network.

**What are ballots used for?**
Ballots are used to propose changes to the network, onboard/remove validators or to propose changes to consensus.

**What is the process of voting via ballots?**
To vote on a ballot, you need to have the correct network selected in MetaMask or Trust Wallet and have the Validator’s Voting Key selected. Visit https://voting.poa.network and cast your vote for an active ballot.

**Can validators collude to vote their friends in as validators?**
While collusion is always a possibility, the reality is that there are 21 validators therefore for any vote to pass, there must be 1) a minimum number of votes met and 2) a majority threshold must be met in order to vote in any new validators. If a validator does try to collude and bring in an actor that may not be beneficial of the network, it is unlikely they will meet the minimum threshold for the vote to be successful.

https://poa.network/governance


#### Honey Badger BFT Consensus
The Honey Badger Byzantine Fault Tolerant (HBBFT) algorithm is in development to support an alternative consensus mechanism for blockchain transactions. Built in Rust, this modular library allows nodes in a distributed, potentially asynchronous environment to achieve agreement on transactions. The agreement process does not require a leader node, tolerates corrupted nodes, and makes progress in adverse network conditions.

https://poa.network/dapps

## poanetwork: bridge-nodejs
Вроде как тут две сетки разворачиваются, и между ними идут транзакции через offchain wather'ы
https://github.com/poanetwork/bridge-nodejs