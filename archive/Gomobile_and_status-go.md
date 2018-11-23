# Notes

1.  It’s possible to compile and run node successfully with go-mobile
    after merging <https://github.com/status-im/status-go/pull/444>
2.  It’s possible to pass an object from ObjC to Status Mobile instance
    written in Go. Check this out:
    // mobile.go
    // SignalBus is an object that can receive signals. // signal can be
    even a struct. type SignalBus interface { SendSignal(signal string)
    }
    // NewMobile creates a new Mobile instance // that can start a node
    etc. func NewMobile(bus) \*Mobile { return \&Mobile{bus} }
    // StartNodeAsync starts a node asynchronously. // When it’s
    started, it sends a signal. func (m \*Mobile)
    StartNodeAsync(rawConfig string) error { config, err :=
    params.LoadNodeConfig(rawConfig) if err \!= nil { return err }
    ```
     nodeStarted, err := m.api.StartNodeAsync(config)

     go func() {
         <-nodeStarted
         m.bus.SendSignal("node started")
     }()

     return err
    ```
    }
    // In Objective-C StatusdMobile \*status = StatusdNewMobile(self);
    \[status startNodeAsync:config error:\&error\];
    // self passed to StatusdNewMobile implements protocol //
    StatusdSignalBus, i.e. this method. // signal can be a Go struct and
    it would be used like this: // switch (signal.type) { // case
    StatusdEventNodeReady: // // do something
      - (void)sendSignal:(NSString\*)signal { switch (signal) { case
        @“node started”: // do something break; } }

No more sending signals in some random places which are super hard to
find and no need to craft some crazy C code. Everything is easy to
reason about.

1.  There is a lot of work to be done… And it will require a complete
    redesign of integration with status-react. However, it will make it
    much easier to navigate through the whole project…

# Resources

1.  <https://github.com/caseylmanus/go-react-native> — how to integrate
    Go with React Native. It also shows how to communicate back and
    forth between Go code and ObjC
    code.
2.  <https://github.com/golang/mobile/blob/master/bind/testpkg/objcpkg/classes.go>
    — A few examples how to call ObjC from Go.