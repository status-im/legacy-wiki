我們依賴於`go-ethereum`，雖然該項目被加載為`vendor/github.com/ethereum/go-ethereum`，但它實際上來自\[http：//github.com/status-im/go-ethereum`github.com/status-im/go-ethereum`\]。

這個想法是在`[github.com/status-im/go-ethereum]（http://github.com/status-im/go）中保留對go-ethereum的所有本地更改-ethereum)`，並將它們記錄在案，並有一個特殊的過程來實現這一點：

1.  在[`status-go`](http://github.com/status-im/status-go)中像往常一樣進行更改，包括供應文件夾中的`go-ethereum`。
2.  克隆[`github.com/status-im/go-ethereum`](http://github.com/status-im/go-ethereum)
    並檢查出最新的分支 `status/1.7.2-stable`.
3.  從`status-go` 供應文件夾複製你的 `go-ethereum`文件夾到新的克隆`go-ethereum`.
4.  看看`git
    diff`，並確保只有你打算做的改變。
5.  承諾並推動新的變化。
6.  修改https://github.com/status-im/go-ethereum/wiki/Rebase-Geth-1.7.0以便其他人知道提交的內容。

[Category:Status-go](Category:Status-go "wikilink")