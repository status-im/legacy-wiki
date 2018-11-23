我们依赖于`go-ethereum`，虽然该项目被加载为`vendor/github.com/ethereum/go-ethereum`，但它实际上来自\[http：//github.com/status-im/go-ethereum`github.com/status-im/go-ethereum`\]。

这个想法是在`[github.com/status-im/go-ethereum]（http://github.com/status-im/go）中保留对go-ethereum的所有本地更改-ethereum)`，并将它们记录在桉，并有一个特殊的过程来实现这一点：

1.  在[`status-go`](http://github.com/status-im/status-go)中像往常一样进行更改，包括供应文件夹中的`go-ethereum`。
2.  克隆[`github.com/status-im/go-ethereum`](http://github.com/status-im/go-ethereum)
    并检查出最新的分支 `status/1.7.2-stable`.
3.  从`status-go` 供应文件夹複製你的 `go-ethereum`文件夹到新的克隆`go-ethereum`.
4.  看看`git
    diff`，并确保只有你打算做的改变。
5.  承诺并推动新的变化。
6.  修改https://github.com/status-im/go-ethereum/wiki/Rebase-Geth-1.7.0以便其他人知道提交的内容。

[Category:Status-go](Category:Status-go "wikilink")