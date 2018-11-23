# 這份文件的目標

以清晰易懂的方式描述新的協議代碼（"鳥瞰圖"，每種不同的消息類型以及它們在解釋的操作方面代表什麼）
詳細描述每種消息類型，包括封裝在其中的所有信息（只要添加了需要協議版本inc的新功能即可更新）
描述新協議的安全機制，其中使用了sym/asym密鑰，它們如何更新/刷新，我們如何選擇耳語話題等。

# TODOS

  - 制定基準
  - 編寫測試
  - 實施acks, nacks
  - 討論傳輸層作為外部庫
  - 從status-react方刪除加密庫
  - 刪除nodeify黑客
  - 清理領域(掛起消息，無用的領域...)

# 情景示例

## 1對1聊天

A將B添加為聯繫人並發送消息。
B接受A作為聯繫人並將他的名字改為C

注意：目前，不考慮多設備支持。但是，A和B有一個單一主題的事實使A有可能在其他設備上接收他的消息，而舊協議中的消息以非對稱加密方式發送時不可能。

![Newstatuscommunicationprotocol_image1.png](Newstatuscommunicationprotocol_image1.png
"Newstatuscommunicationprotocol_image1.png")

## 公共聊天

除了用於發布消息和命令的常規消息之外，公共聊天不需要任何特定的消息類型。由於它們是公開的，我們無法從外部隱藏任何東西，但我們仍然使用對稱加密來限制竊聽者無需主動查找公共聊天內容的能力。公共聊天的對稱密鑰使用chat-id作為密碼，主題基於chat-id（使用web3
sha3方法並取前8個字節）。

要加入公開聊天，A只需要知道用作聊天ID的公開聊天的名稱，並使用以chat-id作為密碼生成的對稱密鑰來過濾衍生的主題。

## 群組聊天

群組聊天由管理員創建。他從他的聯繫人列表中選擇參與者。邀請使用與管理員聯繫時共享的相同主題和同一個密鑰分別發送給每個參與者。
邀請中包含GroupAdminUpdate消息，其中包含聊天名稱以及包含在NewGroupKey消息中的參與者身份列表，該消息提供將用於群聊的聊天ID和對稱密鑰。

A與他的聯繫人中的B和C創建群聊。他們都收到包裹在NewGroupKey消息中的GroupAdminUpdate。
A，B和C使用聊天主題和對稱密鑰將消息交換給群組聊天。 A從聊天中刪除B.
C會收到通知，B已從包含在NewGroupKey消息中的GroupAdminUpdate聊天中刪除，對稱密鑰已更改，因此B不能再跟隨該聊天。此消息以與A分享的主題進行1對1聊天。
A將D添加到聊天中。
C收到一個GroupAdminUpdate，再次包裝在一個NewGroupKey消息中，對稱密鑰被更改，以便D在加入之前無法看到聊天中說的內容。
A，C和D使用聊天主題和對稱密鑰將消息交換到群組聊天。 C離開聊天。 C在組主題中發送GroupLeave消息。
A發送GroupAdminUpdate包裹在一個NewGroupKey消息中，以更新對稱密鑰，以便C不能再跟隨該對話。

總而言之，消息以基於管理員創建組時生成的聊天ID的主題發送。每當參與者列表發生變化時，管理員就會通過直接向每個參與者發送新密鑰來更改對稱密鑰。

# 消息類型

有效載荷以傳輸格式編碼，這基本上是JSON。這使得Javascript和Go易於創建和接收來自應用程序的消息。它還節省了大量的空間，因為記錄被編碼為數組而不是消息之間具有冗餘密鑰的映射。
我們可能會考慮使用msg-pack來進一步減少傳輸的字節數。

Transit可以很容易地從js中使用:
<https://github.com/swannodette/transit-js-example/blob/master/shared/handlers.js>

消息有效內容是一個JSON向量，消息類型標記作為第一個元素，而向量的參數作為第二個元素。

## NewContactKey

發送給未來聯繫人的第一封郵件。此時，我們知道聯繫人正在過濾的唯一主題是使用其公鑰進行發現的主題，以便NewContactKey將被發送到哪個主題。它包含將用於未來通信的sym-key和主題以及我們想發送的實際消息。同義詞和主題是隨機生成的，因為我們不希望主題與其參與者之間存在任何關聯以避免元數據洩露。
當其中一個聯繫人恢復其賬戶時（聯繫人的賬戶恢復目前尚未實施並且是正在進行的群體），則還會發送NewContactKey消息以更改對稱密鑰和主題。

### 參數

sym-key: 對稱密鑰將用於進一步的通信。 Signal使用x3dh作為密鑰協議協議:
<https://signal.org/docs/specifications/x3dh/>
，不幸的是這是一個基於服務器的解決方案，需要公鑰基礎設施，但我們可能想在將來調查一個無服務器的替代方案。我們也可以允許用戶輸入一個可選的離線秘密。
topic: 將用於進一步通信的主題 message: 包裹在NewContactKey消息中的消息（現在只有ContactRequest）

### 編碼

注意:該示例中的消息字段是一個類型為〜＃c2的消息，它是一個ContactRequest

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

## 聯絡請求

發起聯繫請求時，包裝在NewContactKey消息中。

注意：在協議的第一個版本中，ContactRequest被另一個ContactRequest回复，使得它在雙方之間成為有狀態的交互，並且在處理ContactRequest時具有不必要的複雜性。此外，由於新協議在初始ContactRequest後使用了專用主題，因此最好為請求和響應提供兩種不同的消息類型。
先前已刪除的字段包括message-id，require-ack？，類型，聯繫人，密鑰對和內容。
現在基於序列化消息有效載荷的sha3來計算消息ID。需求 -
答案？是不需要通過電線發送的帶外信息。該類型已知，這是一條消息。聯繫人是包含發送請求的用戶的姓名和個人資料圖的地圖，這些用戶現在位於記錄的頂層。密鑰對是clojure側使用的密碼術的一部分，在新協議中完全刪除。內容總是零。

## 參數

\= name: 聯繫人的姓名 profile-image: 聯繫人的個人資料圖片 address: 交易聯繫人的以太坊地址。
注意：地址來源於擴展密鑰的公鑰，這是竊竊私語的身份，所以我們有簽名時的地址。因此這個領域可以被忽略 .
fcm-token: 通知聯繫人的fcm-token 注意：一旦實施，推送通知V2將需要更新：
聯繫人A將廣播具有某個知名主題的消息，通知提供商將知道如何解密。此處的目標是檢索一個特殊主題，以便我們與提供聯繫人B的特定通知提供商進行通信。
當聯繫人A想要將消息發送到聯繫人B時，它會像往常一樣發送消息，然後使用收到的主題和聯繫人B的發布鍵向聯繫人B的通知提供者發送單獨的消息。通知提供商然後將負責使用FCM或其他提供商發送通知。

### 編碼

由於我們需要發送對稱密鑰和主題以供將來進行通信，因此此消息始終嵌入在 [NewContactKey
message中](NewContactKey_message "wikilink")

## ContactRequestConfirmed

這是聯繫人接受聯繫請求時將發送的消息。它將根據NewContactKey消息中提供的主題發送並使用sym鍵。這兩個用戶將因此具有相同的過濾器。

### 參數

參數與ContactRequest相同

### 編碼

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

當用戶更改他的名字或個人資料圖片時發送。

### 參數

name: 聯繫人的姓名 profile-image: 聯繫人的個人資料圖片

### 編碼

    [
      "~#c6",
      [
        "eric sim",
        "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACgAAAAoCAMAAAC7IEhfAAA..."
      ]
    ]

## 信息

兩個用戶之間交換的典型消息。

注意：在協議的第一個版本中，消息本身就是一張地圖，這些字段保留在不必要的地方，例如發送者已經包含在更廣泛的私語消息中，並且更可靠，因為這個可以是偽造不像耳語消息的簽名。
先前已刪除的字段是message-id，require-ack？，type和show？ 現在基於序列化消息有效載荷的sha3來計算消息ID。需求
- 答案？是不需要通過電線發送的帶外信息。該類型已知，這是一條消息。顯示？被刪除，因為它沒用。

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

當用戶看到一條消息（打開聊天並加載消息）時發送。可以同時確認多條消息

注意：在協議的前一版本中，發送消息的方式非常相似，只是當聊天被打開時，幾乎同時發送了N條消息，每條消息的發送數量為1，這在以前的聊天中沒有看到。不同的是，現在我們只發送1條包含這些消息ID的N消息消息。

### 參數

message-ids: 用戶已經看到的消息的id的向量

### 編碼

    [
      "~#c5",
      [
        "0xfebd2cb7aa6d77df5468381d014c9d1663549e7f43e9aabfefe45e338caf76c5",
        "0xf0f3871715c97dd964ef079e1b1eefa5df6c3ebb13d03bb3835a0e74df97bc74"
      ]
    ]

## NewGroupKey

該消息被單獨發送給每個參與者。由於組的創建者/管理員只能將他的聯繫人添加到組中，這意味著他已與每個組成員進行1對1的聊天，並且只有對稱密鑰和隨機主題。
該組將使用的主題由chat-id（使用web3 sha3方法並取前8個字節）確定，該組由管理員隨機選擇。
由於消息包含chat-id和sym-key參與者有足夠的信息來跟踪組中未來的消息。
NewGroupKey消息還包含具有聊天名稱和參與者列表的GroupAdminUpdate消息。

### 參數

  - chat-id: 該聊天的ID，由該群組的管理員隨機生成
  - sym-key: 該對稱密鑰將用於基於chat-ID來加密群組特定主題中的進一步通信
  - message:
擔保新對稱密鑰的GroupAdminUpdate消息或通知參與者創建組的初始GroupAdminUpdate消息。

### 編碼

## GroupAdminUpdate

此消息由組的管理員專門發送，因此應忽略其他參與者發送的所有GroupAdminUpdate。它包含聊天名稱和參與者列表，並在其中一個信息發生更改時發送，因為該組已被重命名或有人離開/加入/被刪除。
當從管理員接收到該消息時，參與者將其與他們當前的參與者列表進行比較，以知道誰離開/加入/從組中移出，這可能是他們自己的。

### 參數

chat-name: 顯示給每個參與者的聊天名稱 participants:
參與者的whisper身份。

### 編碼

## GroupLeave

當用戶離開群組聊天時發送。當發生這種情況時，組的管理員將更改對稱密鑰和主題，以便離開的用戶不能再跟隨對話。這將通過NewGroupKey消息來完成，該消息將包含包含參與者更新列表的GroupAdminUpdate消息。

### 參數

該消息沒有參數

### 編碼

# Message ids

討論消息

  - 可以很容易地為第三方計算這些消息

我們可以使用耳語消息標識，這有什麼問題嗎？

它是否可以從狀態反應中獲取？

我們需要一個特殊的消息類型來包裝遺漏的消息，以便在nack的情況下我們仍然可以擁有原始的消息ID

# 在Status實施情況

傳輸器名稱空間和常規應用程序名稱空間之間的清晰分離使我們能夠大大簡化應用程序代碼。因此，在中期內，眾所周知的複雜聊天命名空間更易於理解和進一步改進。我們的想法是，如果我們切換協議並使用PSS，只需要更改傳輸名稱空間中的代碼，並且其餘代碼可以保持不變。

耳語消息的有效載荷是實現StatusMessage協議的記錄。 StatusMessage協議有兩種方法，發送和接收。
在新創建的記錄上調用發送，該事件需要發送消息並返回效果圖。
在接收到：receive-whisper-message事件中的消息時，接收將在記錄上被調用，並返回效果圖。

傳輸器命名空間的組織如下：

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

以下是與app-db中的傳輸層相關的密鑰，您通常會在聊天列表中使用1對1聊天工具看到這些密鑰：

    :transport/chats {"0x043799de5117592183cf83f677bf354fe5eb9467ed948fe65d7bf9cddf298c09a8bac5f803d41cd1afece8708c3d3836ec61791772b469ead60c41c292b6dcc683"
    {:topic"0x31303369"
    :sym-key-id"d069f3bfe6aaafe463f9f4a47e91d6575db16e3d4e68e4db96c18b17d45532c9"
    :sym-key"0xe63848048f405a97027999c269d862b80ca93eb25097bffc791f35e36485af5e"
    :filter[TaggedValue: ForeignType, ]}
    :transport/discovery-filter [TaggedValue: ForeignType, ]

  -
    transport/chat
    是以chat-id作為關鍵字的地圖。主題和sym-key-id是傳輸層用於基於chat-id發送消息的內容。
    sym-key本身就是您在NewContactKey中收到的或添加聯繫人時創建的。它現在堅持到領域，直到狀態去堅持它。該過濾器在登錄後添加並在註銷後停止，對於不屬於任何特定聊天且始終存在的發現過濾器也是如此。

[Category:Developer
Documentation](Category:Developer_Documentation "wikilink")
[Category:Status Communication
Protocol](Category:Status_Communication_Protocol "wikilink")