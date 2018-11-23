# Jail

这是一个单元经理。它跟踪应用程序执行期间创建的所有单元。它还使用JavaScript代码创建和初始化单元格。

# Cell

一个单元格是带有一些实用程序的JavaScript执行上下文。每个单元格都由`Jail`使用`Jail.Parse`方法进行初始化。

Cell的JS执行上下文使用`web3`（https://github.com/ethereum/web3.js）和任何提供给`jail.Parse`的JS代码进行初始化。

`web3`用一个名为`jeth`的提供程序进行初始化，并在`jail.Parse`中的Go代码中构建。因此，它定义了`web3`和`status-go`之间的桥樑。

`jeth`作为提供者，必须实现`send`和`sendAsync`方法。它们都在`registerHandlers`函数中注册。实际上，当`jeth.sendAsync`调用`jail.Send`时，`jeth.send`会调用<code>
jail.Send \</ code\>，但在一个Goroutine。结果要通过回调来返回。

例如，让我们来看看执行同步调用`web3.eth.blockNumber`时会发生什麽：

1.  `web3`将此调用转换为JSON-RPC方法`eth_blockNumber`,
2.  提供程序用于进行HTTP或IPC调用：`jeth.send`，,
3.  `jail.Send`被有效地调用，因为它在`jeth.send`实现中,
4.  `rpc.Client.CallContext`用方法和params调用，
5.  响应返回给JS代码。

[Category:Status-go](Category:Status-go "wikilink")