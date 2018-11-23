Proof by construction.

# Generate CHT:

1.  Clone <https://github.com/ethereum/go-ethereum>
2.  Run `make`
3.  Run full node: `build/bin/geth --testnet --syncmode "full"
    --lightserv 90 --cache 512 console`
4.  Wait until it synchronises and find log messages like:
    INFO \[12-11|19:45:13\] CHT section=67
    sectionHead=9832cf2ce760d4e3a7922fbfedeaa5dce67f1772e0f729f67c806bfafdedc370
    root=60d43984a1d55e93f4296f4b48bf5af350476fe48679a73263bd57d8a324c9d4
    INFO \[12-11|19:45:13\] BloomTrie section=67
    sectionHead=9832cf2ce760d4e3a7922fbfedeaa5dce67f1772e0f729f67c806bfafdedc370
    root=fd81543dc619f6d1148e766b942c90296343c2cd0fd464946678f27f35feb59b

You’ll need all values here:

  - **Section number:** 67
  - **Section head:** 9832cf…
  - **CHT root**: 60d439…
  - **BloomTrie root**: 9832cf…

# Update CHT:

Currently geth can only work with hardcoded Trusted Checkpoints and the
code is inside `go-ethereum` codebase. Following our protocol of
updating `go-ethereum` vendored package, follow these steps:

1.  Reverse current version of CHT patch:
    patch -p1 -R \< geth-patches/0006-latest-cht.patch
2.  Edit `geth-patches/0006-latest-cht.patch` and change
    `statusRopstenCheckpoint` variable with values from above log
    output.
3.  Apply patch:
    patch -p1 \< geth-patches/0006-latest-cht.patch
4.  Run `build/bin/statusd --networkid 3 les` on all available networks
    and ensure block synchronisation starts from relatively latest
    numbers (see the original geth output)
5.  Commit and file a PR

## **How to avoid running full sync on you localhost?**

1.  Go to machine called `miner1`,
2.  Run `service stop geth` to stop currently running geth,
3.  Run a container:
    docker run -d –name full-geth -i -p 30303:30303 -v
    /mnt/ethereum/custom-full:/root/.ethereum ethereum/client-go:v1.7.2
    –testnet –syncmode “full” –cache 512 –verbosity 5 –lightserv 90

`/mnt/ethereum/custom-full` directory contains previously downloaded
chain-data files.

1.  Grep for CHT logs: `docker logs --since=30s -f full-geth 2>&1 | grep
    -e "INFO\|CHT"`,
2.  When you get the newest CHT number, stop `full-geth` container and
    remove it,
3.  Bring back the stopped geth service: `service start geth`.