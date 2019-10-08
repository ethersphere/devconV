
# Intro
Dear participant of the Swarm incentives workshop. Thank you for being here and congratulations on being one of the first to try out Swarm with the Swarm Accounting Protocol (SWAP) enabled! The Swarm team is very proud of the work which you are about to test. We think that this feature hallmarks the start of a truly scalable and censorship-resistant Swarm.
SWAP works by compensating you for the chunks served to your peers, while you have to compensate your peers for chunks served to you. All of this, you will be able to witness by yourself soon, but first: let’s talk about the setup of the workshop!

During the workshop, you will be able to earn badges for every achievement. Namely:

1. Deploy a chequebook
2. Cash a cheque
3. Have a cheque been cashed

The rest of the handout walks you through all steps needed to install Swarm, run it and earn those badges!

## Structure of the handout
This handout consists of 3 chapters, one for every badge. In every chapter, you will find instructions on how to achieve the badges as well as background information, for those wishing to know more.

# 0. Prerequisites
To download Swarm, you can copy the GitHub repo and compile the source code (required git and go). If you don’t have git and go please proceed with installing the [binaries](http://192.168.55.103:9999).

# 1. Deploy a chequebook
In order to earn this badge, you could theoretically just copy the source code of the chequebook and deploy it, but that is not what we intended! We would like you to start Swarm with SWAP enabled, which will automatically deploy a chequebook for you if it is the first time you boot up Swarm.

## Earn the badge
To run Swarm with SWAP enabled, simply run: 
`./swarm`
With the command-line options:
```
--swap --swap-backend-url http://192.168.55.102:8545 --bzznetworkid 5
```


The first time you will do this, it will:
1) create an Ethereum account for you and stores this encrypted in the folder `.ethereum/keystore` (linux), `~/Library/Ethereum`, `/Appdata/Roaming/Ethereum` (Windows)
2) attempts to deploy a chequebook contract for you from the account which you just created.

Deploying a chequebook will fail because your newly created account does not have any Testnet Ethers. You can get some from the faucet at http://192.168.55.102.
Now, run the command again, but be sure to send at least 0.1 testnet ether together with the deployment. This is 1 with 17 zeroes (100000000000000000) Wei. If you don’t send sufficient funds, you won’t be able to achieve all the badges. 

Congratulations on earning the first badge!

## Extra info
<details>
<summary>About the chequebook</summary>

### About the chequebook
A chequebook contract is the pounding heart of the Swarm; via the chequebook we can send and receive payments. A chequebook allows you to send and receive payments without doing an on-chain transaction. It works very similar to how cheques work in real life: as a debitor (person who pays), you can write cheques to a creditor. The creditor can cash-in this cheque at a later point in time at the bank chequebook contract. It has some more features as well, such as reserving part of the balance for one specific peer (called hard deposit) and the possibility of cashing out a cheque for other peers (let’s help those poor people without Ether!). You can have a look at the source code for the chequebook [here](https://github.com/ethersphere/swap-swear-and-swindle/blob/master/contracts/SimpleSwap.sol). 
</details>

<details>
<summary>Used command-line options</summary>

### Used command-line options
In the previous session, we used several command-line options. Those are minimally needed to connect to Swarm with SWAP enabled. Curious what they mean? Run:

`./swarm -help`

As you can read, the used options have the following descriptions:

**swap**: *Swarm SWAP enabled (default false)*
This option shows that you want to use swarm with the Swarm Accounting Protocol enabled

**swap-backend-url**: *URL of the Ethereum API provider to settle SWAP payments*
Swarm uses Ethereum (at the moment) to settle payments. Via the URL specified after this flag, you tell Swarm how to connect to Ethereum.

**bzznetworkid**: *Numerical network identifier. The default is the public swarm testnet (4)* 
Swarm with SWAP enabled is currently only allowed on bzznetworkid 5
</details>
<details>
<summary>Logs to pay attention to</summary>

### Logs to pay attention to
When running Swarm with SWAP enabled, a couple of interesting logs pass by. We sum up those worth paying attention to:

`Connecting to SWAP API. url=http://192.168.55.102:8545...`
Swarm tells you it is attempting to make a connection to the blockchain which you specified as a command-line option (swap-backend-url). It does so by pinging and waiting for a response. 

`Using backend network ID. ID=14191 `
Every Ethereum network has a network ID (set in the Genesis block). This identifier is used to make sure that a transaction send on mainnet (for example) is invalid on a testnet. 4 stands for Rinkeby.

`Deploying new swap. owner=0x..., deposit=value`
Swarm tells you it attempts to deploy a chequebook for you. If you don’t have Ether, it will try 5 times before aborting the boot-up sequence. If you do have Ether, you might have to wait for some time, but eventually, you will get the next log, telling you that the chequebook was deployed! `owner` and `deposit` are both arguments that can be given to the constructor of the chequebook contract. `owner` is the account that is allowed to write cheques from this smart-contract (this should be equal to the bzz account which you created before), `deposit` is the amount (in Wei) which will be sent along with the deployment transaction and which is the underlying value for the cheques you are going to write soon!

`Deployed chequebook. Contract address=0x…, deposit=value, owner=0x…`
Tells you the chequebook is deployed at the given address. You can view that this is indeed the case at `https://rinkeby.etherscan.io/address/<your_chequebook_address>`. 

`Bound to chequebook. chequebookAddr=0x...`
You won’t see this log the first time you start up Swarm. It will only appear when you boot up Swarm the second time, or when you do it the first time with a `--swap-chequebook` flag.
</details>

# 2.Cash a cheque
In order to earn this badge, all you have to do is to sit back and relax (almost literally!). When you boot up Swarm with SWAP enabled, content is automatically synced to your node and when this content is requested by other peers, you will be compensated for this. The only thing you have to do is to join a network. Synching of content will happen automatically and eventually, you will be asked to serve this content for which you will be compensated!

## Earn the badge
Previously, you boot up Swarm with SWAP enabled, but you did not connect to any peer.

Connecting to a network is done by telling to which bootnode you want to connect and as we are running a private network for the sake of this workshop, you have to pass in this (non-standard) bootnode.
If you are still running Swarm, shut down this process and start it again, now specifying a SWAP-enabled bootnode:

```
./swarm \
--swap \
--swap-backend-url http://192.168.55.102:8545 \
--bzznetworkid 5 \
--bootnodes "enode://9b7571c26d50bed78f614be5bf3b2d661176fdfeb546f100b84dd03545f4bc98e42e640286ac92fe110ec5f4995141743e47d8f642aa49ac05bd5f2cab2e881a@192.168.55.102:30399" \
--verbosity=4
```

That’s all. If the network you just joined is actively used, you will be receiving cheques automatically. The only problems is... The network you are on is not by default actively used; You and other participants of the workshop need to create traffic! To help out your neighbor earning this badge (let’s assume reciprocity, so you will eventually earn your badge as well), please proceed to the next section (“Have a cheque been cashed”).

##  Extra info
<details>
<summary>Monitoring your process</summary>

## Monitoring your progress
Connect to the user interface
In the previous section, you connected to a Swarm network. You probably have seen messages passing by such as
Adding p2p peer and handshake. This means the protocol is running and you are being connected to the network. While this is nice, you probably want to see something more visual, right? To do this, we need to run boot up Swarm with a couple of more flags. Specifically, the added flags will enable our backend to accept websocket requests from external servers. In layman's terms: it needs to allow our website to read data from the running Swarm instance. Run:

```
./swarm \
--swap \
--swap-backend-url http://192.168.55.102:8545 \
--bzznetworkid 5 \
--bootnodes "enode://9b7571c26d50bed78f614be5bf3b2d661176fdfeb546f100b84dd03545f4bc98e42e640286ac92fe110ec5f4995141743e47d8f642aa49ac05bd5f2cab2e881a@192.168.55.102:30399" \
--verbosity=4 \
--ws \
--wsaddr=0.0.0.0 \
--wsapi=accounting,bzz,swap \
--wsorigins="*"
```
Now, navigate to https://swarm-monitor.netlify.com/ 
</details>

<details>
<summary>Accounting-enabled Messages</summary>

## Accounting-enabled Messages
Currently, there are two messages which trigger the Swarm Accounting Protocol. These are:


1) Retrieve request
2) Chunk delivery

A retrieve request is sent out when you request a certain chunk from a peer. Retrieve requests are sent when:

1) you, the user, wants to download content from Swarm
2) your node has got a retrieve request from another node and forwards this request because it does not have the content itself.


Chunk delivery messages are sent as an answer to a retrieve request.
</details>

</details>

<details>
<summary>Cheques</summary>

## Cheques
A cheque has the following fields:

- Contract (the contract address the cheque can be presented to)
- Beneficiary (the bzz address of the beneficiary)
- Cumulative payment (the cumulative value of all cheques ever sent to this beneficiary)
- Signature (a digital signature on the 3 fields above)

With these four pieces of data, the beneficiary has enough data to successfully cash out the cumulative payment amount minus what has been cashed out previously (the worth of the cheque). When a node receives a cheque, the digital signature is verified and if this is correct, the balance of the peer who sent the cheque increases by the worth of the cheque.
The beauty of a cheque lies in the fact that the sum is always cumulative; this allows nodes to collect cheques and only ever cash-out the last-presented cheque with the highest cumulative amount. This obviously saves a lot on transaction costs—something that is highly needed in the context of Swarm. 
</details>

<details>
<summary>Payment threshold and the chunk-o-meter</summary>

## Payment threshold and the chunk-o-meter
The Swarm Accounting Protocol accounts for balances with your peers
The payment threshold is defined as the amount of honey (Swarms internal accounting unit) at which your peer will initiate payments to other peers. The payment threshold is set in such a way that, in case of equal consumption of chunks, no cheques need to be sent. In case of unequal or high variability of consumption, peers will automatically start compensating each other once the chunk-o-meter tilts too much to one side.
![](https://www.rifos.org/wp-content/uploads/2019/07/5.-swap.gif)
</details>

<details>
<summary>Used command-line options</summary>

## Used command-line options
On top of the command line options which were previously introduced, we used several extra command-line options. These were:

**bootnodes:** `Comma separated enode URLs for P2P discovery bootstrap`
This is the enode-address of the bootnode to which you are connecting. An enode address is in the form: enode://<identifier>@<ip_address>:<port_number>

**verbosity:**  *Logging verbosity: 0=silent, 1=error, 2=warn, 3=info, 4=debug, 5=detail*
We set this value to 4, meaning it will log debug messages, additionally to info, warn and error messages which are logged by default. We set this value to 4 such that you can see the debug messages telling you that your Swarm instance is connecting to other peers.

The flags below are needed to allow our user interface: 
**ws:** Enable the WS-RPC serverEnable the WS-RPC server

**wsaddr:** WS-RPC server listening interface (default: "localhost")

**wsapi:** API's offered over the WS-RPC interface

**wsorigins:** Origins from which to accept websockets requests 
</details>

# 3. Have a cheque been cashed
In order to send a cheque, you have to use more resources than your peer. As was previously explained, accounted messages are retrieve request and chunk delivery messages. Let’s trigger those messages to be send and request some chunks!

## Earn the badge
Requesting content is easiest done through the Swarm portal. Go to your browser and navigate to [localhost:8500](localhost:8500)

If you have your Swarm instance running, you should be able to see a bar where you can request content. In Swarm, all content is content-addressed, meaning that the location of the file is based on the content of the file. Specifically, you have to request the root hash of a file. Please request: `6875e293bb58afd0f533a39024eafff1b1defdcba9b10861fcb7913e77dcbb7e` (a pleasant surprise awaits you!)
**TODO: upload picture and change hash**

Did you already send a cheque? If not, you need to download something else as well (just re-downloading won’t trigger more SWAP payments, as the content is cached). You can upload any content by typing in a new terminal:

`./swarm up <path_to_your_file`

After executing this command, you will get the hash of your file, which you can share with your peers. Please use the provided [HackMD](https://hackmd.io/@s-RC5CxwQ4in063_UXoQRg/rJqXJXDOS/edit) to quickly share workshop-related data with your peers.
