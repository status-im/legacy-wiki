# Jail

這是一個單元經理。它跟踪應用程序執行期間創建的所有單元。它還使用JavaScript代碼創建和初始化單元格。

# Cell

一個單元格是帶有一些實用程序的JavaScript執行上下文。每個單元格都由`Jail`使用`Jail.Parse`方法進行初始化。

Cell的JS執行上下文使用`web3`（https://github.com/ethereum/web3.js）和任何提供給`jail.Parse`的JS代碼進行初始化。

`web3`用一個名為`jeth`的提供程序進行初始化，並在`jail.Parse`中的Go代碼中構建。因此，它定義了`web3`和`status-go`之間的橋樑。

`jeth`作為提供者，必須實現`send`和`sendAsync`方法。它們都在`registerHandlers`函數中註冊。實際上，當`jeth.sendAsync`調用`jail.Send`時，`jeth.send`會調用<code>
jail.Send \</ code\>，但在一個Goroutine。結果要通過回調來返回。

例如，讓我們來看看執行同步調用`web3.eth.blockNumber`時會發生什麼：

1.  `web3`將此調用轉換為JSON-RPC方法`eth_blockNumber`,
2.  提供程序用於進行HTTP或IPC調用：`jeth.send`，,
3.  `jail.Send`被有效地調用，因為它在`jeth.send`實現中,
4.  `rpc.Client.CallContext`用方法和params調用，
5.  響應返回給JS代碼。

[Category:Status-go](Category:Status-go "wikilink")