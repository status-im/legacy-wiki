# Frequently Asked Questions

## Why a Messenger?

When Carl & Jarrad looked at how to achieve mass adoption for a client.
We looked at things such as where the people are, how they behave on
their devices, time spent on pc vs mobile - as of 2014 more time is
spent on mobile than desktop, and of that time - a THIRD of it is spent
within Instant Messengers.

Instant Messengers have the highest retention rates, that is when you
purchase an install, they become sticky and typically users won't
immediately uninstall.

The average user lifetime sky rockets once their friends are involved.
When you start thinking about this as a user base and how to connect
that with Ethereum, things like payments and applications built ontop of
Ethereum integrated into a converstional interface starts becoming like
a clear path forward.

Ultimately we're building a hybrid browser and messenger for us to have
the best chance to cast the widest net and focus on user acquisition,
staying agnostic and as close as possible to the principles Ethereum
embodies.

## Does Status support DApps that have their own web page ?

Yes, we absolutely support DApps on their own webpage and are striving
for SWARM support also. Chances are, if a DApp works in Mist or MetaMask
it is likely to work in Status\!

Our goal is to be a DApp browser for Android & iOS, in addition to this
we'll support a Chat API if you wish to integrate within a chat context.

If your web-based DApp renders on mobile, trying using the `@browse`
command inside a chat and navigating to your DApp URL\! During our alpha
release, DApp developers can also add their DApps as default contacts by
following these instructions, but in later releases these will be
discoverable to uses through the Discover feature.

## Can your App be launched with a link?

Not at the moment, the standard we'll likely support is
[EIP67](https://github.com/ethereum/EIPs/issues/67)

## Do I have to run go-ethereum myself or on server?

Status includes `go-ethereum` and connects directly to the Ethereum
network. All you need to do is run the Status app\! This is possible to
do with the new Light Client Protocol we've implemented into our own
[Codebase](https://github.com/status-im/status-go)

## When did you start coding and how you are funded?

Status is largely self-funded. We've been working on it since prior
Devcon1, and we were awarded a Devgrant to port EthereumJ to Android
prior to that.

EthereumJ has different goals, its developers intended for server use
and had no interest in supporting the light client protocol, another
large factor is we wanted largely a single codebase for multiple
platforms, we had Java running on iOS and Android, we want unify the GUI
but the means to do that (JavaFX) is really not suited to mobile
devices.

Our current approach allows us to get bleeding edge tech and stability
of geth whilst maintaining a single
codebase.

## I am new to Ethereum & Blockchain. I would like to contribute to the project, where would you suggest I start?

To get up to speed with Ethereum, here are a few resources to get you
started: [EthDocs](http://www.ethdocs.org/en/latest/) ,
[Ethereum.org](https://www.ethereum.org) , [Ethereum Stack
Exchange](https://ethereum.stackexchange.com) , and [Ethereum on
YouTube](https://www.youtube.com/user/ethereumproject/playlists)

For contributing to Status directly you will want to check out our
[](Developer_Documentation "wikilink")

## I have a DApp that runs on Mist how can I test it in Status?

So each chat context has a command `@browse` - allowing users to access
a webview (you can imagine this as a bit like a browser tab), so much in
the same way as mist. In terms of integration/compatibility nothing is
required on your end to do that if it already works in Mist.

That said, looking beyond the alpha, we'll have developer tooling, and a
way for DApp developers to have profiles for theirs DApps (this then
makes them discoverable through the 'Discover' feature), along with the
Chat API for Developers who want to integrate through a conversational
UI rather than (or in addition to) webview.

## What is the "jail" in your status-react code?

When the Chat API is ready to use, your javascript code is executed in
[Otto VM](https://github.com/robertkrimen/otto) this is the same
javascript engine `go-ethereum` uses. That way your code is executed in
a "jail" and shouldn't interfere with the rest of Status. At least,
that's the theory, at the moment we have no implementation of the
halting problem, but we will.

For web DApps, we rely on the webview to correctly jail javascript.

## Is it going to be for Android only?

No, we support Android & iOS.

## Is it possible to install Status & how can I test?

Yes, we have a section on how to build it yourself ''' Needs link \< '''
. Alternatively we are working through getting more TestFlight invites
out for iOS so make sure to sign up on the homepage you can also see the
section on our nightly builds below for installing the APK yourself.

## What languages do you support?

We’re currently supporting 30 languages — this includes; English, 官話,
官话, 廣東話, 上海话, Nederlands, Français, Deutsch, हिन्दी, Magyar,
Italiano, 日本語, 한국어, Polski, Português brasileiro, Português europeu,
Română, Slovenski, Español, Español (Latin-America), Swahili, Svenska,
Suisse française, Schweizerdeutsch, Svizzera Italiana, ภาษาไทย, Türkçe,
русский, українська, اُردُو & Tiếng Việt\!

If you see a typo, mistranslation or something missing, don't hesitate
to let us know via the Status Slack, or fix it yourself using our
Translation Guide\! **\<Needs link**

## What about all these permissions in the app?

Permissions are always a hot topic for apps, we hate the way we’re doing
them too. The good news is it’s only like this for alpha and in
production we’ll make permission usage on-demand. At the moment there’s
many moving parts and a bunch of react-native dependencies (and
Instabug) which don’t have code to ask on-demand. Thanks for
understanding\!

  - Contacts, SMS, Telephone — these are for an optional step of phone
    contact synchronisation. We don’t use them unless you tap on the
    phone sync request message.

<!-- end list -->

  - Location — Currently used for our `/location` command. Our aim is to
    provide this to DApps For things like sharing location with friends,
    ordering food, or self-driving cars.

<!-- end list -->

  - Microphone — In this version, we don’t need it, it’s there because
    we intended to send audio messages over Whisper, in practice this
    was a bad idea. We still intend to offer Audio messages in Beta so
    it’s going to stay. It is also used in Instabug which we use to
    collect feedback.

<!-- end list -->

  - Storage — We need a place to put the blockchain data this will most
    likely be Swarm.

<!-- end list -->

  - Camera — We use this for setting your profile picture and for
    reading QR codes.

## How can I install a nightly for android?

Here you go\!
<http://artifacts.status.im:8081/artifactory/nightlies-local/>