# 基本信息

  - 我们依赖于正在大力发展中的“以太坊`go-ethereum`项目”
  - 我们有我们自己的分支 `go-ethereum` repo:
    <https://github.com/status-im/go-ethereum>
  - 我们有时需要从上游提取代码 ( `go-ethereum` 是我们的分支 )
  - 除了从上游提取更改外，我们还需要应用我们的自定义提交 (我们在<code> go-ethereum \</
    code\>项目的顶部需要一些自定义功能):

![f1f7ccae-f84b-11e6-9e36-201984db8b64.png](f1f7ccae-f84b-11e6-9e36-201984db8b64.png
"f1f7ccae-f84b-11e6-9e36-201984db8b64.png")

# 改编(Rebase)工作流程

我们从稳定分支开始（假设<code> a07539fb \</ code\>指向我们想要基于的上游<code> go-ethereum \</
code\>的起点）：

`git co -b status/1.5.9 a07539fb`

现在，我们需要应用我们的自定义提交（它们通常在我们以前的分支中可用，比如说 `status/1.5.8-stable`)

`git cherry-pick "start^..end"` (或者你可以挑取( cherry pick
)一个版本然后再接一个的提交，比方说，如果你想省略一个)

当挑取( cherry pick )时我们将会要处理 **冲突 (conflicts)**.
事实上，我们的自定义更改与以前的版本兼容，并且新版本可能会改变某些内容。冲突解决后，确保运行make编译没有任何问题。

在`go-ethereum`储存库(repository) repository 裡从`status/1.5.9`中创建一个PR去
`develop` .合併后，是更新<code> status-go \</ code\>来源文件夹的时候了。

到`status-go`储存库(repository). 执行 `dep ensure --update
github.com/ethereum/go-ethereum)`. 这会将供应源版本更新为最新更新。

试着做 `status-go`. 它会失败的机会。

如果失败，您需要修复必要的回购问题，并提交更改`供应源: 修补程序修复 (vendor: rebase fixes)`.

现在您已准备好向提交初次文件(code files)到 `status-go` (其中将包含2个提交\!)

[Category:Status-go](Category:Status-go "wikilink")