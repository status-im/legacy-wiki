# Jail

It is a cells manager. It keeps track of all cells created during the
application execution. It also creates and initializes cells with
JavaScript code.

# Cell

A cell is a JavaScript execution context with a few utilities. Each cell
is initialized by `Jail` using `Jail.Parse` method.

Cell’s JS execution context is initialized with `web3`
(https://github.com/ethereum/web3.js) and any JS code provided to
`jail.Parse`.

`web3` is initialized with a provider which is called `jeth` and is
constructed in Go code in `jail.Parse`. Thus, it defines a bridge
between `web3` and `status-go`.

`jeth`, as a provider, must implement `send` and `sendAsync` methods.
Both of them are registered in `registerHandlers` function. Effectively,
`jeth.send` calls `jail.Send` while `jeth.sendAsync` calls `jail.Send`
as well, but in a goroutine. The result is returned via a callback.

For instance, let’s take a look at what happens when one executes a
synchronous call `web3.eth.blockNumber`:

1.  `web3` translates this call to a JSON-RPC method `eth_blockNumber`,
2.  Provider is used to make a HTTP or IPC call: `jeth.send`,
3.  `jail.Send` is effectively called as it’s in `jeth.send`
    implementation,
4.  `rpc.Client.CallContext` is called with a method and params,
5.  Response is returned back to the JS code.