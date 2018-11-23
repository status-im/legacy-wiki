### Whisper V5

Whisper is a kind of gossip protocol.

Main properties: the dark. It\`s impossible to say what user reads or
whiter what messages. All messages resend by each peer in the network.

Mail nodes were introduced. It works as a trusted node that can resend
messages to a user as p2p messages even if it\`s TTL is expired.

There\`re two types of messages: direct p2p and \`normal whisper
messages\`. In Status, we use only last ones.

We have PoW changed from 0.2 to 0.001 to make messages faster. But it
causes a few issues: Status network can be DdDSe-d and Status messages
are dropped by other Ethereum clients (Geth, Parity etc).

Traffic: Each user gets all messages in network \* peers_number

### Whisper V6

Mail goal: the good tradeoff between keeping darkness and traffic
reduction.

Full and light nodes were introduced. Full node has \`nil\` or all bytes
1 Bloom filter. Light node has some different bloom filter.

Bloom filter has many collisions per topic by design to not to share
user\`s data.

A bloom filter doesn\`t filter direct p2p messages.

Traffic: the full node\`s behavior is exactly the same as in whisperV5.
So it consumes all messages in network \* peers_number

### Where we are now

We use whisperV5 and switching to whisperV6. All application nodes
should be light whisper nodes with zero-ed bloom filter by default to
not to receive any messages.

We have single \`status\` topic for all messages in 1-1 and group chats.

We haven\`t a restriction to a number of whisper peers. So it could be
up to 25 and all traffic will be multiplied by 25.

Mail nodes:

At the start of the application, we request all messages from mail node.
It causes CPU and network overhead.

We

### want

Make Mail nodes more useful for Status application:

1\. Request messages only by offline time period

2\. Request messages by topics

3\. Request only messages that user didn\`t receive yet -
<https://github.com/status-im/status-go/issues/690>

4\. Find a way to scale mail nodes

Reduce traffic per user per day up to 5Mb