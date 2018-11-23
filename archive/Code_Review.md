## Motivation and fundamental principles

The main CR target is to approve PR.

Status wants the CR process to be transparent for all his contributors.
We want to move as fast as we can.

This doc is a set of rules («SHOULD»s and «SHOULD NOT»s) that all core
contributors are agreed with. It is not a finished state of the
document, more in a way a live iterative process.

Core contributers SHOULD keep in mind that any PR brings a value.

The new version of this document should be approved by all core
developers.

## CR rules

1.  PR should be approved and merged into upstream if TWO OR MORE core
    developers have approved it and if there is no any cause of reject
    with the point of the current document.
2.  CI should pass successfully.
3.  PR should work according the issue or the bug and only according it.
    PR should solve the issue which set in the task and only the ussue.
4.  Any code addition or changing should be covered with proper tests.
5.  PR should not contain a commented code.
6.  Comments like TO DO or FIX ME should be named, e.g. \`//
    todo(@jeka): remove after task\#000\`.
7.  Any refactoring should be described in PR: what, where and why
    something has beed changed.
8.  The changes in a vendored go-ethereum should be done as described in
    [Modifying
    go-ethereum](https://www.notion.so/status/Modifying-go-ethereum-245a078dde834b87aa7e4e9b4e8cc6ec)