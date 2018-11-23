# 基本信息

  - 我們依賴於正在大力發展中的“以太坊`go-ethereum`項目”
  - 我們有我們自己的分支 `go-ethereum` repo:
    <https://github.com/status-im/go-ethereum>
  - 我們有時需要從上游提取代碼 ( `go-ethereum` 是我們的分支 )
  - 除了從上游提取更改外，我們還需要應用我們的自定義提交 (我們在<code> go-ethereum \</
    code\>項目的頂部需要一些自定義功能):

![f1f7ccae-f84b-11e6-9e36-201984db8b64.png](f1f7ccae-f84b-11e6-9e36-201984db8b64.png
"f1f7ccae-f84b-11e6-9e36-201984db8b64.png")

# 改編(Rebase)工作流程

我們從穩定分支開始（假設<code> a07539fb \</ code\>指向我們想要基於的上游<code> go-ethereum \</
code\>的起點）：

`git co -b status/1.5.9 a07539fb`

現在，我們需要應用我們的自定義提交（它們通常在我們以前的分支中可用，比如說 `status/1.5.8-stable`)

`git cherry-pick "start^..end"` (或者你可以挑取( cherry pick
)一個版本然後再接一個的提交，比方說，如果你想省略一個)

當挑取( cherry pick )時我們將會要處理 **衝突 (conflicts)**.
事實上，我們的自定義更改與以前的版本兼容，並且新版本可能會改變某些內容。衝突解決後，確保運行make編譯沒有任何問題。

在`go-ethereum`儲存庫(repository) repository 裡從`status/1.5.9`中創建一個PR去
`develop` .合併後，是更新<code> status-go \</ code\>來源文件夾的時候了。

到`status-go`儲存庫(repository). 執行 `dep ensure --update
github.com/ethereum/go-ethereum)`. 這會將供應源版本更新為最新更新。

試著做 `status-go`. 它會失敗的機會。

如果失敗，您需要修復必要的回購問題，並提交更改`供應源: 修補程序修復 (vendor: rebase fixes)`.

現在您已準備好向提交初次文件(code files)到 `status-go` (其中將包含2個提交\!)

[Category:Status-go](Category:Status-go "wikilink")