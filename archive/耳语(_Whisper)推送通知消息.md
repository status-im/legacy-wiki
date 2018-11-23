注意：历史文档。与截至2017年10月26日的推送通知的实际执行不符。最初由Victor Spring 2017（ish）提供。 -
oskarth

# 概观

这是非常重要了解 **耳语( Whisper) 通知服务** 设计是为了:

  - 通过允许客户订阅启用了服务的任意节点来增强Whisper / 5功能
  - 一旦订阅，客户端将接收随机生成的用于控制订阅（比如查询服务器ID或生成聊天密钥）的对称密钥（`SubscriptionKey`）
  - 使用`SubscriptionKey`，可以启动新的聊天会话（产生另一个SymKey -
    `ChatKey`）。通过与他人共享该密钥，您将拥有可用于触发通知的公共秘密。
  - 尽可能使用Whisper协议本身。例如，发现机制不过是各方之间的Whisper消息交换。通知发送也是如此。目的是尽可能保持服务尽可能轻巧。

## 俯瞰的程序概观

![b3cf02aa-20f8-11e7-882b-1a4958bcda13.png](b3cf02aa-20f8-11e7-882b-1a4958bcda13.png
"b3cf02aa-20f8-11e7-882b-1a4958bcda13.png")

这裡的关键是如何让`DeviceB`在<code> DeviceA \</
code\>上触发通知，所有这些都不知道使用哪个wnode，或者`DeviceA`。而且`Chat
SymKey`可以很好地解决这个问题（漂亮点是在于它可以用于一对一和群聊，所有您需要确保共享一个秘密，即`Chat
SymKey`，以便新手可以将他们的设备添加到订户列表中）。

**重点:**

  - `Device A`
    通过使用发现协议PubKey发送加密消息来启动协议（状态测试群集将在所有节点上安装一个PubKey，允许其中任何一个对发现请求做出响应）。
  - 虽然使用非对称加密技术进行发现，但一旦获得`Subscription SymKey`和`Chat
    SymKey`，您还是要依赖于对称加密。
  - 要了解通知请求消息与正常通信“一起”是很重要的。
      - 在<code> DeviceA \</
        code\>上发送加密信息到`DeviceB`（因此，只有`DeviceB`可以打开信封）
      - 一旦完成，您需要使用`Chat SymKey`发送加密广播消息
      - 那边的一些节点将能够解密你的广播（唯一的节点有`Chat
        SymKey`)
      - 一旦解密，wnode将覆盖先前为此聊天记录的设备ID列表，并使用这些ID向FCM发送通知请求（仅针对一对一聊天的1个请求，`n`聊天组的有`n-1`请求
        - 这是我们不通知自己）
  - 所以消息从不暴露，并且它们并行路由。通知也由Chat
    SymKey加密，因此不会被窃听者知道。这意味着我们不会在偷偷中妥协。以下信息会暴露：
      - 通过注册设备ID与给定的聊天会话，我们将其暴露给 `wnode`
  - 我们公开PubKeys（需要签名的wnodes消息，以便您可以过滤出PubKey，并避免通过自己向自己发送通知）
  - 我们可能会在通知消息的主体中暴露一些关键信息（极不建议！）。

# 通信协议

下面的代码是**过时的**（它是为Geth 1.5.9编写的，并且Geth 1.6.0 Whisper
API剧烈变化）。我们仍建议浏览代码和评论，以更好地理解这个想法。在本文末尾，我们将包含完整的测试套件（将与\[[https://mochajs.org/Mocha\]一起运行](https://mochajs.org/Mocha%5D一起运行)）。

假设您使用`statusd --datadir wnode1 wnode --notify --identity ./wnodekey
--firebaseauth
./firebaseuathkey`启动了服务节点，其中`wnodekey`和`firebaseauthkey`文件分别包含节点的私钥和FCM授权密钥。现在，您的服务器已准备好接收使用PubKey加密的请求，这是我们的群集所使用的`0x040edb0d71a3dbe928e154fcb696ffbda359b153a90efc2b46f0043ce9f5dbe55b77b9328fd841a1db5273758624afadd5b39638d4c35b36b3a96e1a586c1b4c2a`.

在客户端，我们需要使用协议PubKey和预先定义的主题：

``` go
var protocolKey = '0x040edb0d71a3dbe928e154fcb696ffbda359b153a90efc2b46f0043ce9f5dbe55b77b9328fd841a1db5273758624afadd5b39638d4c35b36b3a96e1a586c1b4c2a';
var sendDiscoveryRequest = function (identity) {
    // notification server discovery request is a signed (sent from us),
    // encrypted with Notification Protocol Asymmetric (Public) Key
    var err = web3.shh.post({
        from: identity,
        to: protocolKey,
        topics: [discoverServerTopic],
        ttl: 20
    });
    if (err !== null) {
        console.log("message NOT sent")
    } else {
        console.log("message sent OK")
    }
};
```

以下是主题列表（请参阅完整示例文件，它们全部用于何处）：

``` go
var discoverServerTopic = '0x268302f3'; // DISCOVER_NOTIFICATION_SERVER
var proposeServerTopic = '0x08e3d8c0'; // PROPOSE_NOTIFICATION_SERVER
var acceptServerTopic = '0x04f7dea6'; // ACCEPT_NOTIFICATION_SERVER
var ackClientSubscriptionTopic = '0x93dafe28'; // ACK_NOTIFICATION_SERVER_SUBSCRIPTION
var sendNotificationTopic = '0x69915296'; // SEND_NOTIFICATION
var newChatSessionTopic = '0x509579a2'; // NEW_CHAT_SESSION
var ackNewChatSessionTopic = '0xd012aae8'; // ACK_NEW_CHAT_SESSION
var newDeviceRegistrationTopic = '0x14621a51'; // NEW_DEVICE_REGISTRATION
var ackDeviceRegistrationTopic = '0x424358d6'; // ACK_DEVICE_REGISTRATION
var checkClientSessionTopic = '0x8745d931'; // CHECK_CLIENT_SESSION
var confirmClientSessionTopic  = '0xd3202c5f'; // CONFIRM_CLIENT_SESSION
var dropClientSessionTopic = '0x3a6656bb'; // DROP_CLIENT_SESSION
```

最后，为了测试一切，你最好使用两个不同的客户端，而不是一个。因此，假设我们有两个节点（`Device A`和`Device
B`），它们具有不同的底层Status守护进程：

``` go
var web3 = new Web3();
web3.setProvider(new web3.providers.HttpProvider('http://localhost:8645')); // Device A
web3.setProvider(new web3.providers.HttpProvider('http://localhost:8745')); // Device B
```

要支持这两种不同的应用程序，需要启动两个状态节点：

    statusd --datadir app1 --http --httpport 8645 --shh
    statusd --datadir app2 --http --httpport 8745 --shh

# 工作流程：发现，订阅，聊天管理，通知触发

## 发现请求和通知服务器选择

一切都以`Device A`开头，愿意向某些Notification Service
Provider注册。为此，它将发现请求发送给某个已知群集（因为您需要指定wnode端使用的PubKey）：

``` go
var protocolKey = '0x040edb0d71a3dbe928e154fcb696ffbda359b153a90efc2b46f0043ce9f5dbe55b77b9328fd841a1db5273758624afadd5b39638d4c35b36b3a96e1a586c1b4c2a';
var discoverServerTopic = '0x268302f3'; // DISCOVER_NOTIFICATION_SERVER
var sendDiscoveryRequest = function (identity) {
    // notification server discovery request is a signed (sent from us),
    // encrypted with Notification Protocol Asymmetric (Public) Key
    var err = web3.shh.post({
        from: identity,
        to: protocolKey,
        topics: [discoverServerTopic],
        ttl: 20
    });
    if (err !== null) {
        console.log("message NOT sent")
    } else {
        console.log("message sent OK")
    }
};
var identity = web3.shh.newIdentity();

sendDiscoveryRequest(identity);
```

一旦这个请求被发送，所有有能力的wnode都会回应，所以我们需要注意他们的响应，并选择一个提供者（通常是FIFO）。观看：

``` go

var proposeServerTopic = '0x08e3d8c0'; // PROPOSE_NOTIFICATION_SERVER
var watchProposeServerResponses = function (identity) {
    // some notification servers will be able to serve, so they will send encrypted (to you)
    // message, with a PROPOSE_NOTIFICATION_SERVER topic (for which we wait)
    var filter = web3.shh.filter({
        to: identity, // wait for anon. messages to ourselves
        topics: [proposeServerTopic]
    });
    filter.watch(function (error, result) {
        if (!error) {
            console.log("Server proposal received: ", result);
            // response will be in JSON, e.g. {"server": "0x81f34abd0df038e01a8f9c04bee7ce92925b7240e334dc8f2400dea7a2a6d829678be8b40e1d9b9988e25960552eafe2df7f928188e4143ba657a699519c438d"}
            // which will give you serverID
            var payload = JSON.parse(web3.toAscii(result.payload));

            // no need to watch for the filter any more
            filter.stopWatching();

            // accept (in FIFO order) the server
            // we need to make sure that only a single server is selected,
            // as you will receive multiple proposals for different servers,
            // and may accept more that one of those proposals (which will
            // result in duplicate notifications)
            sendAcceptServerRequest(identity, payload.server);
        }
    });
};
watchProposeServerResponses(identity);
```

现在，只要我们收到服务器提议并愿意接受它，就需要广播`sendAcceptServerRequest（）`消息（以便服务器知道我们选择了它）：

``` go
var acceptServerTopic = '0x04f7dea6'; // ACCEPT_NOTIFICATION_SERVER
var sendAcceptServerRequest = function (identity, serverId) {
    // whenever we are ready to accept server, having a given serverId, we need
    // to notify it by sending signed (from us) and encrypted (using protocol key)
    // acceptance message.
    var err = web3.shh.post({
        from: identity, // it is absolutely important to identify the client, or your acceptance will be dropped
        to: protocolKey,
        topics: [acceptServerTopic],
        payload: '{"server": "' + serverId + '"}',
        ttl: 20
    });
    if (err !== null) {
        console.log("message NOT sent")
    } else {
        console.log("message sent OK")
    }
};
```

要完成发现协议，我们需要从我们选择的Notification
Server接收ACK消息。服务器的ACK消息还将包含SymKey，我们将使用SymKey与服务器下线进行安全通信：

``` go
var ackClientSubscriptionTopic = '0x93dafe28'; // ACK_NOTIFICATION_SERVER_SUBSCRIPTION
var watchServerAckResponses = function (identity) {
    // if server we accepted is ok, it will send encrypted (to you)
    // message, with a ACK_NOTIFICATION_SERVER_SUBSCRIPTION topic (for which we wait)
    // This message completes the subscription process. At this point you should
    // have topic and symkey necessary to manage your subscription.
    var filter = web3.shh.filter({
        to: identity, // wait for anon. messages to ourselves
        topics: [ackClientSubscriptionTopic]
    });
    filter.watch(function (error, result) {
        if (!error) {
            console.log("Server ACK received: ", result);
            // response will be in JSON, e.g. {"server": "0xdeadbeef", "key": "0xdeadbeef"}
            // which will give you serverID
            var payload = JSON.parse(web3.toAscii(result.payload));
            console.log(payload);

            // no need to watch for the filter any more
            filter.stopWatching();

            // this concludes discovery, and we can use obtained key to invoke chat sessions
            createChatSession(payload.key)
        }
    });
};
```

## 创建聊天会话和注册设备

所以，在这一点上，我们有一个客户端会话密钥（`payload.key`在前面的代码示例中传递给`createChatSession（）`），我们准备使用它来创建聊天会话。

现在为什麽我们有不同的客户端和聊天键？原因很简单：客户端会话密钥是客户端和通知服务之间的密码连接，仅在主机设备上才能知道。但是，为了能够相互触发通知，设备需要共享一些秘密。该秘密是聊天会话密钥。对于设备来说，知道聊天会话密钥就足够了：

  - 注册为通知接收者
  - 触发所有其他注册设备上的通知
  - 所有这些以安全/黑暗的方式

以下是创建聊天会话密钥的方式（请记住，我们仍在使用`web3`对象连接到`Device A`节点的）：

``` go
var newChatSessionTopic = '0x509579a2'; // NEW_CHAT_SESSION
    var ackNewChatSessionTopic = '0xd012aae8'; // ACK_NEW_CHAT_SESSION
    var createChatSession = function (subscriptionKey) {
        var chatId = '0xdeadbeef';

        // subscriptionKey is key shared by server that allows us to communicate with server privately
        var keyname = 'SUBSCRIPTION_KEY'; // you might want to be tad more creative
        web3.shh.deleteSymKey(keyname);
        web3.shh.addSymKey(keyname, subscriptionKey);

        console.log("subscription key: ", subscriptionKey);

        // before sending new chat request, let's start waiting for response
        var filter = web3.shh.filter({
            to: identity,
            topics: [ackNewChatSessionTopic]
        });
        filter.watch(function (error, result) {
            if (!error) {
                console.log("Chat Creation ACK received: ", result);
                // response will be in JSON, e.g. {"server": "0xdeadbeef", "key": "0xdeadbeef"}
                // which will give you serverID
                var payload = JSON.parse(web3.toAscii(result.payload));

                // no need to watch for the filter any more
                filter.stopWatching();

                // ok, at this point we have Chat Session SymKey, and we can:
                // 1. register our device with that chat
                // 2. share that key with others, so that they can register themselves
                // 3. use chat key to trigger notifications
                registerDevice(chatId, payload.key);
                shareChatKey(chatId, payload.key);
            }
        });

        var err = web3.shh.post({
            from: identity,
            topics: [newChatSessionTopic],
            payload: '{"chat": "' + chatId + '"}', // globally unique chat ID
            ttl: 20,
            keyname: keyname // we use subscription key, so connection is NOT public
        });
        if (err !== null) {
            console.log("message NOT sent")
        } else {
            console.log("message sent OK")
        }
    };
```

现在我们有聊天会话密钥，我们可以使用它来注册我们的设备ID（由FCM用作目标），并允许其他人注册自己。

我们来看看\`registerDevice（）\`看起来如何：

``` go
    var newDeviceRegistrationTopic = '0x14621a51'; // NEW_DEVICE_REGISTRATION
    var ackDeviceRegistrationTopic = '0x424358d6'; // ACK_DEVICE_REGISTRATION
    var registerDevice = function (chatId, chatKey) {
        console.log('chat session key: ', chatKey)
        // this obtained from https://status-sandbox-c1b34.firebaseapp.com/
        var deviceID = 'ca5pRJc6L8s:APA91bHpYFtpxvXx6uOayGmnNVnktA4PEEZdquCCt3fWR5ldLzSy1A37Tsbzk5Gavlmk1d_fvHRVnK7xPAhFFl-erF7O87DnIEstW6DEyhyiKZYA4dXFh6uy323f9A3uw5hEtT_kQVhT';

        // make sure that chat key is loaded
        var keyname = chatId + 'chatkey'; // there might be many chat keys
        web3.shh.deleteSymKey(keyname);
        web3.shh.addSymKey(keyname, chatKey);

        // before sending request, let's start waiting for response
        var filter = web3.shh.filter({
            to: identity,
            topics: [ackDeviceRegistrationTopic]
        });
        filter.watch(function (error, result) {
            if (!error) {
                console.log("Device Registration ACK received: ", result);
                // response will be in JSON, e.g. {"server": "0xdeadbeef"}
                var payload = JSON.parse(web3.toAscii(result.payload));

                // no need to watch for the filter any more
                filter.stopWatching();
            }
        });

        var err = web3.shh.post({
            from: identity,
            topics: [newDeviceRegistrationTopic],
            payload: '{"device": "' + deviceID + '"}',
            ttl: 20,
            keyname: keyname
        });
        if (err !== null) {
            console.log("message NOT sent")
        } else {
            console.log("message sent OK")
        }
    };
```

如果你已经更新deviceId（指向你的设备），一旦你运行测试，你应该这样做：

# 限制

  - 这个版本的主要限制是所有数据都在内存中，即没有持久性存储，并且一旦节点停止订阅，聊天，设备 -
    所有这些信息都消失了。这个问题将得到高度重视（实际上并不难）

[Category:Status-go](Category:Status-go "wikilink")