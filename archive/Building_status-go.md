## Introduction

status-go can be built in two modes:

1.  standalone server
2.  library for linking with
    [status-react](https://github.com/status-im/status-react)

## Installation

First of all, you need to have [Go programming
language](https://golang.org) installed in your system. Installation is
straightforward, just follow instructions on the website.

## Package structure

Package structure is idiomatic Go layout - we use a few packages for
different modules and **cmd/statusd** for the binary itself.

We also use **Makefile**, and you can run **make help** to see available
commands:

<code>$ make help Usage: make
\[target\]

build:

` statusgo                        Build status-go as statusd server`

cross-compile:

` statusgo-android                Build status-go for Android`
` statusgo-ios                    Build status-go for iOS`
` statusgo-ios-simulator          Build status-go for iOS Simulator`

other:

` help                            Show this help`
` generate                        Regenerate assets and other auto-generated stuff`
` mock                            Regenerate mocks`
` clean                           Cleanup`

tests:

` lint                            Run meta linter on code`
` test                            Run tests`</code>

## Tests

For tests run **make ci** or just **build/env.sh go test**