# 这份文件的目标

以清晰易懂的方式描述新的协议代码（"鸟瞰图"，每种不同的消息类型以及它们在解释的操作方面代表什麽）
详细描述每种消息类型，包括封装在其中的所有信息（只要添加了需要协议版本inc的新功能即可更新）
描述新协议的安全机制，其中使用了sym/asym密钥，它们如何更新/刷新，我们如何选择耳语话题等。

# TODOS

  - 制定基准
  - 编写测试
  - 实施acks, nacks
  - 讨论传输层作为外部库
  - 从status-react方删除加密库
  - 删除nodeify黑客
  - 清理领域(挂起消息，无用的领域...)

# 情景示例

## 1对1聊天

A将B添加为联繫人并发送消息。
B接受A作为联繫人并将他的名字改为C

注意：目前，不考虑多设备支持。但是，A和B有一个单一主题的事实使A有可能在其他设备上接收他的消息，而旧协议中的消息以非对称加密方式发送时不可能。

![Newstatuscommunicationprotocol_image1.png](Newstatuscommunicationprotocol_image1.png
"Newstatuscommunicationprotocol_image1.png")

## 公共聊天

除了用于发布消息和命令的常规消息之外，公共聊天不需要任何特定的消息类型。由于它们是公开的，我们无法从外部隐藏任何东西，但我们仍然使用对称加密来限制窃听者无需主动查找公共聊天内容的能力。公共聊天的对称密钥使用chat-id作为密码，主题基于chat-id（使用web3
sha3方法并取前8个字节）。

要加入公开聊天，A只需要知道用作聊天ID的公开聊天的名称，并使用以chat-id作为密码生成的对称密钥来过滤衍生的主题。

## 群组聊天

群组聊天由管理员创建。他从他的联繫人列表中选择参与者。邀请使用与管理员联繫时共享的相同主题和同一个密钥分别发送给每个参与者。
邀请中包含GroupAdminUpdate消息，其中包含聊天名称以及包含在NewGroupKey消息中的参与者身份列表，该消息提供将用于群聊的聊天ID和对称密钥。

A与他的联繫人中的B和C创建群聊。他们都收到包裹在NewGroupKey消息中的GroupAdminUpdate。
A，B和C使用聊天主题和对称密钥将消息交换给群组聊天。 A从聊天中删除B.
C会收到通知，B已从包含在NewGroupKey消息中的GroupAdminUpdate聊天中删除，对称密钥已更改，因此B不能再跟随该聊天。此消息以与A分享的主题进行1对1聊天。
A将D添加到聊天中。
C收到一个GroupAdminUpdate，再次包装在一个NewGroupKey消息中，对称密钥被更改，以便D在加入之前无法看到聊天中说的内容。
A，C和D使用聊天主题和对称密钥将消息交换到群组聊天。 C离开聊天。 C在组主题中发送GroupLeave消息。
A发送GroupAdminUpdate包裹在一个NewGroupKey消息中，以更新对称密钥，以便C不能再跟随该对话。

总而言之，消息以基于管理员创建组时生成的聊天ID的主题发送。每当参与者列表发生变化时，管理员就会通过直接向每个参与者发送新密钥来更改对称密钥。

# 消息类型

有效载荷以传输格式编码，这基本上是JSON。这使得Javascript和Go易于创建和接收来自应用程序的消息。它还节省了大量的空间，因为记录被编码为数组而不是消息之间具有冗馀密钥的映射。
我们可能会考虑使用msg-pack来进一步减少传输的字节数。

Transit可以很容易地从js中使用:
<https://github.com/swannodette/transit-js-example/blob/master/shared/handlers.js>

消息有效内容是一个JSON向量，消息类型标记作为第一个元素，而向量的参数作为第二个元素。

## NewContactKey

发送给未来联繫人的第一封邮件。此时，我们知道联繫人正在过滤的唯一主题是使用其公钥进行发现的主题，以便NewContactKey将被发送到哪个主题。它包含将用于未来通信的sym-key和主题以及我们想发送的实际消息。同义词和主题是随机生成的，因为我们不希望主题与其参与者之间存在任何关联以避免元数据洩露。
当其中一个联繫人恢复其账户时（联繫人的账户恢复目前尚未实施并且是正在进行的群体），则还会发送NewContactKey消息以更改对称密钥和主题。

### 参数

sym-key: 对称密钥将用于进一步的通信。 Signal使用x3dh作为密钥协议协议:
<https://signal.org/docs/specifications/x3dh/>
，不幸的是这是一个基于服务器的解决方桉，需要公钥基础设施，但我们可能想在将来调查一个无服务器的替代方桉。我们也可以允许用户输入一个可选的离线秘密。
topic: 将用于进一步通信的主题 message: 包裹在NewContactKey消息中的消息（现在只有ContactRequest）

### 编码

注意:该示例中的消息字段是一个类型为〜＃c2的消息，它是一个ContactRequest

    [
      "~#c1",
      [
        "0xc2a38488c043b7c0d138e922887bfcc2966c70849774b6d815596737cc654ee1",
        "0x2d215ad8",
        [
          "~#c2",
          [
            "eric sim",
            "data:image/png;base64,base64-representation-of-profile-image...",
            "e5ac45596d8be7aabea51c0dbbac3494e6a1636d",
            "dFLRZwPx3yQ:APA91bH_7yjq47N5HY0ciKs58HW3VbJ9...”
          ]
        ]
      ]
    ]

## 联络请求

发起联繫请求时，包装在NewContactKey消息中。

注意：在协议的第一个版本中，ContactRequest被另一个ContactRequest回复，使得它在双方之间成为有状态的交互，并且在处理ContactRequest时具有不必要的複杂性。此外，由于新协议在初始ContactRequest后使用了专用主题，因此最好为请求和响应提供两种不同的消息类型。
先前已删除的字段包括message-id，require-ack？，类型，联繫人，密钥对和内容。
现在基于序列化消息有效载荷的sha3来计算消息ID。需求 -
答桉？是不需要通过电线发送的带外信息。该类型已知，这是一条消息。联繫人是包含发送请求的用户的姓名和个人资料图的地图，这些用户现在位于记录的顶层。密钥对是clojure侧使用的密码术的一部分，在新协议中完全删除。内容总是零。

## 参数

\= name: 联繫人的姓名 profile-image: 联繫人的个人资料图片 address: 交易联繫人的以太坊地址。
注意：地址来源于扩展密钥的公钥，这是窃窃私语的身份，所以我们有签名时的地址。因此这个领域可以被忽略 .
fcm-token: 通知联繫人的fcm-token 注意：一旦实施，推送通知V2将需要更新：
联繫人A将广播具有某个知名主题的消息，通知提供商将知道如何解密。此处的目标是检索一个特殊主题，以便我们与提供联繫人B的特定通知提供商进行通信。
当联繫人A想要将消息发送到联繫人B时，它会像往常一样发送消息，然后使用收到的主题和联繫人B的发布键向联繫人B的通知提供者发送单独的消息。通知提供商然后将负责使用FCM或其他提供商发送通知。

### 编码

由于我们需要发送对称密钥和主题以供将来进行通信，因此此消息始终嵌入在 [NewContactKey
message中](NewContactKey_message "wikilink")

## ContactRequestConfirmed

这是联繫人接受联繫请求时将发送的消息。它将根据NewContactKey消息中提供的主题发送并使用sym键。这两个用户将因此具有相同的过滤器。

### 参数

参数与ContactRequest相同

### 编码

    [
      "~#c3",
      [
        "Eric android",
        "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAA...",
        "843bacc6c484e622da922aab3eaa08ac568d9986",
        "dFLRZwPx3yQ:APA91bH_7yjq47N5HY0ciKs58HW3VbJ9EdEJM..."
      ]
    ]

## ContactUpdate

当用户更改他的名字或个人资料图片时发送。

### 参数

name: 联繫人的姓名 profile-image: 联繫人的个人资料图片

### 编码

    [
      "~#c6",
      [
        "eric sim",
        "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACgAAAAoCAMAAAC7IEhfAAA..."
      ]
    ]

## 信息

两个用户之间交换的典型消息。

注意：在协议的第一个版本中，消息本身就是一张地图，这些字段保留在不必要的地方，例如发送者已经包含在更广泛的私语消息中，并且更可靠，因为这个可以是伪造不像耳语消息的签名。
先前已删除的字段是message-id，require-ack？，type和show？ 现在基于序列化消息有效载荷的sha3来计算消息ID。需求
- 答桉？是不需要通过电线发送的带外信息。该类型已知，这是一条消息。显示？被删除，因为它没用。

### Parameters

  - content: the content of the message (a map or a string for now)
  - content-type: the content-type of the message
  - message-type: the type of the message
  - clock-value : the clock of the sender for message ordering
  - timestamp: the timestamp of message in ms

### Encoding

    [
      "~#c4",
      [
        "hi",
        "text/plain",
        "~:user-message",
        1,
        1522194627268
      ]
    ]

## 看

当用户看到一条消息（打开聊天并加载消息）时发送。可以同时确认多条消息

注意：在协议的前一版本中，发送消息的方式非常相似，只是当聊天被打开时，几乎同时发送了N条消息，每条消息的发送数量为1，这在以前的聊天中没有看到。不同的是，现在我们只发送1条包含这些消息ID的N消息消息。

### 参数

message-ids: 用户已经看到的消息的id的向量

### 编码

    [
      "~#c5",
      [
        "0xfebd2cb7aa6d77df5468381d014c9d1663549e7f43e9aabfefe45e338caf76c5",
        "0xf0f3871715c97dd964ef079e1b1eefa5df6c3ebb13d03bb3835a0e74df97bc74"
      ]
    ]

## NewGroupKey

该消息被单独发送给每个参与者。由于组的创建者/管理员只能将他的联繫人添加到组中，这意味着他已与每个组成员进行1对1的聊天，并且只有对称密钥和随机主题。
该组将使用的主题由chat-id（使用web3 sha3方法并取前8个字节）确定，该组由管理员随机选择。
由于消息包含chat-id和sym-key参与者有足够的信息来跟踪组中未来的消息。
NewGroupKey消息还包含具有聊天名称和参与者列表的GroupAdminUpdate消息。

### 参数

  - chat-id: 该聊天的ID，由该群组的管理员随机生成
  - sym-key: 该对称密钥将用于基于chat-ID来加密群组特定主题中的进一步通信
  - message:
担保新对称密钥的GroupAdminUpdate消息或通知参与者创建组的初始GroupAdminUpdate消息。

### 编码

## GroupAdminUpdate

此消息由组的管理员专门发送，因此应忽略其他参与者发送的所有GroupAdminUpdate。它包含聊天名称和参与者列表，并在其中一个信息发生更改时发送，因为该组已被重命名或有人离开/加入/被删除。
当从管理员接收到该消息时，参与者将其与他们当前的参与者列表进行比较，以知道谁离开/加入/从组中移出，这可能是他们自己的。

### 参数

chat-name: 显示给每个参与者的聊天名称 participants:
参与者的whisper身份。

### 编码

## GroupLeave

当用户离开群组聊天时发送。当发生这种情况时，组的管理员将更改对称密钥和主题，以便离开的用户不能再跟随对话。这将通过NewGroupKey消息来完成，该消息将包含包含参与者更新列表的GroupAdminUpdate消息。

### 参数

该消息没有参数

### 编码

# Message ids

讨论消息

  - 可以很容易地为第三方计算这些消息

我们可以使用耳语消息标识，这有什麽问题吗？

它是否可以从状态反应中获取？

我们需要一个特殊的消息类型来包装遗漏的消息，以便在nack的情况下我们仍然可以拥有原始的消息ID

# 在Status实施情况

传输器名称空间和常规应用程序名称空间之间的清晰分离使我们能够大大简化应用程序代码。因此，在中期内，众所周知的複杂聊天命名空间更易于理解和进一步改进。我们的想法是，如果我们切换协议并使用PSS，只需要更改传输名称空间中的代码，并且其馀代码可以保持不变。

耳语消息的有效载荷是实现StatusMessage协议的记录。 StatusMessage协议有两种方法，发送和接收。
在新创建的记录上调用发送，该事件需要发送消息并返回效果图。
在接收到：receive-whisper-message事件中的消息时，接收将在记录上被调用，并返回效果图。

传输器命名空间的组织如下：

    transport
    ├── core.cljs                              ;; functions to start and stop whisper
    ├── db.cljs                                 ;; specs for db
    ├── filters.cljs                            ;; code for manipulating shh filters
    ├── handlers.cljs                       ;; code for receive-whisper-message
    ├── inbox.cljs                            ;; offline inboxing
    ├── message
    │   ├── core.cljs                        ;; defines the StatusMessage protocol
    │   ├── transit.cljs                     ;; transit readers/writers
    │   └── v1                                 ;; StatusMessage implementations
    │       ├── contact.cljs
    │       ├── group_chat.cljs
    │       ├── protocol.cljs
    │       └── public_chat.cljs
    ├── message_cache.cljs          ;; message cache for deduplication
    ├── shh.cljs                              ;; utils for shh functions
    └── utils.cljs                             ;; utils for transport layer

以下是与app-db中的传输层相关的密钥，您通常会在聊天列表中使用1对1聊天工具看到这些密钥：

    :transport/chats {"0x043799de5117592183cf83f677bf354fe5eb9467ed948fe65d7bf9cddf298c09a8bac5f803d41cd1afece8708c3d3836ec61791772b469ead60c41c292b6dcc683"
    {:topic"0x31303369"
    :sym-key-id"d069f3bfe6aaafe463f9f4a47e91d6575db16e3d4e68e4db96c18b17d45532c9"
    :sym-key"0xe63848048f405a97027999c269d862b80ca93eb25097bffc791f35e36485af5e"
    :filter[TaggedValue: ForeignType, ]}
    :transport/discovery-filter [TaggedValue: ForeignType, ]

  -
    transport/chat
    是以chat-id作为关键字的地图。主题和sym-key-id是传输层用于基于chat-id发送消息的内容。
    sym-key本身就是您在NewContactKey中收到的或添加联繫人时创建的。它现在坚持到领域，直到状态去坚持它。该过滤器在登录后添加并在注销后停止，对于不属于任何特定聊天且始终存在的发现过滤器也是如此。

[Category:Developer
Documentation](Category:Developer_Documentation "wikilink")
[Category:Status Communication
Protocol](Category:Status_Communication_Protocol "wikilink")