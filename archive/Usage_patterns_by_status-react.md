1.  Usage patterns by status-react

status-go has a number of customisations for go-ethereum and many of
them are usually not obvious. Nor what status-react is present in
human-readable form.

This page serves as a place with a collection of random facts about how
status-react uses status-go. This should be revised and structured and
currently just a quick start.

## How status-react calls status-go?

1.  React Native, which is used for running app itself, this env can
    call web3 through CallRPC and we have a full control over code
    inside this env
2.  webviews: among other things Status is dapp browser, so users can
    run dapps in the webview and we inject web3 object into webview in
    order to make this happen. We don’t have control over code inside
    this env, it’s third party. All calls to web3 inside webview are
    passed to CallRPC as well
3.  jail (otto vm): we are using this env in order to allow third party
    to define commands and different bot’s behaviour in chat. Calls to
    web3 are handled internally on go side, so we don’t use CallRPC at
    least from status-react side here. Ans again, code is written by
    third parties, except our own bots
4.  JNI signals:
    [1](https://github.com/status-im/status-go/wiki/Notes-on-Bindings)(https://github.com/status-im/status-go/wiki/Notes-on-Bindings)

## What are Send and SendAsync?

\!\[\](https://i.imgur.com/Ov1Y0H4.png)

The source:

`   Consumer->Go Bindings: Call`

1.  1.  Send Transaction Flow

When the (async) handler of \`/send\` command is executed, the callback
doesn’t return till the transaction queue is committed (confirmed), but
immediately we got confirmation from jail about transaction being added
(“transaction.queued” event).

Also, some insights can be found in
[2](https://github.com/status-im/status-go/wiki/Notes-on-Bindings)(https://github.com/status-im/status-go/wiki/Notes-on-Bindings)