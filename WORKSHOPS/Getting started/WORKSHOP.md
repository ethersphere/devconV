# Swarm for dApp Developers (Basic)

## Requirements for optional steps

1. Install `jq` - Homebrew or other package manager `brew install jq`

2. Install `websocat` - https://github.com/vi/websocat/releases


## Download Swarm

Download the `0.5.2` version of Swarm:

- [MacOS](https://ethswarm.blob.core.windows.net/builds/swarm-darwin-amd64-0.5.2-d8b56ff.tar.gz)
- [Linux 64bit](https://ethswarm.blob.core.windows.net/builds/swarm-linux-amd64-0.5.2-d8b56ff.tar.gz)
- [Windows 64bit](https://ethswarm.blob.core.windows.net/builds/swarm-windows-amd64-0.5.2-d8b56ff.zip)


## Start your Swarm node

Start `swarm` (0.5.2 or newer) with the following CLI flags:

Note: It will ask your for a passphrase. You can use whatever you want, even an empty one by just pressing 'enter'.

```bash
$ ./swarm \
  --datadir $PWD/data \
  --ens-api test:e7410170f87102df0055eb195163a03b7f2bff4a@https://rinkeby.infura.io/v3/b41676eda6c749768d51c615fc57bc6c \
  --ws --wsorigins='*' --wsapi=admin,net,debug,bzz,stream,accounting,swap \
  --debug \
  --verbosity 3 \
  --sync-mode push \
  --bootnodes enode://30d8e3ab25ab1fc4bc020f79048d5d138f1362269158a3f2b8e69a242c59155376d8868747869f10a6994d882cf34d45536d90aa9a51041e1f76796e3cb97b1a@192.168.0.12:30300 \
  --enable-pinning \
  --bzznetworkid 61


## One liner (if you're having problems copying the previous command)
$ ./swarm --datadir $PWD/data --ens-api test:e7410170f87102df0055eb195163a03b7f2bff4a@https://rinkeby.infura.io/v3/b41676eda6c749768d51c615fc57bc6c --ws --wsorigins='*' --wsapi=admin,net,debug,bzz,stream,accounting,swap --debug --verbosity 3 --sync-mode push --bootnodes enode://30d8e3ab25ab1fc4bc020f79048d5d138f1362269158a3f2b8e69a242c59155376d8868747869f10a6994d882cf34d45536d90aa9a51041e1f76796e3cb97b1a@192.168.0.12:30300 --enable-pinning --bzznetworkid 61

```


## (Optional) Check Swarm peers in your Kademlia table

Run WS API request (requires `jq` and `websocat`)

```b
$ echo '{"jsonrpc":"2.0","method":"bzz_hive","id":1}' | websocat ws://127.0.0.1:8546/ -n --one-message --origin localhost | jq .result | xargs printf

=========================================================================
commit hash: c1c233d179470661262ff22378dbe4f524c8c5f3
Wed Oct  2 11:40:29 UTC 2019 K???MLI? hive: queen's address: c661b1e8f6333565bff54ac54d7116b4b663a104f6bf878b2aad3d949ed437a7
population: 9 (9), NeighbourhoodSize: 2, MinBinSize: 2, MaxBinSize: 4
000  5 1da7 13c9 1348 5242          |  5 1da7 (0) 13c9 (0) 1348 (0) 5242 (0)
001  2 87f6 8b97                    |  2 87f6 (0) 8b97 (0)
============ DEPTH: 2 ==========================================
002  0                              |  0
003  0                              |  0
004  1 c9a7                         |  1 c9a7 (0)
005  1 c1d6                         |  1 c1d6 (0)
006  0                              |  0
007  0                              |  0
008  0                              |  0
009  0                              |  0
010  0                              |  0
011  0                              |  0
012  0                              |  0
013  0                              |  0
014  0                              |  0
015  0                              |  0
=========================================================================

```

Alternatively try opening http://localhost:8500/bzz:/ac1ab955bf318503ada4c40996199bc3040505b77f911fae0115c48b2bfd010f/ which has a web UI similar to the one above.


## Upload a file

Pick a file (up to 30MB) and upload it with `swarm up`. If you want to see a progress bar, you have to also provide the `--progress` flag. 

E.g. Take a screenshot or upload some cute animal picture.

```
$ ./swarm up --progress ~/Downloads/Big_Buck_Bunny_720_10s_30MB.mp4

Swarm Hash: 8264ca38c2d6c84955dd8dd3fa68d8411b2b34a447901bb59ee7b959110544d5
Tag UID: 2451023057
Upload status:
Syncing 7894 chunks       0s [============================================================>-] 98 %
Done! took 8.367633s
Your Swarm hash should now be retrievable from other nodes!
```

## Download the file

1. Download your file from your local node, either via your browser or on the CLI:

```
wget http://localhost:8500/bzz:/8264ca38c2d6c84955dd8dd3fa68d8411b2b34a447901bb59ee7b959110544d5/
```

2. Share the hash with your colleagues so that they can download it via their node.


## Upload and pin a file

Simply use the `--pin` flag when using the `swarm up` command.


```
$ ./swarm up --pin your_file.mp4
```


## (Optional) Pin content after upload

You can pin any content, as long as it exists on your local node. Make sure that you access the content before you pin it.

You can pin existing content with a HTTP POST request to `http://localhost:8500/bzz-pin:/<hash>`

For example, using curl:

```
$ curl -X POST http://localhost:8500/bzz-pin:/<hash>
```


## List pinned content

You can list all your pinned content via the a HTTP GET request on:

http://localhost:8500/bzz-pin:/

For example, using curl:

```
$ curl -X GET http://localhost:8500/bzz-pin:/
```


## Remove pinned content

You just have to perform an HTTP DELETE request on `http://localhost:8080/bzz-pin:/<hash>`

For example, using curl:

```
$ curl -X DELETE http://localhost:8500/bzz-pin:/<hash>
```


## Upload a website

1. Download a templated HTML/CSS from https://startbootstrap.com/themes/sb-admin-2/ if you don't have a website

2. Unzip and modify the HTML

3. Go to the folder

```
$ ./swarm --defaultpath index.html --recursive up --progress .
```


## Using bzz-raw

Tip: Try using bzz-raw on the hash you've got when you uploaded the website.

```
$ curl http://localhost:8500/bzz-raw:/<reference hash>/

{
    "entries":
    [
        {
            "hash":"0a3235673fe6af3c28e9721334087946b4731e9441cb558012c7d668d4098575",
            "contentType":"image/png",
            "mode":436,
            "size":457290,
            "mod_time":"2019-10-01T14:35:44+02:00"
        }
    ]
}
```


## Using ENS with Swarm

1. Create account on Metamask on Rinkeby testnet.

2. Get Rinkeby ETH via faucet, or ask someone to send Rinkeby ETH to you.

3. Claim a .test ENS name via ENS Manager at https://manager.ens.domains

4. Use Public Resolver via ENS Manager with your claimed .test ENS domain name

5. Add Content Record to Domain via ENS Manager, for example bzz://ref-hash/

6. Open your domain in your browser, for example http://localhost:8500/bzz:/basicworkshop.test/


## Using Encryption

Encrypt a file before your node syncs it to the network.

```
$ ./swarm up --encrypt ~/Desktop/secret-file
```

Note: The returned swarm hash is bigger than usual. This is because the decryption key is now part of that hash.


## Access Control with Password

```bash
$ ./swarm access new pass <encrypted-reference>

# Or you can also provide a password as a file
$ echo "supersecretpassword" > password.txt
$ ./swarm access new pass --password password.txt <encrypted-reference>
```

## Q/A session

Any questions?


## Bonus: Access Control with Elliptic curve keys (single grantee)

[Docs](https://swarm-guide.readthedocs.io/en/latest/dapp_developer/index.html#selective-access-using-ec-keys)


## Bonus: Access Control with Elliptic curve keys (multiple grantees)

[Docs](https://swarm-guide.readthedocs.io/en/latest/dapp_developer/index.html#selective-access-using-ec-keys)
