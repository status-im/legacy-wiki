## 动机

> *“Any fool can write code that a computer can understand. Good
> programmers write code that humans can understand.” – Martin
Fowler*

在编写代码时，我们一定会牢记功能和非功能需求的实现。所以我们经常关注如何快速并且享受複杂的解决方桉。但是，随着代码的增长，团队也在不断增长，发现错误并且必须修复，并且影响现有代码的新需求会进入我们的产品中。所以我们必须记住，我们的代码将被读取，并且稍后必须被其他人理解。时刻记住你在尝试进入一个複杂且糟糕的可维护代码库时的感受，并儘力避免这种情况。

## 通用标准和公约的介绍

以下外部文件涵盖了编写优质代码的技术方面以及常见的编码规范。特别是Google自己的约定对我们的代码是强制性的。

  - Google关于高效Go和编码约定的最重要文档是 [Effective
    Go](https://golang.org/doc/effective_go.html)
  - 补充 *Effective Go* 是 [Go Code Review
    Comments](https://github.com/golang/go/wiki/CodeReviewComments)
  - 对于新的Go开发者s *Kyle Quest* 写[50 Shades of Go: Traps, Gotchas, and
    Common Mistakes for New Golang
    Devs](http://devs.cloudimmunity.com/gotchas-and-common-mistakes-in-go-golang/)

## 特定Status约定

### 评论

  - 可以查看[GoDoc](https://godoc.org/github.com/status-im/status-go)中生成的Web文档，您可以看到应该添加或更好地制定多少文档。
  - 因此，至少要添加关于公共包和文档评论的评论，如[commentary](https://golang.org/doc/effective_go.html#commentary)中所述。特别是不仅要考虑一个小句子作为输入句子，而且要考虑更多有关共同性和动机的语义信息。
  - 比评论更好的是，只有公共标识符才会评论私人标识符以帮助代码维护者。
  - 在函数和方法的内部，单独的部分应该引入一个支持代码内部方向的帮助评论。
  - 当离开TODO评论时，请添加您的Slack暱称（`//
    TODO（themue）测试退出条件与极端值。`）。这有助于其他人员在需要时联繫适当的人员进行阐述。

### 测试

  - 单元测试使用[Testify](https://godoc.org/github.com/stretchr/testify)完成。它基于标准测试库，但更加灵活和冗长。
  - 测试应该测试你期望他们测试什麽，不应该测试不相关的代码。
  - 测试功能最好以arrange/act/assert格式构成。 这将统一测试并帮助在其内部进行导航。

## 程序约定

### 提交

  - 何时情况下要做 `git pull --rebase` (请看 [Git - When to Merge vs. When to
    Rebase](https://www.derekgourlay.com/blog/git-when-to-merge-vs-when-to-rebase/))
  - 保持提交消息的重点 (请看 [How to Write a Git Commit
    Message](https://chris.beams.io/posts/git-commit/))

### 拉取请求

  - 合併[develop](https://github.com/status-im/status-go)分支后自行查看您的代码，并确保一切都取决于您的代码质量栏
  - 确保构建 [Travis](https://travis-ci.org/status-im/status-go/branches)成功
  - 确保测试通过
  - 确保`go fmt` `go lint`，`go
    vet`不会产生任何警告;修复警告或在需要时添加忽略规则
  - 如果您有权创建分支，请确保您从同一个存储库发送拉取请求
  - 如果您发送拉取请求以徵求建议，请明确地向评论者说明，以便他或她仅查看回答问题的必要条件

## 旧的

*将被重写和迁移*

### 建议

  - 使用`testify`进行测试，它比默认的测试库更详细。
  - 测试应该测试你期望他们测试什麽，不应该测试不相关的代码。状态(Status)中的大量测试需要重构。
  - 测试优选採用`Arrange-Act-Assert`格式。它为他们带来了一个统一的结构，并创造了更好的导航捕眼器，例如：这可能看起来像头顶上的，但它带来了结构，迫使你为不同的检查编写不同的测试，并长期证明富有成效。

<!-- end list -->

``` go
  func (s *JailTestSuite) TestParse() {
      // Arrange.
      require := s.Require()
      extraCode := `
      var _status_catalog = {
          foo: 'bar'
      };
      `

      // Act.
      response := s.jail.Parse("newChat", extraCode)

      // Assert.
      expectedResponse := `{"result": {"foo":"bar"}}`
      require.Equal(expectedResponse, response)
  }
```

从`@anna`
> (*质量保证*):

> 顺便说一句，我还喜欢Jan的风格在PR摘要内容中进行修复，例如https://github.com/status-im/status-react/pull/1509，它解释了原因和解决方法。所以，我可以在验证错误的同时获得更多的见解。它可能不适用于所有的修补程序，但我会很感激，如果有一些关于如何修复bug的说明。我不会查看每个PR更改的文件，也不会了解代码中的所有内容，因此一些摘要很有用。

[Category:Status-go](Category:Status-go "wikilink") [Category:Developer
Documentation](Category:Developer_Documentation "wikilink")