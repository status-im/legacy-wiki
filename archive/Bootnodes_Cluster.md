Although Status is a decentralised application, it uses a cluster hosted
on [scaleway.com](http://scaleway.com/) which serves multiple purposes
described below.

# Goals

## Mine Ropsten ether

We have our own faucet sending ether on demand inside the application.
This ether is mined by our cluster.

## Provide “bootnodes” for mobile clients

A bootnode from the Geth point of view is a stripped node which only
participates in discovery (see
<https://ethereum.stackexchange.com/questions/12503/what-is-the-bootnode-command-in-the-go-ethereum-documentation>).

From the Status point of view, a bootnode is what helps a mobile node to
get connected to other nodes AND where it can get blocks from. So,
instead of being just a facility for discovering other nodes, nodes in
our cluster also share their blocks to the requestor.

Such solution makes blockchain synchronisation of mobile nodes faster by
making faster discovery. However, it only makes sense if the mobile node
runs its local Ethereum node instead of using the remote (see about
remote nodes: TBD).

## Relay whisper messages with low PoW

As mobile nodes have limited computational power, we set PoW to a value
that is lower than the geth’s default value. This makes regular nodes
unable to relay these messages and we allow cluster handle that.

# Operation

Enodes are baked into mobile binaries and can’t be changed without
re-compiling. Specifically, their addresses may be found at
<https://github.com/status-im/status-go/blob/develop/static/config/cht.json>
along with CHT (Canonical Hash Trie) Ethereum block numbers.

Actual cluster deployment configuration can be found at
<https://github.com/status-im/status-cluster>