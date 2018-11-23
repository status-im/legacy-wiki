## Universal Links

Note: Universal links are WIP and do not work in release builds.

#### What are Universal Links

With universal links, mobile users can tap a link to Status and get
seamlessly redirected to the Status app and deep linked to specified
screens without going through Safari or Chrome. If Status isn’t
installed, tapping a link to the Status website opens a Status website
with info on the link and instructions to download the mobile app.

#### What Links Are Supported

##### Public Chats

<https://get.status.im/chat/public/>**CHANNEL-NAME** eg
<https://get.status.im/chat/public/status>

##### DApp Browser

<https://get.status.im/browse/>**DAPP-ADDRESS** eg
<https://get.status.im/browse/www.cryptokitties.co>

##### DApp Browser

<https://get.status.im/user/>**CONTACT-ADDRESS** eg
<https://get.status.im/user/0x044166fa04e967e333211ae0cc1087c9c74exxx>

#### Custom URL Scheme

In addition to the deep links the above links can be called with the
status-im:// custom URL scheme, such as status-im://chat/public/status