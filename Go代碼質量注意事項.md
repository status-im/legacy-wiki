## 動機

> *“Any fool can write code that a computer can understand. Good
> programmers write code that humans can understand.” – Martin
Fowler*

在編寫代碼時，我們一定會牢記功能和非功能需求的實現。所以我們經常關注如何快速並且享受複雜的解決方案。但是，隨著代碼的增長，團隊也在不斷增長，發現錯誤並且必須修復，並且影響現有代碼的新需求會進入我們的產品中。所以我們必須記住，我們的代碼將被讀取，並且稍後必須被其他人理解。時刻記住你在嘗試進入一個複雜且糟糕的可維護代碼庫時的感受，並儘力避免這種情況。

## 通用標準和公約的介紹

以下外部文件涵蓋了編寫優質代碼的技術方面以及常見的編碼規範。特別是Google自己的約定對我們的代碼是強制性的。

  - Google關於高效Go和編碼約定的最重要文檔是 [Effective
    Go](https://golang.org/doc/effective_go.html)
  - 補充 *Effective Go* 是 [Go Code Review
    Comments](https://github.com/golang/go/wiki/CodeReviewComments)
  - 對於新的Go開發者s *Kyle Quest* 寫[50 Shades of Go: Traps, Gotchas, and
    Common Mistakes for New Golang
    Devs](http://devs.cloudimmunity.com/gotchas-and-common-mistakes-in-go-golang/)

## 特定Status約定

### 評論

  - 可以查看[GoDoc](https://godoc.org/github.com/status-im/status-go)中生成的Web文檔，您可以看到應該添加或更好地制定多少文檔。
  - 因此，至少要添加關於公共包和文檔評論的評論，如[commentary](https://golang.org/doc/effective_go.html#commentary)中所述。特別是不僅要考慮一個小句子作為輸入句子，而且要考慮更多有關共同性和動機的語義信息。
  - 比評論更好的是，只有公共標識符才會評論私人標識符以幫助代碼維護者。
  - 在函數和方法的內部，單獨的部分應該引入一個支持代碼內部方向的幫助評論。
  - 當離開TODO評論時，請添加您的Slack暱稱（`//
    TODO（themue）測試退出條件與極端值。`）。這有助於其他人員在需要時聯繫適當的人員進行闡述。

### 測試

  - 單元測試使用[Testify](https://godoc.org/github.com/stretchr/testify)完成。它基於標準測試庫，但更加靈活和冗長。
  - 測試應該測試你期望他們測試什麼，不應該測試不相關的代碼。
  - 測試功能最好以arrange/act/assert格式構成。 這將統一測試並幫助在其內部進行導航。

## 程序約定

### 提交

  - 何時情況下要做 `git pull --rebase` (請看 [Git - When to Merge vs. When to
    Rebase](https://www.derekgourlay.com/blog/git-when-to-merge-vs-when-to-rebase/))
  - 保持提交消息的重點 (請看 [How to Write a Git Commit
    Message](https://chris.beams.io/posts/git-commit/))

### 拉取請求

  - 合併[develop](https://github.com/status-im/status-go)分支後自行查看您的代碼，並確保一切都取決於您的代碼質量欄
  - 確保構建 [Travis](https://travis-ci.org/status-im/status-go/branches)成功
  - 確保測試通過
  - 確保`go fmt` `go lint`，`go
    vet`不會產生任何警告;修復警告或在需要時添加忽略規則
  - 如果您有權創建分支，請確保您從同一個存儲庫發送拉取請求
  - 如果您發送拉取請求以徵求建議，請明確地向評論者說明，以便他或她僅查看回答問題的必要條件

## 舊的

*將被重寫和遷移*

### 建議

  - 使用`testify`進行測試，它比默認的測試庫更詳細。
  - 測試應該測試你期望他們測試什麼，不應該測試不相關的代碼。狀態(Status)中的大量測試需要重構。
  - 測試優選採用`Arrange-Act-Assert`格式。它為他們帶來了一個統一的結構，並創造了更好的導航捕眼器，例如：這可能看起來像頭頂上的，但它帶來了結構，迫使你為不同的檢查編寫不同的測試，並長期證明富有成效。

<!-- end list -->

``` go
  func (s *JailTestSuite) TestParse() {
      // Arrange.
      require := s.Require()
      extraCode := `
      var _status_catalog = {
          foo: 'bar'
      };
      `

      // Act.
      response := s.jail.Parse("newChat", extraCode)

      // Assert.
      expectedResponse := `{"result": {"foo":"bar"}}`
      require.Equal(expectedResponse, response)
  }
```

從`@anna`
> (*質量保證*):

> 順便說一句，我還喜歡Jan的風格在PR摘要內容中進行修復，例如https://github.com/status-im/status-react/pull/1509，它解釋了原因和解決方法。所以，我可以在驗證錯誤的同時獲得更多的見解。它可能不適用於所有的修補程序，但我會很感激，如果有一些關於如何修復bug的說明。我不會查看每個PR更改的文件，也不會了解代碼中的所有內容，因此一些摘要很有用。

[Category:Status-go](Category:Status-go "wikilink") [Category:Developer
Documentation](Category:Developer_Documentation "wikilink")