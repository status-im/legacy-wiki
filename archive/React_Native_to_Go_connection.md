# Function calls

All `status-react` to `status-go` calls are centralized in
[StatusModule](https://github.com/status-im/status-react/blob/develop/modules/react-native-status/android/src/main/java/im/status/ethereum/module/StatusModule.java)
(android) or
[RCTStatus](https://github.com/status-im/status-react/blob/develop/modules/react-native-status/ios/RCTStatus/RCTStatus.m#L97)
(ios). General structure of methods is:

  - caller provides a callback
  - check for availability of current Activity
  - if available: call a static method on `StatusGo` via an
    ExecutorService (or sometimes a Thread) and use its result as
    callback payload
  - else: ivoke callback with false as payload

From RN perspective calls do not block, result is provided through
callback execution.

StatusGo is a C library generated from go cade embeded in Status apps.
On android it is packaged as a JNI `aar` (generated using
[xgo](https://github.com/karalabe/xgo)).

StatusGo directly expose `status-go` methods marked for exportation.
Those methods are defined in
[cmd/statusd/library.go](https://github.com/status-im/status-go/blob/develop/cmd/statusd/library.go)
and marked as `export`. `go` side calls are then dispatched to
[geth/api](https://github.com/status-im/status-go/blob/develop/geth/api/api.go#L172)
then
[geth/jail](https://github.com/status-im/status-go/blob/develop/geth/jail/jail.go#L213)

Data is provided to the go layer as primitive types and retieved RN side
as JSON.

# Events

`status-go` can directly [send
events](https://github.com/status-im/status-go/blob/develop/geth/node/signals.go#L65)
that will be received RN side. It achieves that by directly calling
[signal-event](https://github.com/status-im/status-react/blob/develop/modules/react-native-status/android/src/main/java/im/status/ethereum/module/StatusService.java#L51)
using some [JNI
glue](https://github.com/status-im/status-go/blob/develop/geth/node/signals.c#L173).

StatusModule is also used as a react context
[listener](https://github.com/status-im/status-react/blob/develop/modules/react-native-status/android/src/main/java/im/status/ethereum/module/StatusModule.java#L68)
that
[emits](https://github.com/status-im/status-react/blob/develop/modules/react-native-status/android/src/main/java/im/status/ethereum/module/StatusModule.java#L105)
event to the RN layer.

`status-react` listens to those events via
[gethEvent](https://github.com/status-im/status-react/blob/develop/src/status_im/components/status.cljs#L60)
then dispatched by a [re-frame
handler](https://github.com/status-im/status-react/blob/develop/src/status_im/handlers.cljs#L155).