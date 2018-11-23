NOTE: Historical document. Doesn’t correspond to actual implementation
of Push Notifications as of October 26, 2017. Originally wrritten by
Victor Spring 2017 (ish). — oskarth

# Overview

It is crucial to understand what is **Whisper Notification Service** is
designed for:

  - to augment Whisper/5 functionality, by allowing clients to subscribe
    to arbitrary node that has service enabled
  - once subscribed, client receives randomly generated symmetric key
    (`SubscriptionKey`), which is used to control subscription (say,
    querying for server id or generating chat keys).
  - using `SubscriptionKey`, new chat sessions can be initiated
    (yielding yet another SymKey - `ChatKey`). By sharing that key with
    others, you will have a common secret that can be used to trigger
    notifications.
  - everywhere possible, Whisper protocol itself was used. For example,
    discovery mechanism is nothing more but Whisper message exchanges
    between parties. The same goes for notification sending. The aim was
    to keep the Service as slim as
possible.

## High-Level Overview of the process

![<File:https://cloud.githubusercontent.com/assets/188194/25035829/b3cf02aa-20f8-11e7-882b-1a4958bcda13.png>](https:/cloud.githubusercontent.com/assets/188194/25035829/b3cf02aa-20f8-11e7-882b-1a4958bcda13.png
"File:https://cloud.githubusercontent.com/assets/188194/25035829/b3cf02aa-20f8-11e7-882b-1a4958bcda13.png")

The crux here is how to allow `DeviceB` to trigger notifications on
`DeviceA`, all this w/o knowing much of which wnode is used, or details
of `DeviceA`. And `Chat SymKey` solves this issue elegantly (the beauty
is that it works for both one-on-one and group chats, all you need to
ensure is share a secret, namely `Chat SymKey`, so that newcomers can
add their devices to subscribers list).

**Important Notes:**

  - `Device A` starts the protocol by sending encrypted message using
    Discovery Protocol PubKey (Status test cluster will have a single
    PubKey installed on all the nodes, allowing any of them to respond
    on discovery requests).
  - While discovery happens using asymmetric cryptography, once you
    obtain `Subscription SymKey` and `Chat SymKey` you rely on symmetric
    encryption.
  - It is important to understand that notification request messages go
    *along* the normal communication i.e.
      - on `DeviceA` you send encrypted message to `DeviceB` (so, only
        `DeviceB` can open envelope)
      - once done, you need to send a broadcast message encrypting it
        with `Chat SymKey`
      - some node over there, will be able to decrypt your broadcast
        (the only one that has `Chat SymKey`)
      - once decrypted, wnode will go over list of device IDs previously
        registered for this Chat, and send notifications requests to FCM
        using those IDs (only 1 request for one-on-one chats, `n-1`
        requests for group of `n` chats - that’s we do not notify
        ourselves)
  - So, messages are never exposed, and they are routed in parallel.
    Notifications are also encrypted (by Chat SymKey), so are not
    exposed to eavesdropper either. This means we are not compromising
    on darkness. The following info gets exposed though:
      - by registering device ID with a given chat session we expose it
        to `wnode`
      - we expose our PubKeys