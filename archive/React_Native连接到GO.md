### 函数调用

所有`status-react`到`status-go`调用集中于
[StatusModule](https://github.com/status-im/status-react/blob/develop/modules/react-native-status/android/src/main/java/im/status/ethereum/module/StatusModule.java)
(android) 或
[RCTStatus](https://github.com/status-im/status-react/blob/develop/modules/react-native-status/ios/RCTStatus/RCTStatus.m#L97)
(ios). 方法的一般结构是：

  - caller provides a callback
  - check for availability of current Activity
  - if available: call a static method on `StatusGo` via an
    `ExecutorService` (or sometimes a `Thread`) and use its result as
    callback payload
  - else: ivoke callback with false as payload

<!-- end list -->

  - 呼叫者提供回叫
  - 检查当前活动的可用性
  - 如果可用：通过`ExecutorService`（有时是`Thread`在`StatusGo`上调用静态方法，并将其结果用作回调负载
  - else : ivoke作为有效负载的回调

从RN的角度来看呼叫不会阻塞，结果通过回调执行提供。

`StatusGo`是从状态应用中嵌入的go
cade生成的C库。在Android上，它被打包为一个JNI`aar`（使用[xgo](https://github.com/karalabe/xgo)生成）。

StatusGo直接暴露标记为导出的`status-go`方法。这些方法在[cmd/statusd/library.go](https://github.com/status-im/status-go/blob/develop/cmd/statusd/library.go)中定义并标记为`export`代码\>。然后将`go`端调用分派到[geth/api](https://github.com/status-im/status-go/blob/develop/geth/api/api.go#L172)，然后\[
<https://github.com/status-im/status-go/blob/develop/geth/jail/jail.go#L213>
geth/jail\]

数据以原始类型来提供给go层，并以`JSON`的形式检索RN。

### 事件

`status-go`可以直接[发送事件](https://github.com/status-im/status-go/blob/develop/geth/node/signals.go#L65)将被接收的RN侧。
它通过直接调用[/StatusService.java\#L51
信号事件](https://github.com/status-im/status-react/blob/develop/modules/react-native-status/android/src/main/java/im/status/ethereum/module)使用一些[JNI胶](https://github.com/status-im/status-go/blob/develop/geth/node/signals.c#L173)。

StatusModule也当作React
context来使用[listener](https://github.com/status-im/status-react/blob/develop/modules/react-native-status/android/src/main/java/im/status/ethereum/module/StatusModule.java#L68)
[emits](https://github.com/status-im/status-react/blob/develop/modules/react-native-status/android/src/main/java/im/status/ethereum/modules/StatusModule.java＃L105)事件到RN层。

`status-react`
透过[gethEvent](https://github.com/status-im/status-react/blob/develop/src/status_im/components/status.cljs#L60)通过听取这些事件
然后由一个[re-frame
handler](https://github.com/status-im/status-react/blob/develop/src/status_im/handlers.cljs#L155).派遣

## Go 到 React Native Signals

#### 信号意图

信号允许*status-react*通知*status-go*中异步发生的事件。通常在前端的交互式使用期间，它会调用后端。其中一些人得到同步答桉，另一些人则由于技术的性质只能在前端不锁定的情况下才能回答。这裡*status-go*发送包含有关事件更详细信息的信号。
*status-react*中的注册处理程序接收并处理这些信号。

以下信号当前正在使用中。

### Signals

#### transaction.queued

`transaction.queued` 当进来的交易是在队伍中时被触发.

#### transaction.failed

`transaction.failed` 是在发生错误时由事务响应处理程序时触发。

#### node.started

`node.started` 在底层节点启动时触发。

#### node.ready

`node.ready` 在底层节点完全准备就绪时触发。这考虑到后端被完全注册。

#### node.stopped

`node.stopped` 在底层节点完全停止时触发。

#### node.crashed

`node.crashed` 在底层节点崩溃时被触发。 `chaindata.removed`

#### chaindata.removed

`chaindata.removed` 在底层节点的鍊式数据被删除时触发。

#### module.initialized

#### request_geo_permissions

#### jail.signal 

[Category:Developer
Documentation](Category:Developer_Documentation "wikilink")
[Category:Status-go](Category:Status-go "wikilink") [Category:Status
React](Category:Status_React "wikilink")