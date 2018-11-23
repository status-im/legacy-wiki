### 介绍

status-go 可以建立在两种模式：

  - 独立服务器 (`cmd/statusd`)
  - Android (`build/android-16/aar`) 和 iOS
    (`build/os-9.3/framework`)交叉构建, 为iOS模拟器构建要用额外的variant

`cgo` 习惯于为了提供JNI和iOS之间的桥樑。我们在内部使用 [xgo](https://github.com/karalabe/xgo)
用于交叉构建 (xgo是一个具有所有必要的构建依赖项的docker镜像，用于安装给定的构建目标).

主要工作分支是`开发` status-go.

### 先决条件

  - **Go** - 您需要安装Go编程语言，目前版本为1.8.3。安装非常简单，只需按照网站上的说明操作即可。
  - **Docker** - xgo依赖于Docker镜像。因此，为了使构建过程正常工作，您需要配置Docker环境。

### Makefile

我们使用 `Makefile`, 你可以运行make help来查看可用的命令：

```
 $ make help
 Usage: make [target]

 构建:
   statusgo                        构建 status-go 像 statusd server
   wnode-status                    构建 wnode-status (Whisper 5 debug tool)

 交叉编译:
   statusgo-android                构建 Androin版本的 status-go
   statusgo-ios                    构建 iOS版本的 status-go for iOS
   statusgo-ios-simulator          构建 iOS模拟器版本的 status-go

 其他:
   help                            显示此帮助
   generate                        重新生成资产和其他自动生成的内容
   mock-install                    安装模拟工具
   mock                            重新生成 mocks
   clean                           清理

 测试:
   test                            在开发过程中运行基本的简短测试
   test-unit                       运行单元和集成测试
   test-unit-coverage              用覆盖面运行单元和集成测试
   ci                              立即运行所有linters和测试
```

Makefile 命令使用 `build/env.sh` 脚本确保所有内容都构建在 `within build/_workspace`
并且我们不会触及系统方面的Go安装，并且所有供应的依赖关係都会推送到`build/_workspace`.

你也可以使用 `go build` / `go test` 命令手动。

### iOS的构建

使用任一`make statusgo-ios` 或 `make
statusgo-ios-simulator` (后者将构建包含模拟器SDK的iOS库).
再次，iOS中的特定代码在`library.c` 应该由 `#ifdef IOS_DEPLOYMENT`编译.

我们为SDK的版本9.3构建。一旦构建过程完成，请参阅<code> build / bin \</ code\>文件夹中的工件。

### Android的构建

使用 `make statusgo-android`.
为了让绑定能够传达，我们依靠 [JNI](wikipedia:Java_Native_Interface "wikilink"): *Go
→ C → JNI → Java App*.

通讯代码保存在 `library.c`. 我们的*补丁*版本的xgo，使我们能够区分平台：

```
 #ifdef IOS_DEPLOYMENT
 /* iOS related code */
 #else
 /* Android related code */
 #include <jni.h>
 #endif
```

[category:status-go](category:status-go "wikilink")