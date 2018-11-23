虽然Status是一个分散的应用程序，但它使用\[[http://scaleway.com/calendar.com\]上託管的集群，该集群可用于下述多种目的](http://scaleway.com/calendar.com%5D上託管的集群，该集群可用于下述多种目的)。

# 目标

## 开挖Ropsten的以太币

我们有自己的发测试用以太币的服务(faucet)在应用程序内按需发送以太币。这个以太币是由我们的集群开采的。

## 为移动客户端提供“bootnodes”

从Geth的角度来看，bootnode是一个只能参与发现的剥离节点（请参阅https://ethereum.stackexchange.com/questions/12503/what-is-the-bootnode-command-in-the-go-ethereum
-documentation）。

从状态的角度来看，bootnode可以帮助移动的节点连接到其他节点，并且可以从中获取数据块。因此，我们不仅仅是发现其他节点的工具，我们集群中的节点也将它们的块共享给请求者。

这种解决方桉通过加快发现速度，使移动节点的区块链同步更快。但是，如果移动节点运行其本地以太坊节点而不是使用远程节点（请参阅有关远程节点：TBD），则仅有意义。

## 中继耳语(whispe)讯息与低工作量证明

由于移动节点的计算能力有限，我们将PoW设置为低于geth默认值的值。这使得常规节点无法中继这些消息，并且我们允许群集处理该消息。

# 运作

Enode被崁入移动的二进製文件中，无需重新编译就无法更改。具体来说，他们的地址可以在https://github.com/status-im/status-go/blob/develop/static/config/cht.json以及CHT（Canonical
Hash Trie）以太坊数据块中找到。

实际的集群部署配置可以在https://github.com/status-im/status-cluster找到

[Category:Status-go](Category:Status-go "wikilink")