## 11:00 - 11:20

| Presenter |Title|
| -------- | -------- |
|Viktor Tron, Daniel A. Nagy, Piper Merriam |  Ethereum State on Swarm |

## Viktor Tron, Daniel A. Nagy, Piper Merriam: Ethereum State on Swarm
Storing the ethereum state as well as timely access to current state have
proven real bottlenecks in scaling ethereum.

Getting input from developers of several Ethereum clients
(Trinity, geth, turbo geth) prompted us to draw up a staged plan that will help alleviate these bottlenecks by using Swarm for storing and serving blockchain data. 

Each milestone of the development track will deliver valueable functionality supporting: 
- (0) header chain,
- (1) trie node requests for the ethereum subprotocol,
- (2) light client requests for accounts with their Merkle proof, 
- (3) syncing the state efficiently even without running the VM.

In this talk, we explain the architecture of the solution and give a roadmap, also presenting the role and implementation status of swarm subsystems dependencies. Finally, we demonstrate our initial MVP for the implementation of (0) using the bzz-eth protocol which allows ethereum nodes and swarm nodes to connect and request headers to build the header chain.

### About Viktor
Currently team lead for the swarm project, Viktor has worked for the Ethereum Foundation since the beginning. Committed to the ideal of a sovereign ditital society, he has a keen interest in decentralisation, cryptography, networking, data structures and algorithms and believes in technology and innovation as the conduit for peaceful social change. A long-time contributor to the open source community he is working on architecting base layer infrastructure for web3, the decentralised world wide web.

### About Dani
Daniel A. Nagy, Ethereum Swarm architect and developer. Has been active in financial cryptography since 2008 at ePoint Systems Ltd. of which he is one of the founders. PhD in applied mathematics from Queen’s University of Kingston, Ontario, Canada. Teaching Advanced Cryptography at ELTE Budapest University of Science in Budapest, Hungary.

### About Piper
Piper Merriam is the team lead for the Ethereum Foundation "Snake Charmers" team which maintains most of the python tooling for the Ethereum ecosystem as well as the Trinity client for both the Eth2 and Eth2 networks.  Piper has been working on the Ethereum network in various capacities since the mainnet launched.