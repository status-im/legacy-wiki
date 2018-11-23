# 筆記

  - 合併後，可以使用go-mobile成功編譯和運行節點
    <https://github.com/status-im/status-go/pull/444>
  - 可以將ObjC中的對像傳遞給Go中編寫的Status Mobile實例。看一下這個：

<!-- end list -->

``` go
// mobile.go

// SignalBus is an object that can receive signals.
// signal can be even a struct.
type SignalBus interface {
    SendSignal(signal string)
}

// NewMobile creates a new Mobile instance
// that can start a node etc.
func NewMobile(bus) *Mobile {
  return &Mobile{bus}
}

// StartNodeAsync starts a node asynchronously.
// When it's started, it sends a signal.
func (m *Mobile) StartNodeAsync(rawConfig string) error {
    config, err := params.LoadNodeConfig(rawConfig)
    if err != nil {
        return err
    }

    nodeStarted, err := m.api.StartNodeAsync(config)

    go func() {
        <-nodeStarted
        m.bus.SendSignal("node started")
    }()

    return err
}

// In Objective-C
StatusdMobile *status = StatusdNewMobile(self);
[status startNodeAsync:config error:&error];

// self passed to StatusdNewMobile implements protocol
// StatusdSignalBus, i.e. this method.
// signal can be a Go struct and it would be used like this:
//   switch (signal.type) {
//     case StatusdEventNodeReady:
//       // do something
- (void)sendSignal:(NSString*)signal {
    switch (signal) {
        case @"node started":
            // do something
            break;
    }
}
```

在一些非常難找的隨機地方不會有更多的信號發送，無需製作一些瘋狂的C代碼。一切都很容易推理。

  - 很多工作要做......而且需要對status-react進行全面的重新設計。但是，它將使瀏覽整個項目變得更加容易...

# 資源

1.  <https://github.com/caseylmanus/go-react-native> — 如何將Go與React
    Native集成。它還展示瞭如何在Go代碼和ObjC代碼之間來回通信。
2.  <https://github.com/golang/mobile/blob/master/bind/testpkg/objcpkg/classes.go>
    — 幾個例子如何從Go中調用ObjC。

[Category:Status-go](Category:Status-go "wikilink")