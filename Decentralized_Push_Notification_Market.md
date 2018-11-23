Taking a hard stance on decentralization in the client isn’t without its
challenges. Due to the peer-to-peer nature of the Ethereum communication
subprotocol, Whisper (SHH), simple expected user experiences, such as
being notified when a friend has responded to your messages, have to be
themselves redesigned in a decentralized context.

With the new Whisper V5 protocol, we can now delegate nodes to do o line
inboxing (storing messages while clients are o line) and we will extend
this ability to support push notifications.

This allows us to establish a market for push notification providers.
For service, stakeholders will deposit SNT with a provider, who may
charge microtransactions for notification and storage.

This a ords stakeholders the right to choose which nodes in the network
will provide the service for them, and what kind of push notification
providers they would like to use, perhaps choosing Deepstream over
Google or opting completely out of the service to maximize their
privacy.

We recognize that in a model where the User is no longer the Product,
paying for push notifications may initially seem like a hurdle, as Users
in existing platforms currently get this ‘for free’. Instead we make the
costs explicit to the User and don’t exclude possibilities where a push
notification node could build a business on top of the infrastructure,
ie. o er the service ‘for free’ by the User viewing ads to pay for push
notifications. The end result is that we give the User a choice.

Status implements an application protocol for message re-delivery, so as
soon as both parties are online in the network, messages are exchanged
and history is updated. Technical details of the implementation can be
found
[here](https://github.com/status-im/status-go/wiki/Whisper-Push-Notifications).

**SNT Utility**

  - SNT is required by Stakeholders to select and receive push
    notifications.

**Example use-cases**

  - Stakeholder A, a casual user, wants to receive Push Notifications
    and pays a microtransaction in SNT to do so.
  - Stakeholder B, an early adopter in Cuba, has been paying for Push
    Notifications, but Google’s Firebase has just been blocked in his
    country. He seamlessly changes to a different provider from with
    within Status.
  - Stakeholder C, a Push Notification provider, wants to generate SNT,
    so they participate in the market to o er their services.