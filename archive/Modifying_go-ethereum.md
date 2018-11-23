We depend on `go-ethereum` and though the project is loaded as
`vendor/github.com/ethereum/go-ethereum` , it actually comes from
[`github.com/status-im/go-ethereum`](http://github.com/status-im/go-ethereum).

The idea is keep all our local changes to `go-ethereum` in
`[github.com/status-im/go-ethereum](http://github.com/status-im/go-ethereum)`
and keep them documented and there’s a special process to make this
happen:

1.  Do changes as usual in
    [`status-go`](http://github.com/status-im/status-go) including
    `go-ethereum` in the vendor folder.
2.  Clone
    [`github.com/status-im/go-ethereum`](http://github.com/status-im/go-ethereum)
    and check out the latest branch like `status/1.7.2-stable`.
3.  Copy your `go-ethereum` folder from the `status-go` vendor directory
    into the newly cloned `go-ethereum`.
4.  Look at `git diff` and ensure there’re only those changes you’ve
    intended to make.
5.  Commit and push the new changes.
6.  Modify
    <https://github.com/status-im/go-ethereum/wiki/Rebase-Geth-1.7.0> so
    that other’s are aware what the commit there is.