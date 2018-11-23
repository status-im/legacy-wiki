# 初始化

耳语在[这裡](https://github.com/status-im/status-react/blob/cede0957467739fb660f70944dd0fd2f2580e438/src/status_im/protocol/core.cljs#L78)
我们的 `core.cljs` 文件中被初始化。

## 为所有群组创建过滤器

为了做到这一点，我们首先使用组ID作为密码来生成对称密钥(`shh.generateSymKeyFromPassword`).
目前，该密钥仅用作群聊中消息的“主题”（这就是我们如何区分群聊的方式）。在whisper
v2中，我们被允许使用任意长度的主题，这就是为什麽可以将 `topic`
选项传递给`shh.newMessageFilter`. 的原因。但在耳语v5中 `topic`
只能有4个字节，这对我们来说是不够的。

当我们有对称密钥时，我们可以开始使用`shh.newMessageFilter` 和下一个参数来开始收听新消息:

``` json
{
 "sym": "generated sym key",
 "topics": ["hardcoded status topic 0xaabb11ee"]
}
```

## 为1-1聊天创建过滤器

使用相同的 `shh.newMessageFilter` ，参数为

``` json
{
 "asym": "user's public key",
 "topics": ["0xaabb11ee"]
}
```

对于所有传入的1-1消息，只会创建一个过滤器，因为我们可以通过其签名区分发件人。

## 为配置文件更新创建过滤器

与组消息过滤器不同，我们知道谁将发送消息，并且可以在这裡使用`sig`
选项。当消息被广播给所有联繫人时，仍使用对称密钥。所以`shh.newMessageFilter`
参数是：

``` json
{
 "sig": "user's public key",
 "sym": "key generated from hardcoded string 'status-discovery'",
 "topics": ["0xaabb11ee"]
}
```

## 创建交付循环(将在单独的文档中描述)

## 发送`:online` 消息

**参数**

``` clojure
{
 ;; web3 object
 :web3                        web3
 ;; user's public key
 :identity                    public-key
 ;; vector of groups, where group is {:group-id "id" :keypair {:private "" :public ""}}
 :groups                      groups
 ;; function which will be called on incoming message,
 ;; first parameter is message type, second payload
 :callback                    #(dispatch [:incoming-message %1 %2])
 ;; if message require ack and it wasn't received during :ack-not-received-s-interval after previous
 ;; attempt to send message message will be sent again. Should be bigger than default-ttl.
 ;; In seconds.
 :ack-not-received-s-interval 125
 ;; default ttl for all messages in seconds
 :default-ttl                 120
 ;; interval for sending :online message
 :send-online-s-interval      180
 ;; custom ttl config for specific message types
 :ttl-config                  {;; we don't expect to receive acc for  :public-group-message, that's why ttl is
                               ;;bigger  than default. This increases chances for message
                               ;; delivery, though guaranties nothing.
                               :public-group-message 2400}
 ;; Number of attempts of sending message if it requires ack.
 :max-attempts-number         3
 ;; Interval for delivery loop. Details will be described in separate doc.
 :delivery-loop-ms-interval   500
 ;; Keypair which is used for sending profile updates and discovery
 :profile-keypair             {:public  updates-public-key
                               :private updates-private-key}
 ;; messages from db that require ack but haven't received it
 :pending-messages            (pending-messages/get-all)
 ;; all contacts which can send profile updates/discovery
 :contacts                    all-contacts
 ;; callback for errors in `shh.post` call
 :post-error-callback         #(dispatch [::post-error %])}
```

# 交付

1.  对于每条新消息，我们创建一个： `:pending-message` 实体的条目并将其存储在 `db`中
2.  每个新消息都被添加到 `status-im.protocol.web3.delivery/messages` 地图。
3.  来自`db`的\#All `:pending-message`
    项被添加到`status-im.protocol.web3.delivery/messages`在耳语(whisper)初始化。
4.  来自`status-im.protocol.web3.delivery/messages`
    的所有消息均通过Whisper在递送循环内发送。
5.  从`:pending-message`和`status-im.protocol.web3.delivery/messages`中删除了不需要`ack`的消息成功调用`shh.post`。
6.  只有收到`ack`消息后，才会从
    `pending-message`和`status-im.protocol.web3.delivery/messages`。

# 交付循环

交付循环在我们`delivery.cljs` 中启动
[here](https://github.com/status-im/status-react/blob/43a5d91ca2702cfb63cc57c2eac2c5193d77aa8f/src/status_im/protocol/web3/delivery.cljs#L204)
文件中。

**参数:**

``` clojure
{;; function which sends :online message
 ;;https://github.com/status-im/status-react/blob/cede0957467739fb660f70944dd0fd2f2580e438/src/status_im/protocol/core.cljs#L112
 :online-message    fn
 ;; other parameter are same as for whisper initialisation
 :delivery-loop-ms-interval ...
 :ack-not-received-s-interval  ...
 :max-attempts-number ...
 :default-ttl ...
 :send-online-s-interval ...}
```

交付循环以`delivery-loop-ms-interval` 间隔运行, 预设是500ms.

为了检查是否应在当前递送循环中发送消息，我们使用
<https://github.com/status-im/status-react/blob/develop/src/status_im/protocol/web3/delivery.cljs>
函数来检查消息。，如果发生以下情况，将发送消息：

1.  以前没有成功通过 `shh.posts` 发送(在当前会话期间)，我们重试执行此操作`max-attempts-number * 5`
    次
2.  已成功发送，但需要未收到 `ack`
      - 先前尝试发送消息(在当前会话期间)超过 `ack-not-received-s-interval`秒前完成
      - 我们没有超过 `max-attempts-number`
以前的尝试(在当前会话期间)

# 关键

密钥类型`asym`（非对称）意味着非对称密钥将在发送之前用于悄悄的加密消息，在我们的例子中它总是用户的公钥，这意味着当前具有`asym`键类型应该直接从`UserA`发送到
`UserB`，而不是任何用户组。

密钥类型`sym`（对称）表示对称密钥将用于加密消息。目前所有的`symkey`都是从硬编码密码或是组的ID生成的，所以这是不安全的，必须改变。

|                                                                               |                                        |                  |
| ----------------------------------------------------------------------------- | -------------------------------------- | ---------------- |
| **Type**                                                                      | **Description**                        | **Key type**     |
| [:message](Whisper_Messaging#:message "wikilink")                             | 1-1聊天消息                                | asym             |
| [:group-message](Whisper_Messaging#:group-message "wikilink")                 | 群组消息                                   | sym (group's id) |
| [:public-group-message](Whisper_Messaging#:public-group-message "wikilink")   | 公共群组消息                                 | sym (group's id) |
| [:ack](Whisper_Messaging#:ack "wikilink")                                     | 确认消息传送                                 | asym             |
| [:seen](Whisper_Messaging#:seen "wikilink")                                   | 确认消息被看到                                | asym             |
| [:group-invitation](Whisper_Messaging#:group-invitation "wikilink")           | 由管理员发送                                 | asym             |
| [:update-group](Whisper_Messaging#:update-group "wikilink")                   | 目前此消息是在某个用户发送时发送的 从群组中删除，结果群组的密钥对被更新   | asym             |
| [:add-group-identity](Whisper_Messaging#:add-group-identity "wikilink")       | 是在将新用户添加到组时发送的                         | sym (group's id) |
| [:remove-group-identity](Whisper_Messaging#:remove-group-identity "wikilink") | 当某个用户从群组中删除时(通过管理员)发送， 之后：发送带有新密钥对的更新组 | sym (group's id) |
| [:leave-group](Whisper_Messaging#:leave-group "wikilink")                     | 在用户离开群组时发送                             | sym (group's id) |
| [:contact-request](Whisper_Messaging#:contact-request "wikilink")             | \-                                     | asym             |
| [:discover](Whisper_Messaging#:discover "wikilink")                           |                                        | sym              |
| [:discoveries-request](Whisper_Messaging#:discoveries-request "wikilink")     |                                        | sym              |
| [:discoveries-response](Whisper_Messaging#:discoveries-response "wikilink")   |                                        | sym              |
| [:profile](Whisper_Messaging#:profile "wikilink")                             |                                        | sym              |
| [:update-keys](Whisper_Messaging#:update-keys "wikilink")                     |                                        | sym              |
| [:online](Whisper_Messaging#:online "wikilink")                               |                                        | sym              |

## :message

``` clojure
{:message-id    "1498723691404-7711c1b3-411c-5e4b-a64b-dc0ab2317c5a",
 :requires-ack? true,
 :type          :message,
 :timestamp     1498723691404,
 :content       "123",
 :content-type  "text/plain",
 :clock-value   1,
 :show?         true}
```

## :group-message

``` clojure
{:message-id    "1498724252836-24753a20-b67e-50ae-8fcd-95b22a1ee78b",
 :group-id      "1498723975430-5a3f18f5-189e-5966-b159-bdf837d47e58",
 :content       "{\"iv\":[2071556246,-1120403611,-1962404966,921696242],\"v\":1,\"iter\":1000,\"ks\":128,\"ts\":64,\"mode\":\"ccm\",\"adata\":\"\",\"cipher\":\"aes\",\"ct\":[-1840979203,1955718739,-1431291835,8794767622144],\"tag\":\"75b9b052a2c2835d8ae9525bd6ae3245e879f4b4b8b510fca22b4ad142a2f10913a83c6ff87e80ef766e194d5017cd1eca6c27fb41611cf9c8d8cf3a5df8980b6959d6d66bbfc9e4cf8a6af27decc1663f60a9e201992236a9ea21b2ccab7284\"}",
 :username      nil,
 :type          :group-message,
 :show?         true,
 :clock-value   1,
 :requires-ack? true,
 :content-type  "text/plain",
 :timestamp     1498724252865}
```

## :public-group-message

``` clojure
{:message-id    "1498724827175-9c097aa0-d859-5ddd-bdd1-0c83e52d930e",
 :group-id      "wow",
 :content       "123",
 :username      "Name",
 :type          :public-group-message,
 :show?         true,
 :clock-value   1,
 :requires-ack? false,
 :content-type  "text/plain",
 :timestamp     1498724827202}
```

## :contact-request

``` clojure
{:message-id    "1498723171063-c3ecb16a-b3fd-5ee1-99f0-783ef7a1070c",
 :requires-ack? true,
 :type          :contact-request,
 :contact       {:name          "Yearly Onerlooked Westafricanantelope",
                 :profile-image "..."},
 ;; public key is used by UserA (who sends this request) for broadcasting changes of his profile to UserB
 ;; (and other contacts)
 :keypair       {:public  "..",
                 :private ".."},
 :content       nil}
```

## :online

``` clojure
{:message-id    "1498723263025-a83a4fe0-eff6-518d-9348-2570d1494067",
 :requires-ack? false,
 :type          :online,
 :content       "{\"iv\":[1966114636,-1876596685,-393541684,-299376444],\"v\":1,\"iter\":1000,\"ks\":128,\"ts\":64,\"mode\":\"ccm\",\"adata\":\"\",\"cipher\":\"aes\",\"ct\":[-1468463324,357257435,1338099857,1642960690,-1068289456,705936716,-1541701378,-565102533,26389113252352],\"tag\":\"3151bd11093fdeb90ccdf311b660386b2edb959a5c52d9ed63095fb9ac9bf3d9bdb7ce3841dce1d2820c04d43ca2548c171c4049e06e498b35e6bea3e210afd8dfb32437663e1497a8d3ed7772b72160edc69f124e69e2385ca19cfc5692c7f9\"}"}
```

## :profile

``` clojure
{:message-id "1498723799459-6642807d-2b32-58cd-98d6-19359ad3d8da",
 :type       :profile,
 :timestamp  1498723799465,
 :content    "encrypted content: username, status, photo etc"}
```

## :group-invitation

``` clojure
{:group-admin   "0x04e40d8b966a6644bb8ef2a5c1e170c7c0ef9ba83e8d0cb2e2f07930d092af51cdec3aab1c97b1c451460d0e3525fd5b079ab32ba1ff179fc0db2f047ddb71a703",
 :message-id    "1498723975437-09f7ca06-2ffe-5067-87d0-a65d128efb31",
 :contacts      ["0x0428c9d6c1aaaa8369a7c63819684f30e34396dc0907d49afeac85a0a774ccb919b3482097d992e66bcc538e7a0c6acf874c77748f396f53c0a102e10d1a37765b"
               "0x04e40d8b966a6644bb8ef2a5c1e170c7c0ef9ba83e8d0cb2e2f07930d092af51cdec3aab1c97b1c451460d0e3525fd5b079ab32ba1ff179fc0db2f047ddb71a703"],
 :group-id      "1498723975430-5a3f18f5-189e-5966-b159-bdf837d47e58",
 :content       nil,
 :type          :group-invitation,
 :requires-ack? true,
 :group-name    "T",
 ;; this keypair is used for sending messages to all group members, so that any user could encrypt/decrypt message
 :keypair       {:public  "..",
                 :private ".."},
 :timestamp     1498723975439}
```

## :discoveries-request

``` clojure
{:message-id "1498724283011-65da6bd7-1cb0-566f-a055-27d66cbd04e4",
 :type       :discoveries-request,
 :content    nil}
```

## :remove-group-identity

``` clojure
{:message-id    "1498724483015-0d468376-69f5-551c-a6c1-3762f7cb9651",
 :requires-ack? true,
 :type          :remove-group-identity,
 :identity      "id",
 :group-id      "1498723975430-5a3f18f5-189e-5966-b159-bdf837d47e58",
 :username      nil,
 :timestamp     1498724483015,
 :content       nil}
```

## :update-group

``` clojure
{:group-admin   "0x04e40d8b966a6644bb8ef2a5c1e170c7c0ef9ba83e8d0cb2e2f07930d092af51cdec3aab1c97b1c451460d0e3525fd5b079ab32ba1ff179fc0db2f047ddb71a703",
 :message-id    "1498725030208-c44f66bb-4d9c-5022-b992-eb663511792a",
 :contacts      ("0x04e40d8b966a6644bb8ef2a5c1e170c7c0ef9ba83e8d0cb2e2f07930d092af51cdec3aab1c97b1c451460d0e3525fd5b079ab32ba1ff179fc0db2f047ddb71a703"              "0x0428c9d6c1aaaa8369a7c63819684f30e34396dc0907d49afeac85a0a774ccb919b3482097d992e66bcc538e7a0c6acf874c77748f396f53c0a102e10d1a37765b"),
 :group-id      "1498723975430-5a3f18f5-189e-5966-b159-bdf837d47e58",
 :content       nil,
 :type          :update-group,
 :requires-ack? true,
 :group-name    "T",
 :keypair       {:private "..", :public  ".."},
 :timestamp     1498725030220}
```

## :add-group-identity

``` clojure
{:message-id    "1498725030221-3a4f2890-3897-546b-8819-c240d99089a3",
 :requires-ack? true,
 :type          :add-group-identity,
 :identity      "0x0428c9d6c1aaaa8369a7c63819684f30e34396dc0907d49afeac85a0a774ccb919b3482097d992e66bcc538e7a0c6acf874c77748f396f53c0a102e10d1a37765b", :group-id "1498723975430-5a3f18f5-189e-5966-b159-bdf837d47e58",
 :username      nil,
 :timestamp     1498725030222,
 :content       nil}
```

## :seen

``` clojure
{:message-id    "1498724483032-ef9f6889-5699-5603-9ffd-05bee945f146",
 :requires-ack? false,
 :type          :seen,
 :group-id      "1498723975430-5a3f18f5-189e-5966-b159-bdf837d47e58",
 :content       nil}

{:message-id    "1498725866330-4a77d7d9-5d18-5628-8cf4-1f7d862f5122",
 :requires-ack? false,
 :type          :seen,
 :group-id      nil,
 :content       nil}
```

## :leave-group

``` clojure
{:message-id    "1498725404895-36369ea8-1403-5554-b921-c58aaa8d1e0a",
 :requires-ack? true,
 :type          :leave-group,
 :group-id      "1498723975430-5a3f18f5-189e-5966-b159-bdf837d47e58",
 :username      nil,
 :timestamp     1498725404895,
 :content       nil}
```

## :discover

``` clojure
{:message-id "1498725562980-10a58c17-b994-5dd3-87b1-cbdc30533df0",
 :type       :discover,
 :status     "the #life we're given is on a thread, so wear it well",
 :hashtags   ["life"],
 :profile    {:name         "Name",
             :profile-image "data:image/png;base64,..."},
 :content    nil}
```

## :discoveries-response

``` clojure
{:message-id "1498725718717-2c483805-da99-568b-8958-1e9a54d30630",
 :type       :discoveries-response,
 :data       [{:message-id "1498725562980-10a58c17-b994-5dd3-87b1-cbdc30533df",
               :name       "Name",
               :status     "the #life we're given is on a thread",
               :whisper-id "0x04e40d8b966a6644bb8ef2a5c1e170c7c0ef9ba83e8d0cb2e2f07930d092af51cdec3aab1c97b1c451460d0e3525fd5b079ab32ba1ff179fc0db2f047ddb71a703",
               :photo-path "data:image/png;base64,...",
               :tags       ({:name  "life",
                             :count 1}),
               :created-at 1498725562990}],
 :content    nil}
```

## :ack

``` clojure
{:message-id     "1498725867070-78e60ec0-266e-5dba-a9f6-e53a333f38c7",
 :type           :message,
 :ack?           true,
 :ack-of-message "1498725866330-4a77d7d9-5d18-5628-8cf4-1f7d862f5122",
 :group-id       nil,
 :content        nil}
```

[Category:Status Architecture](Category:Status_Architecture "wikilink")
[Category:Developer
Documentation](Category:Developer_Documentation "wikilink")
[Category:Whisper Messaging](Category:Whisper_Messaging "wikilink")
[Category:Outdated](Category:Outdated "wikilink")