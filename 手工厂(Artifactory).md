对于核心贡献者：

Artifactory是我们承载手工制品的地方。
[here](http://139.162.11.12:8081/artifactory/webapp/#/home)可以找到

管理员详细信息在Lastpass中。

## 如何修复像“功能”失踪的手工制品？

1.  `Git clone `<https://github.com/status-im/function>
2.  `mvn package`
3.  产生 \`target/function-0.0.1.jar\` 并上传到Artifactory
4.  按左边的'Artifacts' 并转到 `libs-release-local/status-im/function/0.0.1`
5.  点击部署，检查目标储存库(repo)是 `libs-release-local` ，目标路径是正确的。
6.  添加生成的jar文件并确认

[Category:Developer
Documentation](Category:Developer_Documentation "wikilink")
[Category:Miscellaneous](Category:Miscellaneous "wikilink")