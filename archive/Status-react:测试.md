本文档概述了改进我们测试策略的选项。

测试原因：以可重现的方式尽快提供关于状态质量的反馈。 （重要的是：反馈，快速，可重複）

质量状态 (*attributes/dimensions*)
功能：它工作吗？它是否适用于特定的设备/操作系统？如果它起作用，则其他质量属性：例如性能;可用性等

手动测试（检查PR中的新功能，使用有限的设备覆盖范围进行开发构建的回归和集成/系统测试，查看新的UI设计实施前）自动UI测试涵盖基本功能（快速获得烟雾覆盖范围，测量性能，电池/内存/网络消耗，在云端平台上的真实设备上运行测试）社区手动测试（额外设备/操作系统/语言覆盖，测试开发建立，对性能/内存/网络使用的反馈，审查新的UI？）防止错误：审查新的故事/功能，用户界面设计和描述测试
- ＆GT;在实施开发人员之前知道将检查什麽。

## 单元测试

验证纯功能的简短和快速测试。可以经常运行。基于`cljs.test`。适当的时候`test.check`是查找不明显的问题的好工具。
可以使用`re-frame-test`来编写特定于`re-frame`的更高级别测试。
每个PR都应该测试新的/更新的功能。高覆盖率不是目标，而是基本功能和极限情况。

## 集成测试

在频谱的另一端，可运输应用程序用更複杂的用户场景进行测试。这些测试也将用ClojureScript编写，以利用现有的代码库。
`Appium`提供了一个我们可以利用的`nodejs`层

## 非回归测试

引入性能和升级测试以确保新版本不会打破客户的期望可能是有用的。

## 值得调查

  - <https://github.com/pixielabs/cavy>
  - <https://github.com/wix/detox>

## 进一步讨论

**TODO:**还要从相关要点中导入相关评论
[这裡](https://gist.github.com/jeluard/4564c04f118b1cfa828ffe741811b0aa)

[Category:Status Application](Category:Status_Application "wikilink")
[Category:Developer
Documentation](Category:Developer_Documentation "wikilink")