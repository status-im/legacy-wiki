# 核心團隊目前的PR測試工作流程

所有新功能和功能都由PR構建提供。如何測試PR？下面的步驟可以幫助一下！

1.  準備好了哪些測試？好的問題和我們的團隊委員會（<https://github.com/status-im/status-react/projects/7>）回答
    - 檢查列`TO TEST`
2.  安裝具有您即將測試的問題/功能的PR內部版本。構建基本上在Diawi上可用，我們的<code>＃internal-jenkins \</
    code\>頻道在Slack中包含所有已準備好構建的鏈接。請記住，構建版本可能會過期，因此如果您要測試某個可以進行測試的內容（如3天或更長時間），則需要要求公關作者重新分配版本並為您創建一個新版本。如果您開始構建並且失敗：
    1.  嘗試重新運行構建
    2.  檢查系統性，即只是一個PR或許多
    3.  **要求在`#devops` channel**中（特別是如果它看起來是系統的，而不是Clojure dev / core
        contrib可以修復的代碼錯誤）
    4.  <u>**ping author in `#clojure` channel**</u> 如果以上沒有任何幫助
    5.  如果PR作者是外部撰稿人 - 請使用`@ [author-name]`
        發表評論（因此您直接提及他，並會收到電子郵件通知）問題是什麼並要求協助。
3.  將在儀表上適當的PR卡移到 `IN
    TESTING` (https://github.com/status-im/status-react/projects/7)
    讓別人知道你在上面 - **把它分配給你自己**！
    :)
4.  安裝Android和iOS後，請檢查當前PR修復/傳遞的功能（與功能相關的正面/負面測試）。在某些情況下，應該查看GitHub中的“文件已更改”選項卡，以檢查已更改的列表以了解測試覆蓋率或必須涵蓋的“弱”位置。如果PR中的筆記不清楚，請要求PR作者在slack的\#clojure
    channel的地方更改哪些內容已更改。
5.  使用
    <https://ethstatus.testrail.net/index.php?/suites/view/15&group_by=cases:section_id&group_order=asc&group_id=388>
    檢查合理的回歸 （大部分都是自動的）. 如果需要更多測試 - 在testteam slack的通道中討論，所以我們同意並更新回歸測試
6.  檢查升級功能。嘗試將最新發布的版本（在谷歌播放和測試飛行中可用）升級到當前正在測試的PR。
7.  沒有問題？完善！將適當的標籤放到PR（`Tested - OK`），並將PR實例移動到**MERGE（review /
    merge）。**合併後，它在儀表上將顯示`DONE`代碼\>。如果PR確定了問題描述中提到的問題 -
    請不要忘記關閉**問題。**

<!-- end list -->

1.  發現問題？在添加之前檢查重複項。 **提示(Hint)**：確保問題確實由當前的PR引入 -
    請檢查最新的開發版本。問題存在於開發中？檢查現有的問題列表，並確保在創建自己的bug之前不添加重複項目。）有一些提示如何創建一個顯著的問題
    - 請檢查[Bug Report](Bug_Report "wikilink")。還有一件事 -
    **使用標籤''，這是超級贊助！我們有哪些標籤？看看這個[列表](https://hackmd.io/s/HyopujPPZ)。一旦記錄了所有問題，將標籤**測試問題**發送給PR，並通知開發人員有幾個問題會阻止PR合併。將PR移動到板上的**CONTRIBUTOR'''。
2.  為了幫助其他人和開發者，請將PR編號放在問題標題末尾（<u>for those which are introduced by
    PR</u>）。例如：登錄按鈕被禁用\[1234\]。這意味著錯誤在PR 1234中“lives”。
3.  在我們將Test
    Rail與Github集成之前，請在測試時將以下信息添加到PR中：應用程序版本，操作系統，檢查您已經以簡要方式執行（如登錄/註銷/已恢復的帳戶），記錄/現有的（如果有的話）

# 核心團隊日常測試任務池

1.  檢查自動化測試每晚構建和分析的結果。必要時記錄問題（<u>檢查已知問題(check the known
    problems)</u>**首先**以避免重複）
2.  檢查Board for `TO
    TEST`任務（新功能和錯誤修正）。拿起任何未分配的，隨時測試！當bugfix被合併時，不要忘記關閉錯誤。
3.  貢獻於[ideas](https://github.com/status-im/ideas)。如果你現在沒有被分配任何想法 -
    繼續並從列表中選擇它（你可能想與測試主管核對哪個Idea現在值得貢獻）
4.  查看**開放問題(Open
    Issues)**列表並關閉固定/過期/無效的列表。
5.  安排時間在夜間構建上運行回歸。來自核心團隊的測試人員在一周內只有1天的時間專門用於夜間構建測試，並從狀態日常頻道（其他團隊成員撰寫他們在夜間製作時看到的問題）中查看問題。跟踪TestRail中的測試執行。對於鬆散的頻道
    -
    如果問題已知，則回复，並有助於復制並報告問題是否為新問題。
6.  創建測試用例並使用它們更新我們的**TestRail**（https://ethstatus.testrail.net/index.php?/dashboard）。我們有更多的
    - 更多將會自動化！ :)
7.  審查Instabug問題
8.  添加/維護自動測試。
UI正在改變，所以我們需要保持我們的測試活著。

# VM進行升級測試

下面是虛擬機上的圖片，帶有`0.9.11`狀態版本，但沒有太多，但很多數據：1-1聊天，群聊，公共聊天。我們可能會使用此虛擬機和帳戶來測試應用升級方案。

*如何安裝它：*'

1.  你需要有 Genymotion
2.  你需要有 VirtualBox
3.  下載這個.ova文件
    https://drive.google.com/open?id=1MBRaRx5hY0-TsAXiZqEiIn7QseS1SxAy
4.  在VirtualBox菜單中選擇文件-\>匯入(import)。選擇你下載的.ova文件。並繼續匯入(import)。
5.  重新啟動Genymotion，以便它拿起虛擬機(VM)。
6.  運行並找到Status app `0.9.11`

*User password*: `Aaaaaa`

<u>我們在這個賬戶裡有什麼數據：</u>

  - 創建了10個1-1chat。在每個1-1聊天\> 20條消息中總共從兩個聯繫人發送。 /位置和/請求也使用。一些用戶確認
  - 10在每次聊天中由初始用戶發送的隨機聯繫人\> 10條消息創建的群聊
  - 創建5個公共聊天，並且由初始用戶發送\> 5個文本/表情符號消息

# 取的測試代幣ropsten

對於STT只需發送一個具有Gas limit 105000的交易（可以是0 ether）（您需要手動設置該值！）到以下地址：
`0x34358C45FbA99ef9b78cB501584E8cBFa6f85Cef`

<code>`HND`
`Handy Test Token`
`0x9e47fb3049f0d9c953f5428ce2e6c3a8321780bf`

`LXS`
`Lucky XS Test`
`0xf29d2dc0687d7d49f57d4a731ac8bfb6edc23473`

`ADI`
`Adi Test Token`
`0xd2a816110c1177478c7e644ae4853d8e80aaec35`

`WGN`
`Wagner Test Token`
`0x65c69bc258afa0906683f42e576b272a95c203dd`

`MDS`
`Modest Test Token`
`0x972b0570d9cd8b7c41aa8349f707ec7356daa825`</code>

用戶可以通過向合適的合同地址發送正好0的乙太幣。來獲得每個1000乙太幣。

# Ropsten contract:

  - [copy of mainnet SNT
    contract](https://ropsten.etherscan.io/token/0xc55cf4b03948d7ebc8b9e8bad92643703811d162#balances)
  - [magic STT
    faucet](https://ropsten.etherscan.io/address/0x34358C45FbA99ef9b78cB501584E8cBFa6f85Cef)

## 如何在Android上查找geth.log文件

在Android 6+上，您需要在應用程序設置（`設置(Settings)->應用程序管理器(Application
manager)->狀態(Status) -
>權限(Permissions)->存儲(Storage)`）中提供`存儲(Storage)`權限。日誌文件可以在 `Download` 文件夾中找到

[Category:Testing](Category:Testing "wikilink")
[Category:FAQ](Category:FAQ "wikilink") [Category:Developer
Documentation](Category:Developer_Documentation "wikilink")