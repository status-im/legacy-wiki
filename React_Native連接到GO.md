### 函數調用

所有`status-react`到`status-go`調用集中於
[StatusModule](https://github.com/status-im/status-react/blob/develop/modules/react-native-status/android/src/main/java/im/status/ethereum/module/StatusModule.java)
(android) 或
[RCTStatus](https://github.com/status-im/status-react/blob/develop/modules/react-native-status/ios/RCTStatus/RCTStatus.m#L97)
(ios). 方法的一般結構是：

  - caller provides a callback
  - check for availability of current Activity
  - if available: call a static method on `StatusGo` via an
    `ExecutorService` (or sometimes a `Thread`) and use its result as
    callback payload
  - else: ivoke callback with false as payload

<!-- end list -->

  - 呼叫者提供回叫
  - 檢查當前活動的可用性
  - 如果可用：通過`ExecutorService`（有時是`Thread`在`StatusGo`上調用靜態方法，並將其結果用作回調負載
  - else : ivoke作為有效負載的回調

從RN的角度來看呼叫不會阻塞，結果通過回調執行提供。

`StatusGo`是從狀態應用中嵌入的go
cade生成的C庫。在Android上，它被打包為一個JNI`aar`（使用[xgo](https://github.com/karalabe/xgo)生成）。

StatusGo直接暴露標記為導出的`status-go`方法。這些方法在[cmd/statusd/library.go](https://github.com/status-im/status-go/blob/develop/cmd/statusd/library.go)中定義並標記為`export`代碼\>。然後將`go`端調用分派到[geth/api](https://github.com/status-im/status-go/blob/develop/geth/api/api.go#L172)，然後\[
<https://github.com/status-im/status-go/blob/develop/geth/jail/jail.go#L213>
geth/jail\]

數據以原始類型來提供給go層，並以`JSON`的形式檢索RN。

### 事件

`status-go`可以直接[發送事件](https://github.com/status-im/status-go/blob/develop/geth/node/signals.go#L65)將被接收的RN側。
它通過直接調用[/StatusService.java\#L51
信號事件](https://github.com/status-im/status-react/blob/develop/modules/react-native-status/android/src/main/java/im/status/ethereum/module)使用一些[JNI膠](https://github.com/status-im/status-go/blob/develop/geth/node/signals.c#L173)。

StatusModule也當作React
context來使用[listener](https://github.com/status-im/status-react/blob/develop/modules/react-native-status/android/src/main/java/im/status/ethereum/module/StatusModule.java#L68)
[emits](https://github.com/status-im/status-react/blob/develop/modules/react-native-status/android/src/main/java/im/status/ethereum/modules/StatusModule.java＃L105)事件到RN層。

`status-react`
透過[gethEvent](https://github.com/status-im/status-react/blob/develop/src/status_im/components/status.cljs#L60)通過聽取這些事件
然後由一個[re-frame
handler](https://github.com/status-im/status-react/blob/develop/src/status_im/handlers.cljs#L155).派遣

## Go 到 React Native Signals

#### 信號意圖

信號允許*status-react*通知*status-go*中異步發生的事件。通常在前端的交互式使用期間，它會調用後端。其中一些人得到同步答案，另一些人則由於技術的性質只能在前端不鎖定的情況下才能回答。這裡*status-go*發送包含有關事件更詳細信息的信號。
*status-react*中的註冊處理程序接收並處理這些信號。

以下信號當前正在使用中。

### Signals

#### transaction.queued

`transaction.queued` 當進來的交易是在隊伍中時被觸發.

#### transaction.failed

`transaction.failed` 是在發生錯誤時由事務響應處理程序時觸發。

#### node.started

`node.started` 在底層節點啟動時觸發。

#### node.ready

`node.ready` 在底層節點完全準備就緒時觸發。這考慮到後端被完全註冊。

#### node.stopped

`node.stopped` 在底層節點完全停止時觸發。

#### node.crashed

`node.crashed` 在底層節點崩潰時被觸發。 `chaindata.removed`

#### chaindata.removed

`chaindata.removed` 在底層節點的鍊式數據被刪除時觸發。

#### module.initialized

#### request_geo_permissions

#### jail.signal 

[Category:Developer
Documentation](Category:Developer_Documentation "wikilink")
[Category:Status-go](Category:Status-go "wikilink") [Category:Status
React](Category:Status_React "wikilink")