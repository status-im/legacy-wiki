# General Info

  - we depend on `go-ethereum` project, which is under heavy development
  - we have our own fork of the `go-ethereum` repo:
    <https://github.com/status-im/go-ethereum>
  - from time to time we need to pull code from the upstream (which is
    `go-ethereum` for our fork)
  - in addition to pulling changes from upstream, we need to apply our
    custom commits (which we have for some custom functionality we need
    on top of the stock `go-ethereum` project)

# Rebase workflow

We start out of stable branch (assuming that `a07539fb` points to the
starting point in upstream `go-ethereum` we want to base on):

`git co -b status/1.5.9 a07539fb`

Now, we need to apply our custom commits (they are generally available
in our previous branch, say `status/1.5.8-stable`)

`git cherry-pick "start^..end"` (or you can cherry pick commits one by
one, say, if you want to omit one in between)

While cherry picking, we will have to deal with **conflicts**. Indeed,
our custom changes are compatible with the previous release, and new
release might have changed sth. Once conflicts have been resolved, make
sure that running `make` compiles w/o any problems.

Create a PR from `status/1.5.9` to `develop` in `go-ethereum`
repository. When it is merged, it is time to update `status-go` vendor
folder.

Go to `status-go` repository. Run `dep ensure --update
github.com/ethereum/go-ethereum)`. That will update the vendored version
to the latest update.

Try to make `status-go`. Chances that it will fail.

If it fails, you need to fix issues in the necessary repo, and commit
changes as `vendor: rebase fixes`.

Now you are ready to file a PR to `status-go` (which will contain 2
commits\!)