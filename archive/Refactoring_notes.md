# Refactoring notes

This document consists of random notes about status-go refactoring so
that we don’t forget about them. Here they are:

1.  In many places we use switches over the three networks we support
    (mainnet, ropsten, rinkeby). This may be meaningful to move them
    into a separate interface encapsulating common operations for these
    networks and created by a factory.
2.  I believe transactions sending and deciding which server to use for
    it deserve better architecture. Let’s examine if concerns are
    separated correctly and fix it if not.
3.  Make `jail.baseJSCode` always end with `;` somehow. Those ugly hacks
    adding `;` to the end are annoying.
4.  Investigate why we return `0x` address in case of a missing value in
    `RPCCall.ParseFromAddress`.
5.  ExecutionPolicy should be an interface and an appropriate
    implementation should be set in construction time, not during
    execution. Hence less checks for `config.UpstreamConfig.Enabled`.
    Generally, all logic regarding RPC requests handling must be
    incapsulated in small components which complies to a single
    interface, this shouldn’t be handled with `if` statements for
    different cases.
6.  `preProcessRequest` and `postProcessRequest` should be named
    appropriately and shouldn’t be just hanging functions.
7.  Probably, it should be not `StopRPCCallError` which callees return
    but callers should instead detect an error and terminate if it’s
    fatal or something.
8.  Revisit Jail, NodeManager and so on and determine concerns of each.
9.  Search for StatusIM under vendor/go-ethereum and fix logger
    dependency for Status components which must come from geth/log.
    Tackle it only no earlier \#245
10. `StatusAPI`’s `TxQueueManager` and `SendTransaction` are only
    exposed for tests and look like either a design shortcoming or test
    helpers. Either way, this should be fixed after tests for C bindings
    are made separate from testing application logic.
11. `TxQueueManager` should not know about `AccountManager` and
    `NodeManager`. However, it should be able to send a transaction
    (after receiving `CompleteTransaction()` call).
    <https://github.com/status-im/status-go/issues/338>
12. Refactor `TxQueue` and `TxQueueManager` handlers as after moving it
    to `status-go`, their purpose is unknown.
    <https://github.com/status-im/status-go/issues/338>
13. Remove `common.QueuedTxID`.
    <https://github.com/status-im/status-go/issues/338>
14. Refactor logging, some ideas are presented in this discussion:
    <https://github.com/status-im/status-go/pull/256>, the main point is
    that `NodeConfig.LogLevel` seems redundant.
15. Remove `node.RPCClient`.
16. Bring `NetworkId` from `NodeConfig` together with `UpstreamConfig`
    as they are tightly coupled and this would decrease chances that
    they’re inconsistent to one another.
17. Refactor `NodeManager`’s all start and stop methods. Start/Stop
    logic is overcomplicated with synchronisation and has bad naming.
18. Think about creating `RunningNode` field in `NodeManager` which will
    eliminate a lot of unneeded checks.
19. Rename `geth/jail/jail_cell.go` to `geth/jail/cell.go`
20. Get rid of `makeSendHandler` call in `geth/jail/handlers.go`. It’s
    post-refactoring byproduct, just use `jail.Send` instead.
21. Get rid of ExecutionPolicy.
22. Rename `upstream` to `remote`.
23. Remove `faucet` cmd, we now use an original geth node to request
    ether.
24. Fix lint notice for NodeManager `type name will be used as
    node.NodeManager by other packages, and that stutters; consider
    calling this Manager`

Subtasks will be added up until there is enough work to start.

  - TODO: Split this issue over several each taking approximately 1 day.
  - TODO: Traverse the source for
  - TODOs and FIXMEs for other refactoring notes.