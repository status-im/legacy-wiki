本文檔介紹了在Github中標記問題的方法 [Status.im](http://status.im/) 儲存庫 `status-react`
和 `status-go`.

# **標籤**

## **問題類型**

`bug` - 錯誤報告的標籤

`Epic` - 這些epic問題有助於將與高級功能或需求相關的問題分組

`test` - 與創建或更新代碼的測試相關的問題

`story` - 用戶故事描述了流程的特定特徵

## **優先級**

`high-priority`, `medium-priority`, `low-priority` -
有助於確定問題的優先順序，並將重點放在首要任務上。

## **複雜度**

`beginner` - 這個問題可以由新手開發人員解決，並且不需要完全理解現有的代碼庫和依賴關係。

`advanced` - 該問題需要專家對現有代碼及其依賴性的理解，可能涉及一些研究和團隊合作。

`intermediate` - `beginner` 和 `advanced` 之間的東西 :)

## **平台**

`ios` - iOS特定問題`android` - Android特定的問題

## **處理**

`wontfix` - 團隊考慮過的問題，並且在不久的將來不會解決。

`design` - 在開始實施之前需要批准設計的問題。

設計標籤可以擴展到 `design-todo`, `design-in-progress` (或 `design-wip`?) 和
`design-approved` 幫助跟踪設計進度併計劃實施和測試。

`bounty` - TBD

`blocked` - 該問題被稱為阻止其他問題，不一定明確鏈接，所以建議檢查應注意問題的評論。

`blocker` - 該問題被稱為阻止其他問題，不一定明確鏈接，所以建議檢查應注意問題的評論。

`workflow` - 該問題與團隊內部使用的流程或工作流程相關，它既不是產品功能也不是應用程序中的錯誤，例如，一個發布過程的改進。

`devops` - 該問題與團隊用於維護產品或過程的開發操作有關，例如， a Jenkins job 創造.

## **測試**

此外 `bug` 和 `test` 上面描述的標籤還有一些與測試更相關：

`help to reproduce` - 該問題具有零星的性質，需要社區參與幫助重現該錯誤。

`help needed` - 需要社區努力幫助理解或驗證問題。

## **組合標籤**

可以組合標籤以更好地分類問題，例如：

`bug high-priority android` - 將清楚地分類特定於Android平台的高優先級錯誤

`Epic high-priority` - 高優先級功能的Epic問題對於盡快實施至關重要。

`advanced ios` - iOS特定功能需要團隊的努力和研究來尋找解決方案。

[Category:Status-go](Category:Status-go "wikilink")