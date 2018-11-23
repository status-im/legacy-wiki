# 笔记

  - 合併后，可以使用go-mobile成功编译和运行节点
    <https://github.com/status-im/status-go/pull/444>
  - 可以将ObjC中的对像传递给Go中编写的Status Mobile实例。看一下这个：

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

在一些非常难找的随机地方不会有更多的信号发送，无需製作一些疯狂的C代码。一切都很容易推理。

  - 很多工作要做......而且需要对status-react进行全面的重新设计。但是，它将使浏览整个项目变得更加容易...

# 资源

1.  <https://github.com/caseylmanus/go-react-native> — 如何将Go与React
    Native集成。它还展示瞭如何在Go代码和ObjC代码之间来回通信。
2.  <https://github.com/golang/mobile/blob/master/bind/testpkg/objcpkg/classes.go>
    — 几个例子如何从Go中调用ObjC。

[Category:Status-go](Category:Status-go "wikilink")