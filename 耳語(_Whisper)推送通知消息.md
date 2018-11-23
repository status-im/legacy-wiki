注意：歷史文檔。與截至2017年10月26日的推送通知的實際執行不符。最初由Victor Spring 2017（ish）提供。 -
oskarth

# 概觀

這是非常重要了解 **耳語( Whisper) 通知服務** 設計是為了:

  - 通過允許客戶訂閱啟用了服務的任意節點來增強Whisper / 5功能
  - 一旦訂閱，客戶端將接收隨機生成的用於控制訂閱（比如查詢服務器ID或生成聊天密鑰）的對稱密鑰（`SubscriptionKey`）
  - 使用`SubscriptionKey`，可以啟動新的聊天會話（產生另一個SymKey -
    `ChatKey`）。通過與他人共享該密鑰，您將擁有可用於觸發通知的公共秘密。
  - 盡可能使用Whisper協議本身。例如，發現機制不過是各方之間的Whisper消息交換。通知發送也是如此。目的是盡可能保持服務盡可能輕巧。

## 俯瞰的程序概觀

![b3cf02aa-20f8-11e7-882b-1a4958bcda13.png](b3cf02aa-20f8-11e7-882b-1a4958bcda13.png
"b3cf02aa-20f8-11e7-882b-1a4958bcda13.png")

這裡的關鍵是如何讓`DeviceB`在<code> DeviceA \</
code\>上觸發通知，所有這些都不知道使用哪個wnode，或者`DeviceA`。而且`Chat
SymKey`可以很好地解決這個問題（漂亮點是在於它可以用於一對一和群聊，所有您需要確保共享一個秘密，即`Chat
SymKey`，以便新手可以將他們的設備添加到訂戶列表中）。

**重點:**

  - `Device A`
    通過使用發現協議PubKey發送加密消息來啟動協議（狀態測試群集將在所有節點上安裝一個PubKey，允許其中任何一個對發現請求做出響應）。
  - 雖然使用非對稱加密技術進行發現，但一旦獲得`Subscription SymKey`和`Chat
    SymKey`，您還是要依賴於對稱加密。
  - 要了解通知請求消息與正常通信“一起”是很重要的。
      - 在<code> DeviceA \</
        code\>上發送加密信息到`DeviceB`（因此，只有`DeviceB`可以打開信封）
      - 一旦完成，您需要使用`Chat SymKey`發送加密廣播消息
      - 那邊的一些節點將能夠解密你的廣播（唯一的節點有`Chat
        SymKey`)
      - 一旦解密，wnode將覆蓋先前為此聊天記錄的設備ID列表，並使用這些ID向FCM發送通知請求（僅針對一對一聊天的1個請求，`n`聊天組的有`n-1`請求
        - 這是我們不通知自己）
  - 所以消息從不暴露，並且它們並行路由。通知也由Chat
    SymKey加密，因此不會被竊聽者知道。這意味著我們不會在偷偷中妥協。以下信息會暴露：
      - 通過註冊設備ID與給定的聊天會話，我們將其暴露給 `wnode`
  - 我們公開PubKeys（需要簽名的wnodes消息，以便您可以過濾出PubKey，並避免通過自己向自己發送通知）
  - 我們可能會在通知消息的主體中暴露一些關鍵信息（極不建議！）。

# 通信協議

下面的代碼是**過時的**（它是為Geth 1.5.9編寫的，並且Geth 1.6.0 Whisper
API劇烈變化）。我們仍建議瀏覽代碼和評論，以更好地理解這個想法。在本文末尾，我們將包含完整的測試套件（將與\[[https://mochajs.org/Mocha\]一起運行](https://mochajs.org/Mocha%5D一起運行)）。

假設您使用`statusd --datadir wnode1 wnode --notify --identity ./wnodekey
--firebaseauth
./firebaseuathkey`啟動了服務節點，其中`wnodekey`和`firebaseauthkey`文件分別包含節點的私鑰和FCM授權密鑰。現在，您的服務器已準備好接收使用PubKey加密的請求，這是我們的群集所使用的`0x040edb0d71a3dbe928e154fcb696ffbda359b153a90efc2b46f0043ce9f5dbe55b77b9328fd841a1db5273758624afadd5b39638d4c35b36b3a96e1a586c1b4c2a`.

在客戶端，我們需要使用協議PubKey和預先定義的主題：

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

以下是主題列表（請參閱完整示例文件，它們全部用於何處）：

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

最後，為了測試一切，你最好使用兩個不同的客戶端，而不是一個。因此，假設我們有兩個節點（`Device A`和`Device
B`），它們具有不同的底層Status守護進程：

``` go
var web3 = new Web3();
web3.setProvider(new web3.providers.HttpProvider('http://localhost:8645')); // Device A
web3.setProvider(new web3.providers.HttpProvider('http://localhost:8745')); // Device B
```

要支持這兩種不同的應用程序，需要啟動兩個狀態節點：

    statusd --datadir app1 --http --httpport 8645 --shh
    statusd --datadir app2 --http --httpport 8745 --shh

# 工作流程：發現，訂閱，聊天管理，通知觸發

## 發現請求和通知服務器選擇

一切都以`Device A`開頭，願意向某些Notification Service
Provider註冊。為此，它將發現請求發送給某個已知群集（因為您需要指定wnode端使用的PubKey）：

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

一旦這個請求被發送，所有有能力的wnode都會回應，所以我們需要注意他們的響應，並選擇一個提供者（通常是FIFO）。觀看：

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

現在，只要我們收到服務器提議並願意接受它，就需要廣播`sendAcceptServerRequest（）`消息（以便服務器知道我們選擇了它）：

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

要完成發現協議，我們需要從我們選擇的Notification
Server接收ACK消息。服務器的ACK消息還將包含SymKey，我們將使用SymKey與服務器下線進行安全通信：

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

## 創建聊天會話和註冊設備

所以，在這一點上，我們有一個客戶端會話密鑰（`payload.key`在前面的代碼示例中傳遞給`createChatSession（）`），我們準備使用它來創建聊天會話。

現在為什麼我們有不同的客戶端和聊天鍵？原因很簡單：客戶端會話密鑰是客戶端和通知服務之間的密碼連接，僅在主機設備上才能知道。但是，為了能夠相互觸發通知，設備需要共享一些秘密。該秘密是聊天會話密鑰。對於設備來說，知道聊天會話密鑰就足夠了：

  - 註冊為通知接收者
  - 觸發所有其他註冊設備上的通知
  - 所有這些以安全/黑暗的方式

以下是創建聊天會話密鑰的方式（請記住，我們仍在使用`web3`對象連接到`Device A`節點的）：

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

現在我們有聊天會話密鑰，我們可以使用它來註冊我們的設備ID（由FCM用作目標），並允許其他人註冊自己。

我們來看看\`registerDevice（）\`看起來如何：

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

如果你已經更新deviceId（指向你的設備），一旦你運行測試，你應該這樣做：

# 限制

  - 這個版本的主要限制是所有數據都在內存中，即沒有持久性存儲，並且一旦節點停止訂閱，聊天，設備 -
    所有這些信息都消失了。這個問題將得到高度重視（實際上並不難）

[Category:Status-go](Category:Status-go "wikilink")