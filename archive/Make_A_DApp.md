##  Introduction

Status allows users to interact with a wide array of Decentralized
Applications (DApps) using the same intuitive chat interface (it also
does a bunch of other things, but we'll focus on this aspect for now).
In the near future, Status users will be able to have group chats where
most of the participants are DApp chatbots. All DApp developers will
benefit from this synergy, because a common chat interface for multiple
DApps makes using your specific DApp more convenient, and effectively
makes your DApp more powerful by giving it access to potentially far
wider and more powerful network effects.

In this guide, we’ll explore how you can use the Status API to develop
your DApp and create custom commands for your users that will work in a
beautifully-intuitive, mobile context. As a result of developing on
Status, you’ll have a DApp that your users can access on MetaMask, Mist,
and Status. It's really worth emphasising that using Status brings with
it access to mobile users with native mobile commands. With little extra
developer time invested, you’ll gain a mobile DApp.

## Quickstart

If you already have a DApp with a web interface, then this will be the
quickest Quickstart ever (trademark pending). Simply open Status,
navigate to Console, hit the `@browse` command and type in the url where
your DApp is running.

Voila\! Users in Status can already see your DApp and interact with it
(on the Ropsten or Rinkeby Test Networks). Make some mobile
optimizations of your own and you're away. That is the power of
decentralized, web3 technologies. Awesome, right?

OK, but what if `(a)` I don't have a DApp but want to learn how to build
one on Status, or `(b)` I want to use this awesome API to make the most
of an awesome, native mobile UX, or `(c)` I know what I'm doing and want
to create fully decentralized, chatbot functionality for my DApp?

## Tutorials

### Overview

Just before we get started, it's well worth acquainting yourself with
some of our terminology so you'll be able to make sense of it all. This
anatomy establishes the different sections of the chat interface and
establishes a common verbiage. The main components
are:

  - Message
  - Input
  - Keyboard
  - Suggestions

<https://raw.githubusercontent.com/status-im/wiki.status.im/master/docs/proposals/img/chat-anatomy.png>

Please take some time to familiarize yourself with all the areas and the
different configurations possible depending on what you want to do.
Missing from the above is what we refer to throughout this documentation
as the 'markup', by which we mean the mobile equivalent of the 'view',
i.e. where the messages appear.

Creating all of these native setups is totally possible through the API
- just read on.

### Installing Status

OK, let's learn how to build our first DApp on Status (mobile-first
ftw\!). To progress further, you need to have Status running either:

  - on a real phone,

<!-- end list -->

  - in an Android simulator, or

<!-- end list -->

  - in an iOS simulator.

You can go to [<https://test.status.im>](https://test.status.im/) to
download for Android. Or you can DM @jarradhope with your email in our
[Slack](https://status-im.slack.com/) for iOS Testflight.

**Please note:** *These documents are intended for the latest version of
"status-dev-cli" (^3.2.8) and the latest release of Status app (^0.9.8).
To update `status-dev-cli` please run `npm uninstall -g status-dev-cli`
and then `npm i -g status-dev-cli`.*

You can build Status yourself for either Android or iOS by following
[these
guidelines](https://wiki.status.im/contributing/development/building-status/).
There, you will find instructions for installing an Android simulator,
or starting up Status in the Xcode simulator.

Then, connect your phone to your Wi-Fi network (do not use USB).
Development machine and phone should be in the same network. Navigate to
the Console, select the `/debug` command and choose the `On` suggestion.
You’ll get back a message telling you that debugging is on, and that you
can use the
[status-dev-cli](https://github.com/status-im/status-dev-cli) tools.

You also need to install `status-dev-cli` to make talking between Status
and your machine a
breeze.

    npm i -g status-dev-cli

<https://raw.githubusercontent.com/status-im/docs.status.im/develop/static/images/starting.png>
*With your phone connected, /debug "On"*

### Debugging

OK, so Status is installed, but how do we interact with it?

You can check <DEVICE-IP> by running the `scan` command in the terminal.
Another useful command we will be using a lot later on is `list` which
will return a list of all DApps you have added to Status.

    status-dev-cli scan
    status-dev-cli list --ip <DEVICE-IP>  #be sure to use the IPv4 address

You will see that these tutorials also use <MACHINE-IP> throughout. This
refers to the INTERNAL IPv4 address of your development machine. You can
find that by running the command provided.

    ifconfig | grep inet

    # Look for this line - i.e. your IPv4 address - and use the equivalent of 192.168.0.103
    --> inet 192.168.0.103 netmask 0xffffff00 broadcast 192.168.0.255

Even more importantly though, how can we actually debug the code we
write? We know how important this is for good developers and we're still
working on it. Currently, the options are limited, but we ask you to
work with us here as it will improve rapidly over the next little while.

The first and the most powerful option is the development tools included
in your browser. Chrome supports remote debugging, and you can easily
connect to any web view on your mobile and debug it right from the
browser. The detailed instructions can be found
[here](https://developers.google.com/web/tools/chrome-devtools/remote-debugging/).

If you use iOS, you can use Safari on your Mac to debug any remote web
page.
[Here](http://developer.telerik.com/featured/a-concise-guide-to-remote-debugging-on-ios-android-and-windows-phone/)
is a nice guide that will explain everything to you.

Unfortunately, you cannot use your browser's development tools to debug
bots, as switching the node only works for DApps currently. So, the
option we provide for bots is the ability to extract logs.

    status-dev-cli log <DAPP-IDENTITY> --ip <DEVICE-IP>

### Networking

We give instructions here for Genymotion (a popular Android emulator).

1.  Install genymotion
2.  Create a genymotion virtual device
3.  Switch to network bridge mode (in the settings of your virtual
    device)
      -
        <https://raw.githubusercontent.com/status-im/docs.status.im/develop/static/images/networking.png>*Switch
        to "Bridge"*
4.  Start virtual device
5.  Install status.im apk from nightly builds by dragging onto emulator
    window
6.  Start status.im app on virtual device
7.  Turn on debugging in console (`/debug On`).
8.  Open terminal and run `status-dev-cli scan` it returns two
    <DEVICE-IP> addresses, use `192.168.1.*`, and ignore `192.168.56.*`.
    Alternately retrieve the device ip from the emulator window
    `Settings > About phone > Status > IP address`.
9.  Run `status-dev-cli add [dapp] --ip `<DEVICE-IP>. **Replace**
    `[dapp]` with your dapp details (`whisper-identity`, etc) as
    stringified json or **remove** `[dapp]` if those details are
    specified in a `package.json` in the current directory.

### My First DApp

#### Do It Yourself with \`status-dev-cli\`

First, install the `status-dev-cli` tools globally using NPM. Then,
create a directory for your app to live in, switch into it, and create
an `index.html` file and an `app.js` file.

    npm install -g status-dev-cli
    mkdir ~/my-dapp && cd my-dapp
    touch index.html app.js

The index.html will be really simple. We are going to add several meta
tags to make our DApp look good on the small screens of mobile phones,
and add a span that will be used by our JavaScript.

Our `app.js` file will also be simple. We are going to display the
information about your account inside a span with `account id`. Status
injects `web3.js` automatically, so you have an access to web3 variable
from everywhere and you don’t need to care about including `web3.js`
manually. However, you can do this and most probably you want to do this
if you want to make your DApps work on other platforms.

In index.html, add:

    <html>
      <head>
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
        <meta name="HandheldFriendly" content="True">
        <meta name="MobileOptimized" content="320">
        <meta name="viewport" content="width=device-width,minimum-scale=1,maximum-scale=1">
        <title>My DApp</title>
        <script src="app.js"></script>
      </head>
      <body>
        <h1>Hello world!</h1>
        Web3 account: <span id="account"></span>
      </body>
    </html>

In app.js, add:

    function onContentLoaded() {
      var accountSpan = document.getElementById("account");
      accountSpan.innerHTML =
        (typeof web3 === "undefined" ? "undefined" : web3.eth.accounts);
    }
    document.addEventListener("DOMContentLoaded", onContentLoaded);

You then need to install a really simple `http-server` from NPM (we
recommend it for ease-of-use), and start it in the `my-dapp` directory
we just created.

Open a new terminal session, navigate back to your `my-dapp` directory,
and go ahead and add your dapp to Status\! Make sure to pass in the
`--ip` flag using the address returned to you by Console, when you
\[enabled debugging\](\#enabling-debugging).

<MACHINE-IP> needs to be the internal IPv4 address returned when you run
`ifconfig | grep inet`.

    npm install http-server -g
    http-server

In a new terminal window run the port forwarding and DApp connection
stuff. It is important to pass in the `--ip` flag with the IP address
listed by Console once you have selected the `debug` option and turned
it
    on.

    status-dev-cli add '{"whisper-identity": "hello-bot", "dapp-url": "http://<MACHINE-IP>:8080/", "name": "Hello"}' --ip <DEVICE-IP>

That's it\! You should be able to see your DApp in the chats, and
opening it should browse automatically to a page that shows your account
information. You can also do live-reloading once you're happy with this
by running `status-dev-cli watch $PWD hello-bot --ip `<DEVICE-IP>

**Known Issues**

1.  You may need to escape the `json` information you are passing into
    `status-dev-cli` if you are on a windows machine, like
    so:

<!-- end list -->

    status-dev-cli add '{\"whisper-identity\": \"my-dapp5\", \"dapp-url\": \"http://<MACHINE-IP>:8080\",\"name\": \"My DApp\"}' --ip <DEVICE-IP>

1.  If using a real device, you also need to ensure that it is set to
    use MTP (media transfer Protocol) in the USB-debugging settings.
    Open the equivalent of your settings or develop options by pulling
    down the top menu and clicking in the debugging options.
2.  The new version of `status-dev-cli` does not require you to do any
    `adb` port forwarding any more and will, in fact, fail if you do.
    Just use the `scan` and `list` commands as and when you need them.

Happy travels\!

*You need not use NPM's http-server to serve content, there are
innumerable ways of doing this. Just pick whatever is best for your
environment.*

#### Truffle

OK, so we can write a little HTML5 DApp that displays information to
Status users through a simple webView component. However, we want to do
so much more\! We want to write smart contracts, deploy them, and
interact with them through a fully mobile user interface; and we want to
build decentralized chatbots that live within Status and make friends
with all the humans.

If you want to get straight to making chatbots, please go
\[\#my-first-1-1-chatbot here\].

Frameworks can lighten the load of developing considerably, and so we
include here some quick examples to get you up and running with the two
most popular Ethereum frameworks currently available - Truffle and
Embark.

Firstly, we'll need a network to develop against, so let's go get
`testrpc` - a neat little simulation of the rules and logic of the
Ethereum network that runs much faster than the live network (because
it's only simulating things) and is very useful for quick and dirty
iterations.

    npm install -g ethereumjs-testrpc
    testrpc -p 8546

That’s it\! It will show you a list of available accounts, private keys,
your HD wallet and mnemonic. Please note that we're running testrpc on
(non-default) RPC port 8546, just to avoid potential conflict with the
node running inside Status. If using android, you need to open the
connection from your PC to your phone on that port, so Status can listen
for changes there.

We also need to make sure that our Status client is listening to our new
`testrpc` network, rather than Ropsten, so that we can get the
information we need about our contracts etc. Luckily, this is as easy as
running:

    status-dev-cli switch-node "http://<MACHINE-IP>:8546" --ip <DEVICE-IP>

    # Of course, there are options. You can use go-ethereum instead of TestRPC, and instead of port forwarding you can switch to any other accessible node using its IP address.

*Please note that "switch-node" only works for DApps, and NOT for bots.
You javascript is still confined to the Otto VM jail for now. We are
working on a solution, but please use "dapp-url" if developing against a
local network for now.*

Open a new shell (i.e. a new Terminal window or tab) for the next part.
You’ll leave `testrpc` running in the first window, and use the second
window for the rest of the tutorial.

Now that you have `testrpc` is going, and a new shell is open, run:

    npm install -g truffle # Version 3.0.5+ required.

This installs the Truffle framework, and you can find its GitHub [page
here](https://github.com/trufflesuite/truffle).

With Truffle installed, we can grab the Status Truffle Box, and get a
basic DApp running. All the Truffle Boxes also include the app
frameworks React and Redux, which were designed by Facebook and are
widely used by app developers. You can find the other Truffle Boxes
[here](https://truffle-box.github.io/).

To install the Status Truffle box, all you have to do is run this
command in the same Terminal window:

    git clone https://github.com/status-im/truffle-box-status.git

    # Change into the truffle box directory and install the node dependencies
    cd truffle-box-status && npm install

    # Compile the contracts from Solidity (much like JavaScript) into runnable EVM code
    truffle compile

    # Publish the compiled contracts to your network. testrpc must already be running
    truffle migrate

<https://raw.githubusercontent.com/status-im/docs.status.im/develop/static/images/starting-a-dapp-on-status-with-frameworks_02.png>*Example
on OS X: testrpc running on the left, and installing Truffle on the
right*

As you run the `migrate` command - which is what deploys your contracts
to the network - you can look at the window with `testrpc` running, and
you’ll see your transactions being published. If you get `Error: Invalid
JSON RPC response`, you probably forgot to run `testrpc`.

Now we are ready to see our DApp running on Status. From within your
DApp directory, run:

    # Run your Javascript
    npm run start

    #(Remember to set ENV variable if working with real device)
    IP=<DEVICE-IP> npm run start

This should tell you that the the app is running, and that the DApp has
been added to the Status
Contacts.

<https://raw.githubusercontent.com/status-im/docs.status.im/develop/static/images/starting-a-dapp-on-status-with-frameworks_03.png>*The
DApp added to the default Contacts*

Again, the DApp should appear in your chats screen, and navigating to it
should automatically open a browser that reveals all the goodies hidden
in this truffle box. It should also display the stored value of `5` from
the SimpleStorage contract that we deployed to `testrpc` by running
`truffle migrate` above.

If you would like to change the name that appears for your bot when it
gets added to Status, simply edit the `package.json` and update the name
there.

Once everything is running, leave it as is and move on directly to the
next truffle tutorial to see the power of the `status-dev-cli watch`
command.

**Known problems and Notes:**

1.  If you are running on a real iOS device, you need to configure
    Truffle Box to use the network on your computer. In `truffle.js`,
    change `host` to the IP of your computer:

<!-- end list -->

    module.exports = {
      migrations_directory: "./migrations",
      networks: {
        development: {
          host: "<MACHINE-IP>",
          port: 8546,
          network_id: "*" // Match any network id
        }
      }
    };

1.  You can always use `localhost` instead of <MACHINE-IP>, but the
    application won't be accessible automatically, since it runs on port
    3000 and your device/emulator knows nothing about it. Execute the
    following to induce black magic and make the web application
    accessible:

<!-- end list -->

    adb reverse tcp:8546 tcp:8546
    adb reverse tcp:3000 tcp:3000

#### Embark

OK, so Truffle is not your favourite and you prefer using Embark. Fine
with us. As always, make sure you have installed the framework first.

    npm -g install embark

Embark makes it super easy to set up a demo and get up and running fast:

    embark demo
    cd embark_demo

Next we want to start the network. You can run a full Ethereum node with
`embark blockchain`, but for development you probably want to run
`testrpc`. We want it to run on port 8546 though, so it doesn't
interfere with Status, so we need to edit the `blockchain.js`:

    nano config/blockchain.json

    # Now edit the 'Development' Network and change only the rpcPort field to 8546:

    "development": {
        "enabled": true,
        "networkType": "custom",
        "genesisBlock": "config/development/genesis.json",
        "datadir": ".embark/development/datadir",
        "mineWhenNeeded": true,
        "nodiscover": true,
        "maxpeers": 0,
        "rpcHost": "<MACHINE-IP>",
        "rpcPort": 8546,
        "rpcCorsDomain": "http://<MACHINE-IP>:8000",
        "account": {
          "password": "config/development/password"
        }
      }

    # ctrl+O will write your changes to file, ctrl+X will exit the file.
    # Now, navigate back to the root of your project and start testrpc

    cd .. && embark simulator

Open a new shell tab and switch the RPC node. Once that is done, you
need to install the `embark-status`
    package.

    status-dev-cli switch-node "http://<MACHINE-IP>:8546" --ip <DEVICE-IP>
    npm install embark-status --save

When using a real device, you will now need to insert the relevant IP
into two config files.

Open the file `embark.json` and edit the `plugins` key to reflects your
DEVICE's IP address:

    "plugins": {
      "embark-status": {
        "deviceIp": "<DEVICE-IP>",
        "whisperIdentity": "dapp-embark-test",
        "name": "MyEmbarkDapp"
      }
    }

Navigate to and change the `config/webserver.js` file so that it
reflects your development machine's IP address:

    {
      "enabled": true,
      "host": "<MACHINE-IP>",
      "port": 8000
    }

Now we’re ready to run the DApp on Status. From within your DApp
directory, run:

    embark run

The Embark console will appear within your shell with useful information
about the SimpleStorage contract it has created, compiled, and deployed.
Now just add your DApp, and have some
    fun\!

    status-dev-cli add '{"whisper-identity": "embark", "dapp-url": "http://<MACHINE-IP>:8000/", "name": "Embark"}' --ip <DEVICE-IP>

<https://raw.githubusercontent.com/status-im/docs.status.im/develop/static/images/introduction-to-status-chat-api_04.png>

<https://raw.githubusercontent.com/status-im/docs.status.im/develop/static/images/introduction-to-status-chat-api_05.png>*The
Embark simulator runs in one Terminal window on the top, and the Embark
console on the bottom*

**Known Issues and Notes:**

1.  To deploy the DApp successfully on a device you may need to patch
    [this
    line](https://github.com/status-im/embark-status/blob/master/index.js#L13)
    in embark-status to include `+ " --dapp-port 5561"`.
2.  You can always use `localhost` instead of <MACHINE-IP>, but the
    application won't be accessible automatically, since it runs on port
    8000 and your device/emulator knows nothing about it. Execute the
    following to induce black magic and make the web application
    accessible:

<!-- end list -->

    adb reverse tcp:8546 tcp:8546
    adb reverse tcp:8000 tcp:8000

If you want to remove your DApp because you're unhappy with it for some
reason, just run `status-dev-cli list --ip `<DEVICE-IP> and then
`status-dev-cli remove --ip `<DEVICE-IP>` [my-dapp]` where \[my-dapp\]
is the `whisper-identity` returned by the `list` command.

Using Status, you can now develop mobile DApps as easily as developing
for MetaMask or Mist\! But Status offers extra goodies as well.

In particular, Status will help you allow your users to chat with your
DApp\! The chat interface will let your users easily and intuitively
accomplish tasks. In the future, your users will be able to hold group
conversations where all the other participants are DApps, which is kind
of amazing.

Later we’ll have an easy mechanism to make your DApp available for
others to use on Status, but for now please just submit a pull request
using our [guide on adding
DApps](http://wiki.status.im/contributing/development/adding-dapps/).