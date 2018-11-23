## Notes on status-react bindings/API

### 1\. Main Account creation

**Exported method:**

``` go
func CreateAccount(password *C.char) *C.char
```

Internally relies on:

// createAccount creates an internal geth account

  -   - Sample response:\*\*

`   {`

1.  1.  2\. Sub-account creation

<!-- end list -->

  -   - Exported method:\*\*

`   func CreateChildAccount(parentAddress, password *C.char) *C.char`

Internally relies
on:

`   // createChildAccount creates sub-account for an account identified by parent address.`

  -   - Sample Response:\*\*

`   {`

1.  1.  3\. Recover account details

<!-- end list -->

  -   - Exported Method:\*\*

`   func RecoverAccount(password, mnemonic *C.char) *C.char`

Internally relies on:

`   // recoverAccount re-creates master key using given details.`

(same as on main account creation)

`   {`

1.  1.  4\. Login (select active/current account)

<!-- end list -->

  -   - Exported
Method\*\*:

`   func Login(address, password *C.char) *C.char`

Internally:

`   // selectAccount selects current account, by verifying that address has corresponding account which can be decrypted`

  -   - Sample Response:\*\*

`   {`

1.  1.  5\. Logout

When called system makes sure that Whisper identity (injected on
\`Login()\`) is wiped out.

  -   - Exported method:\*\*

`   func Logout() *C.char`

  -   - Sample Response:\*\*

`   {`

1.  1.  666\. How to Unlock Account?

Every time somebody unlocks Ethereum account Cthulhu kills kittens (ðŸ±
ðŸ˜¸ ), dozens of them. So, don't use this method.

This method is deprecated, and will return error, asking you to rely on
\`Login()\` instead (if kittens weren't enough for you\!)

  -   - Output:\*\*

`   {`

1.  1.  6\. Sending Transactions

Every time transaction is sent to geth node (via \`SendTransaction\`,
which passes through \`jail\` package's \`Send\` method) it is
intercepted and queued. Call is \*\*NOT\*\* returned immediately and
hangs on until either of below happens:

\- \`CompleteTransaction()\` received (see below) -
\`DiscardTransaction()\` received (see below) - request times out
(normally, after 300 seconds)

When request is queued, native application will receive a signal (with
transaction \`id\` to be used to complete pending/queued transaction).

In order to complete transaction, use the following exported method:

`   func CompleteTransaction(id, password *C.char) *C.char`

where \`id\` is transaction id, and \`password\` is user's password we
will attempt to sign transaction with (if this succeeds, then
transaction will be sent to blockchain).

If user wants to cancel transaction (instead of providing password and
completing), then the following method should be used:

`   func DiscardTransaction(id *C.char) *C.char`

Note that in order to discard a transaction, only \`id\` is required (no
need to supply password). Whenever transaction is discarded,
\`SendTransaction()\` unblocks (and triggers \`transaction.failed\`
signal to application, with error code 4).

When \`DiscardTransaction()\` returns, its response is JSON having the
following data:

`   {`

ðŸŒŸ Below are possible error codes that binding can signal back to
react-native app:

`   SendTransactionNoErrorCode        = "0" // no error, tx executed ok`

1.  1.  6.1 Discarding and Completing multiple transactions

In addition to discarding and completing transactions one by one, you
can act on multiple items:

\- \`DiscardTransactions('\["tx-id1", "tx-id2", .., "tx-idN"\]')\`
allows you to discard multiple items, and response looks sth like this
(note that redundant usage of id both as key in \`results\` and
attribute is intentional, as it allows to quickly navigate the results +
the very same struct is used to return on a single discard, where
knowing the \`id\` might be necessary):

`   {`

\- \`CompleteTransactions('\["tx-id1", "tx-id2", .., "tx-idN"\]',
'password')\`, and response contains result for each transaction (since
it is important to know the status of each request):

`   {`

â­ï¸ It is important to note that response of multi-entity actions
will be returned when \*\*all\*\* of the items are processed (success,
failure or timeout for each item). Of course, bindings will send async
signals as transactions are being processed (so that app can react on
each tx individually, w/o waiting for all of them to be done with).

1.  1.  7\. Node Management

To generate default configuration object (in JSON format):

`   func GenerateConfig(datadir *C.char, networkId C.int) *C.char`

Note: \`GenerateConfig()\` requires data directory and network id to be
present, because for special networks (like testnet) default network
parameters (genesis, for instance) are loaded.

To start a node with a given configuration:

`   func StartNode(configJSON *C.char) *C.char`

To stop/resume the running node:

`   func StopNode() *C.char `

One can stop/resume just node's HTTP RPC server:

`   func StopNodeRPCServer() *C.char`

From time to time it might be necessary to wipe out the chain data, and
re-sync, for that:

`   func ResetChainData() *C.char`

To add peer
node:

`   func AddPeer(url *C.char) *C.char // adds peer, which main node will listen to`

1.  1.  8\. Jail

Requests from native application do \*\*NOT\*\* go directly to running
node. Instead, they are forwarded via
\[Otto\](https://github.com/robertkrimen/otto) VMs i.e. you use
\`Call()\` binding which internally executes JavaScript code within
jailed JS environment.

That jailed environment is initialized with JS code from
\`jail/web3.go\`, and is augmented with the code coming via \`Parse()\`
method (thus allowing native apps specify their custom
methods/commands).

Everything MUST start with \`InitJail()\`:

`   func InitJail(js *C.char)`

which accepts custom JS, to setup custom JS environment. If you haven't
initialized jailed environment with \`InitJail()\` calls to other
exported methods will \*\*fail\*\*.

Now, once jailed environment is prepared, it is time to start/setup VM:

`   func Parse(chatId *C.char, js *C.char) *C.char`

You need to specify \`chatId\` and custom JS. That \`chatId\` will
uniquely identify crated VM. Then, to execute some JS code on that
uniquely identified VM:

`   func Call(chatId *C.char, path *C.char, params *C.char) *C.char`

The \`path\` identifies command to run e.g. \`\["commands", "send"\]\`
and \`params\` is just command arguments (passed as object i.e. \`{}\`).

1.  1.  9\. Whisper

This section will be updated once we migrate to Whisper ver.5 + Enable
Push Notifications and Inboxing

1.  1.  10\. Profiling

A few bindings that can be used to collect some CPU and memory reports.

Collected data is not returned directly, but instead it is saved into a
file in the directory passed in the first argument.

  -   - Exported Methods:\*\*

To collect CPU profiling data:

`   func StartCPUProfile(dataDir *C.char) *C.char`

and to stop it:

`   func StopCPUProfiling() *C.char`

Internally relies
on:

`   // StartCPUProfile enables CPU profiling for the current process. While profiling,`

and

`   // StopCPUProfile stops the current CPU profile, if any, and closes the file.`

To collect memory profiling data:

`   func WriteHeapProfile(dataDir *C.char) *C.char`

Internally relies on:

`   // WriteHeapFile writes heap memory to the file.`