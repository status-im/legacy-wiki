## Help Improve Status

Status is a community driven project built together with its users.
We’re completely open source and anyone is free to contribute. In this
spirit, Status optionally asks its users to contribute anonymous usage
data so that we can understand how the app is used in order to improve
it. This is strictly opt-in, and can be changed at any time.

### Data we collect

We collect anonymous usage data about when and how the app is being
used, this includes:

  - When a transaction is sent
  - When a DApp is opened
  - When a message is sent

A full list of the anonymous data we collect is available on
[Github](https://github.com/status-im/status-react/blob/develop/src/status_im/utils/mixpanel_events.edn).

Along with these events we also send the version of the Status app, the
device operating system and version.

### How it works

  - We use the services of [Mixpanel](https://mixpanel.com) to collect
    anonymous usage data.
  - We create an anonymous account id that is a hash of the wallet key.
    This way your wallet key never leaves your phone.
  - We only collect anonymous usage information if you explicitly gave
    us permission to do so. If you want to withdraw this permission, do
    the following:
  - \* Select Opt-out in your profile in the Status app
  - Your choice to (not) share anonymous usage data will not impact the
    performance of the app.

### Data we DON’T collect

  - Your IP address
  - The amount, currency or other details of your transactions.
  - The content of your messages
  - Your wallet key
  - Your name, or other identifiable information
  - Any other sensitive information, such as your password, recovery key
    or signing phrase

### We promise

  - We do not and will not share the data we collect with anyone.
  - We only collect data that will help us improve your experience of
    the app.