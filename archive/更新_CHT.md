通过施工证明。.

# 生成CHT:

  - 克隆 (Clone) <https://github.com/ethereum/go-ethereum>
  - 运行`make`
  - 运行完整节点： `build/bin/geth --testnet --syncmode "full" --lightserv 90
    --cache 512 console`
  - 等到它同步并找到类似的日志消息:

`INFO [12-11|19:45:13] CHT section=67
sectionHead=9832cf2ce760d4e3a7922fbfedeaa5dce67f1772e0f729f67c806bfafdedc370
root=60d43984a1d55e93f4296f4b48bf5af350476fe48679a73263bd57d8a324c9d4`
`INFO [12-11|19:45:13] BloomTrie section=67
sectionHead=9832cf2ce760d4e3a7922fbfedeaa5dce67f1772e0f729f67c806bfafdedc370
root=fd81543dc619f6d1148e766b942c90296343c2cd0fd464946678f27f35feb59b`

你需要在这裡的所有的值：

  - **Section number:** 67
  - **Section head:** 9832cf…
  - **CHT root**: 60d439…
  - **BloomTrie root**: 9832cf…

# 更新CHT:

目前geth只能使用硬编码的可信检查点，代码在裡面 `go-ethereum` 基础代码库。遵循我们的更新协议 `go-ethereum`
供应源包，请按照下列步骤操作：

  - CHT补丁反向当前版本：

`patch -p1 -R < geth-patches/0006-latest-cht.patch`

  - 编辑`geth-patches/0006-latest-cht.patch` 并改变 `statusRopstenCheckpoint`
    变量来自上面日志输出的值。
  - 应用补丁:

`patch -p1 < geth-patches/0006-latest-cht.patch`

  - 运行 `build/bin/statusd --networkid 3 les`
    在所有可用的网络上，并确保数据块同步从相对最新的数字开始（请参阅原始geth输出）
  - Commit and file a PR

## **如何避免在本地主机上运行完全同步？**

  - 去机器调用 `miner1`,
  - 运行 `service stop geth`
    停止当前正在运行的geth，
  - 运行一个容器:

<!-- end list -->

    docker run -d –name full-geth -i -p 30303:30303 -v /mnt/ethereum/custom-full:/root/.ethereum ethereum/client-go:v1.7.2 –testnet –syncmode “full” –cache 512 –verbosity 5 –lightserv 90

`/mnt/ethereum/custom-full` 目录包含以前下载的鍊式数据文件。

  - CHT日志的Grep: `docker logs --since=30s -f full-geth 2>&1 | grep -e
    "INFO\|CHT"`,
  - 当你得到最新的CHT号码时，停止 `full-geth` 容器并将其删除，
  - 退回已停止的geth服务：`service start geth`.

[Category:Status-go](Category:Status-go "wikilink")