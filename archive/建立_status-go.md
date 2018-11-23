### 介紹

status-go 可以建立在兩種模式：

  - 獨立服務器 (`cmd/statusd`)
  - Android (`build/android-16/aar`) 和 iOS
    (`build/os-9.3/framework`)交叉構建, 為iOS模擬器構建要用額外的variant

`cgo` 習慣於為了提供JNI和iOS之間的橋樑。我們在內部使用 [xgo](https://github.com/karalabe/xgo)
用於交叉構建 (xgo是一個具有所有必要的構建依賴項的docker鏡像，用於安裝給定的構建目標).

主要工作分支是`開發` status-go.

### 先決條件

  - **Go** - 您需要安裝Go編程語言，目前版本為1.8.3。安裝非常簡單，只需按照網站上的說明操作即可。
  - **Docker** - xgo依賴於Docker鏡像。因此，為了使構建過程正常工作，您需要配置Docker環境。

### Makefile

我們使用 `Makefile`, 你可以運行make help來查看可用的命令：

```
 $ make help
 Usage: make [target]

 構建:
   statusgo                        構建 status-go 像 statusd server
   wnode-status                    構建 wnode-status (Whisper 5 debug tool)

 交叉編譯:
   statusgo-android                構建 Androin版本的 status-go
   statusgo-ios                    構建 iOS版本的 status-go for iOS
   statusgo-ios-simulator          構建 iOS模擬器版本的 status-go

 其他:
   help                            顯示此幫助
   generate                        重新生成資產和其他自動生成的內容
   mock-install                    安裝模擬工具
   mock                            重新生成 mocks
   clean                           清理

 測試:
   test                            在開發過程中運行基本的簡短測試
   test-unit                       運行單元和集成測試
   test-unit-coverage              用覆蓋面運行單元和集成測試
   ci                              立即運行所有linters和測試
```

Makefile 命令使用 `build/env.sh` 腳本確保所有內容都構建在 `within build/_workspace`
並且我們不會觸及系統方面的Go安裝，並且所有供應的依賴關係都會推送到`build/_workspace`.

你也可以使用 `go build` / `go test` 命令手動。

### iOS的構建

使用任一`make statusgo-ios` 或 `make
statusgo-ios-simulator` (後者將構建包含模擬器SDK的iOS庫).
再次，iOS中的特定代碼在`library.c` 應該由 `#ifdef IOS_DEPLOYMENT`編譯.

我們為SDK的版本9.3構建。一旦構建過程完成，請參閱<code> build / bin \</ code\>文件夾中的工件。

### Android的構建

使用 `make statusgo-android`.
為了讓綁定能夠傳達，我們依靠 [JNI](wikipedia:Java_Native_Interface "wikilink"): *Go
→ C → JNI → Java App*.

通訊代碼保存在 `library.c`. 我們的*補丁*版本的xgo，使我們能夠區分平台：

```
 #ifdef IOS_DEPLOYMENT
 /* iOS related code */
 #else
 /* Android related code */
 #include <jni.h>
 #endif
```

[category:status-go](category:status-go "wikilink")