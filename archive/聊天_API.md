# 界面解剖

这解剖学建立聊天界面的不同部分并且建立一个共同的言辞。主要组成部分是：

  - 信息
  - 输入
  - 键盘
  - 建议

![chat-anatomy.png](chat-anatomy.png
"chat-anatomy.png")

# 访问一个机器人

`/global`（hidden）bot的通用命令允许通过`@botname`在任何聊天中引用bot命令。如果存在，它应该出现在命令列表中作为底部的另一部分。

`/init` (hidden)
在开放的聊天会话中运行，只能进行1对1的聊天。

`/`<command> 机器人可以注册任何ASCII标题的命令处理程序，然后状态(Status)重複

`@botname/command`
如果聊天上下文中有多个机器人，则可以使用`@botname`来区分这个区别（或者应该出现一个建议/命令列表来帮助区分）

`Bot Message`
机器人可以发送一条消息，该消息只能在聊天记录中被其自身的另一个实例读取。该消息可以发送标记来绘製和包含数据。它由用户点击它激活。

`A message parsed from chat history`
在1对1聊天中，任何用户发送的消息都可以被机器人读取，并且可以通过`status.on（'chat-update'，function（）{...}）;`处理程序。

# Bot消息

一个Bot消息可以包含它自己的标记和一个数据有效载荷，这个消息可以发送到聊天环境。如果它是可交互的，那麽交互（我们目前称之为响应处理程序）在点击时处理消息。

**如果用户没有安装该机器人，那麽我们将显示一个自定义键盘，显示机器人名称，评论/信誉和 a `Install / Add to
Contacts` 按钮，用户可以取消或安装并继续响应流程。**

# 命令处理程序

一旦调用了一个命令处理程序，它就会传递消息的当前文本/数据负载，并且需要返回参数（及其占位符信息）。 处理程序的返回必须包含建议区域的 -
`markup` 或`status component(data)` for the suggestions area - `markup` 或
`status component(data)` 对于键盘 - 一个错误对象 -
修改后的文本/数据有效载荷。

# 隐私

  - 机器人只能通过命令或消息通过1对1聊天进行交互
  - 一个机器人只能看到自己和当前用户在群聊中的消息。

# 部署＆amp;测试

控制台应该有一些`/debug`模式，允许用户插入手机并热插入javascript/web页面，以便从桌面轻鬆测试基于Web的dapp或机器人。理想情况下，它应该与[Truffle](http://truffleframework.com/)和[Embark](https://github.com/iurimatias/embark-framework)集成，并且尊重[EIP190](https://github.com/ethereum/EIPs/issues/190)

# 打开问题

Otto VM监狱周围是否存在资源和保护问题？它应该每个聊天上下文运行每个dapp吗？或每个DApp，然后每个聊天上下文中有一个对象？

[Category:Chat API](Category:Chat_API "wikilink") [Category:Status
Application](Category:Status_Application "wikilink")
[Category:API](Category:API "wikilink") [Category:Developer
Documentation](Category:Developer_Documentation "wikilink")