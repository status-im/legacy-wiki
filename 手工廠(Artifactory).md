對於核心貢獻者：

Artifactory是我們承載手工制品的地方。
[here](http://139.162.11.12:8081/artifactory/webapp/#/home)可以找到

管理員詳細信息在Lastpass中。

## 如何修復像“功能”失踪的手工制品？

1.  `Git clone `<https://github.com/status-im/function>
2.  `mvn package`
3.  產生 \`target/function-0.0.1.jar\` 並上傳到Artifactory
4.  按左邊的'Artifacts' 並轉到 `libs-release-local/status-im/function/0.0.1`
5.  點擊部署，檢查目標儲存庫(repo)是 `libs-release-local` ，目標路徑是正確的。
6.  添加生成的jar文件並確認

[Category:Developer
Documentation](Category:Developer_Documentation "wikilink")
[Category:Miscellaneous](Category:Miscellaneous "wikilink")