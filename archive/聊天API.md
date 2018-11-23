# 界面解剖

這解剖學建立聊天界面的不同部分並且建立一個共同的言辭。主要組成部分是：

  - 信息
  - 輸入
  - 鍵盤
  - 建議

![chat-anatomy.png](chat-anatomy.png
"chat-anatomy.png")

# 訪問一個機器人

`/global`（hidden）bot的通用命令允許通過`@botname`在任何聊天中引用bot命令。如果存在，它應該出現在命令列表中作為底部的另一部分。

`/init` (hidden)
在開放的聊天會話中運行，只能進行1對1的聊天。

`/`<command> 機器人可以註冊任何ASCII標題的命令處理程序，然後狀態(Status)重複

`@botname/command`
如果聊天上下文中有多個機器人，則可以使用`@botname`來區分這個區別（或者應該出現一個建議/命令列表來幫助區分）

`Bot Message`
機器人可以發送一條消息，該消息只能在聊天記錄中被其自身的另一個實例讀取。該消息可以發送標記來繪製和包含數據。它由用戶點擊它激活。

`A message parsed from chat history`
在1對1聊天中，任何用戶發送的消息都可以被機器人讀取，並且可以通過`status.on（'chat-update'，function（）{...}）;`處理程序。

# Bot消息

一個Bot消息可以包含它自己的標記和一個數據有效載荷，這個消息可以發送到聊天環境。如果它是可交互的，那麼交互（我們目前稱之為響應處理程序）在點擊時處理消息。

**如果用戶沒有安裝該機器人，那麼我們將顯示一個自定義鍵盤，顯示機器人名稱，評論/信譽和 a `Install / Add to
Contacts` 按鈕，用戶可以取消或安裝並繼續響應流程。**

# 命令處理程序

一旦調用了一個命令處理程序，它就會傳遞消息的當前文本/數據負載，並且需要返回參數（及其占位符信息）。 處理程序的返回必須包含建議區域的 -
`markup` 或`status component(data)` for the suggestions area - `markup` 或
`status component(data)` 對於鍵盤 - 一個錯誤對象 -
修改後的文本/數據有效載荷。

# 隱私

  - 機器人只能通過命令或消息通過1對1聊天進行交互
  - 一個機器人只能看到自己和當前用戶在群聊中的消息。

# 部署＆amp;測試

控制台應該有一些`/debug`模式，允許用戶插入手機並熱插入javascript/web頁面，以便從桌面輕鬆測試基於Web的dapp或機器人。理想情況下，它應該與[Truffle](http://truffleframework.com/)和[Embark](https://github.com/iurimatias/embark-framework)集成，並且尊重[EIP190](https://github.com/ethereum/EIPs/issues/190)

# 打開問題

Otto VM監獄周圍是否存在資源和保護問題？它應該每個聊天上下文運行每個dapp嗎？或每個DApp，然後每個聊天上下文中有一個對象？

[Category:Chat API](Category:Chat_API "wikilink") [Category:Status
Application](Category:Status_Application "wikilink")
[Category:API](Category:API "wikilink") [Category:Developer
Documentation](Category:Developer_Documentation "wikilink")