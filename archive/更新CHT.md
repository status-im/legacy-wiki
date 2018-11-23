通過施工證明。.

# 生成CHT:

  - 克隆 (Clone) <https://github.com/ethereum/go-ethereum>
  - 運行`make`
  - 運行完整節點： `build/bin/geth --testnet --syncmode "full" --lightserv 90
    --cache 512 console`
  - 等到它同步並找到類似的日誌消息:

`INFO [12-11|19:45:13] CHT section=67
sectionHead=9832cf2ce760d4e3a7922fbfedeaa5dce67f1772e0f729f67c806bfafdedc370
root=60d43984a1d55e93f4296f4b48bf5af350476fe48679a73263bd57d8a324c9d4`
`INFO [12-11|19:45:13] BloomTrie section=67
sectionHead=9832cf2ce760d4e3a7922fbfedeaa5dce67f1772e0f729f67c806bfafdedc370
root=fd81543dc619f6d1148e766b942c90296343c2cd0fd464946678f27f35feb59b`

你需要在這裡的所有的值：

  - **Section number:** 67
  - **Section head:** 9832cf…
  - **CHT root**: 60d439…
  - **BloomTrie root**: 9832cf…

# 更新CHT:

目前geth只能使用硬編碼的可信檢查點，代碼在裡面 `go-ethereum` 基礎代碼庫。遵循我們的更新協議 `go-ethereum`
供應源包，請按照下列步驟操作：

  - CHT補丁反向當前版本：

`patch -p1 -R < geth-patches/0006-latest-cht.patch`

  - 編輯`geth-patches/0006-latest-cht.patch` 並改變 `statusRopstenCheckpoint`
    變量來自上面日誌輸出的值。
  - 應用補丁:

`patch -p1 < geth-patches/0006-latest-cht.patch`

  - 運行 `build/bin/statusd --networkid 3 les`
    在所有可用的網絡上，並確保數據塊同步從相對最新的數字開始（請參閱原始geth輸出）
  - Commit and file a PR

## **如何避免在本地主機上運行完全同步？**

  - 去機器調用 `miner1`,
  - 運行 `service stop geth`
    停止當前正在運行的geth，
  - 運行一個容器:

<!-- end list -->

    docker run -d –name full-geth -i -p 30303:30303 -v /mnt/ethereum/custom-full:/root/.ethereum ethereum/client-go:v1.7.2 –testnet –syncmode “full” –cache 512 –verbosity 5 –lightserv 90

`/mnt/ethereum/custom-full` 目錄包含以前下載的鍊式數據文件。

  - CHT日誌的Grep: `docker logs --since=30s -f full-geth 2>&1 | grep -e
    "INFO\|CHT"`,
  - 當你得到最新的CHT號碼時，停止 `full-geth` 容器並將其刪除，
  - 退回已停止的geth服務：`service start geth`.

[Category:Status-go](Category:Status-go "wikilink")