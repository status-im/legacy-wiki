本文檔概述了改進我們測試策略的選項。

測試原因：以可重現的方式盡快提供關於狀態質量的反饋。 （重要的是：反饋，快速，可重複）

質量狀態 (*attributes/dimensions*)
功能：它工作嗎？它是否適用於特定的設備/操作系統？如果它起作用，則其他質量屬性：例如性能;可用性等

手動測試（檢查PR中的新功能，使用有限的設備覆蓋範圍進行開發構建的回歸和集成/系統測試，查看新的UI設計實施前）自動UI測試涵蓋基本功能（快速獲得煙霧覆蓋範圍，測量性能，電池/內存/網絡消耗，在雲端平台上的真實設備上運行測試）社區手動測試（額外設備/操作系統/語言覆蓋，測試開發建立，對性能/內存/網絡使用的反饋，審查新的UI？）防止錯誤：審查新的故事/功能，用戶界面設計和描述測試
- ＆GT;在實施開發人員之前知道將檢查什麼。

## 單元測試

驗證純功能的簡短和快速測試。可以經常運行。基於`cljs.test`。適當的時候`test.check`是查找不明顯的問題的好工具。
可以使用`re-frame-test`來編寫特定於`re-frame`的更高級別測試。
每個PR都應該測試新的/更新的功能。高覆蓋率不是目標，而是基本功能和極限情況。

## 集成測試

在頻譜的另一端，可運輸應用程序用更複雜的用戶場景進行測試。這些測試也將用ClojureScript編寫，以利用現有的代碼庫。
`Appium`提供了一個我們可以利用的`nodejs`層

## 非回歸測試

引入性能和升級測試以確保新版本不會打破客戶的期望可能是有用的。

## 值得調查

  - <https://github.com/pixielabs/cavy>
  - <https://github.com/wix/detox>

## 進一步討論

**TODO:**還要從相關要點中導入相關評論
[這裡](https://gist.github.com/jeluard/4564c04f118b1cfa828ffe741811b0aa)

[Category:Status Application](Category:Status_Application "wikilink")
[Category:Developer
Documentation](Category:Developer_Documentation "wikilink")