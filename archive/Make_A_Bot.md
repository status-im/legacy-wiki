###  My First 1-1 Chatbot

#### Do It Yourself

First, we're going to create a new `bots` directory and add file to keep
our javascript in.

    cd ~/my-dapp
    mkdir bots && cd bots && touch bot.js

Instead of adding a new DApp, we can now include a `bot-url` parameter
in our call to `status-dev-cli`. The chatbot url targets a `js` file,
and this file will be loaded in the Otto VM shown in the anatomy. Code
in this file сan interact with the input field, messages and react
however we program it to.

Add the following code snippet to the newly-created `bot.js` file.

    status.addListener("on-message-send", function (params, context) {
        var result = {
            err: null,
            data: null,
            messages: []
        };

        try {
            result["text-message"] = "You're amazing, master!";
        } catch (e) {
            result.err = e;
        }

        return result;
    });

Then, navigate back to the `bots` directory, do the necessary Android
steps if you are using that platform, and start the http server again.
Then, open a new shell window and add your new bot. It's important to
note a few things here. We are `add`ing a new new bot, rather than just
watching the DApp we built earlier for changes. Also, we are only
passing in a `bot-url`, rather than a `dapp-url` - this will ensure
that, when you open your new bot contact, it won't automatically launch
a browser a window.

    #make sure you're in my-dapp or the equivalent
    cd ~/my-dapp

    #start the server and then open a new shell window
    http-server

    status-dev-cli add '{"whisper-identity": "botler",  "name": "Botler" ,"bot-url": "http://<MACHINE-IP>:8080/bots/bot.js"}' --ip <DEVICE-IP>

This is pretty much the simplest responsive chatbot we can write, using
only the `addListener` function from the API. All it's listening for is
a message-send event, to which it will try to respond with the test
`You're amazing, master!`.

Obviously, there's much more we can do than simply listen for messages
and send flattering responses. All will be revealed in the next
tutorial. If you're feeling impatient, you can find a full Demo Bot
[here](https://github.com/status-im/status-react/tree/34b77022f7926dbabbb0e8c5e8471eaea5ed812f/bots/demo_bot).

**Known Issues and Notes:**

1.  You can always use `localhost` instead of <MACHINE-IP>, but the
    application won't be accessible automatically, since it runs on port
    8080 and your device/emulator knows nothing about it. Execute the
    following to induce black magic and make the web application
    accessible:

<!-- end list -->

    adb reverse tcp:8080 tcp:8080

#### Embark

OK, so even though `status-dev-cli` is lightweight and awesome, the
frameworks offer some really cool features and make development
significantly easier for many projects. Let's see what it's like to add
the same chatbot to Status using Embark.

First, go back to the `embark_demo/` directory we created
\[earlier\](\#my-first-dapp), where we set up the correct configuration
for Embark and Status. Essentially, you just want to make sure - as with
the Truffle Box example - that you put the javascript file in the right
place so that you can reference it correctly.

Instead of adding a new DApp, we can now include a `bot-url` parameter
in our call to `status-dev-cli`. The chatbot url targets a `js` file,
and this file will be loaded in the Otto VM shown in the anatomy. Code
in this file сan interact with the input field, messages and react
however we program it to.

Let's create a bot.js in the `/embark_demo/app/js/` directory and put
the short snippet of javascript we need into it.

    touch ~/embark_demo/app/js/bot.js
    nano ~/embark_demo/app/js/bot.js

    status.addListener("on-message-send", function (params, context) {
        var result = {
            err: null,
            data: null,
            messages: []
        };

        try {
            result["text-message"] = "You're amazing, master!";
        } catch (e) {
            result.err = e;
        }

        return result;
    });

This is pretty much the simplest responsive chatbot we can write, using
only the `addListener` function from the API. All it's listening for is
a message-send event, to which it will try to respond with the test
`You're amazing, master!`.

Obviously, there's much more we can do than simply listen for messages
and send flattering responses. All will be revealed in the next
tutorial. If you're feeling impatient, you can find a full Demo Bot
[here](https://github.com/status-im/status-react/tree/34b77022f7926dbabbb0e8c5e8471eaea5ed812f/bots/demo_bot).

We now need to update the `config/blockchain.json` so it copies over our
bot into a public directory (see the last line inside app object).

    "contracts": ["app/contracts/**"],
    "app": {
        "css/app.css": ["app/css/**"],
        "images/": ["app/images/**"],
        "js/app.js": ["embark.js", "app/js/_vendor/jquery.min.js", "app/js/_vendor/bootstrap.min.js", "app/js/**"],
        "index.html": "app/index.html",
        "js/bot.js": "app/js/bot.js"   # Add this line in #
    },
    "buildDir": "dist/",

That should serve the `js` files you need, so the last thing you'll need
to do is add this bot to Status as per
    usual.

    status-dev-cli add '{"whisper-identity":"embark-demo-bot", "name":"Embark-Demo-Bot", "bot-url":"http://<MACHINE-IP>:8000/js/bot.js"}' --ip <DEVICE-IP>

Again, you could always use `localhost` instead if you run into issues,
and just make sure to run `adb reverse tcp:8000 tcp:8000`.

#### Truffle

Let's see what it looks like to add the same, flattering little chatbot
to Status through the Truffle Box we set up earlier. We assume here that
you still have `testrpc` running from the previous tutorial and that you
are still running the `npm start` script. If not, navigate back to that
directory, ensure that `testrpc` is switched on and that you have opened
all the right ports if you're on Android.

Only if you have switched off everything from the previous tutorial.

    testrpc -p 8546

    status-dev-cli switch-node "http://<MACINE-IP>:8546" --ip <DEVICE-IP>

    cd ~/truffle-box-status

    # If you want to be extra sure your contracts are there, run:
    truffle migrate --reset

So, all we need to do here is add a `bot` directory in Truffle's
`public` directory (i.e. the one accessible from the web) and write some
simple javascript and - hey presto\! - we'll have a decentralized
chatbot. Copy the code provided into another `bot.js` file. The
`bot-url` targets a `js` file, and this file will be loaded in the Otto
VM shown in the anatomy. Code in this file сan interact with the input
field, messages and react however we program it to.

    cd ~/truffle-box-status/public/ && mkdir bot/
    touch bot/bot.js
    nano bot/bot.js

    status.addListener("on-message-send", function (params, context) {
        var result = {
            err: null,
            data: null,
            messages: []
        };

        try {
            result["text-message"] = "You're amazing, master!";
        } catch (e) {
            result.err = e;
        }

        return result;
    });

If you look on line 42-43 of the `npm start` script, you'll see that we
already passed in a `bot-url` when adding the Truffle Box in the
previous tutorial. So, all we need to do now is `status-dev-cli watch`
that same DApp to activate the live-reloading and start chatting to our
bot through the chat, or interacting with our contracts through the
webView. However, we need to know what to tell `status-dev-cli` to watch
for, so let's try `list` our DApps currently in Status.

    status-dev-cli list --ip <DEVICE-IP>
    # dapp-0x74727566666c652d626f782d737461747573 (Name: "truffle-box-status", DApp URL: "http://<MACINE-IP>:3000")

The first value returned there is the DApp's `whisper-identity`, which
is what we need to pass to the `watch` command to tell it what to look
for. Using it, we can now tell `status-dev-cli` to watch for our changes
and go find that brand new
    bot.

    # Make sure you're in the truflle-box-status/ directory as that is what $PWD refers to below
    cd ~/truffle-box-status

    # I would use dapp-0x74727566666c652d626f782d737461747573 for <whisper-identity> below, Substitute your own one in.
    status-dev-cli watch $PWD "<whisper-identity>" --ip <DEVICE-IP>

And you're away\! You should be able to see your DApp, browse to the
same site as before, and chat with the repetitively flattering greeter
bot.

Once you have worked through the first tutorials and understood the
basic steps to building a DApp and adding it into Status, it's time to
get our hands a little more dirty by actually writing a simple,
one-response chatbot that will begin to utilise the awesome power of the
Status API.

We kind of cheated a little in the previous tutorials. While it is
totally possible to have your html5 DApp work perfectly in Status via
`webView`, it's obviously not the most optimal way to do things. There
are essentailly two different ways for developers to interact with
Status, best illustrated by the Status Anatomy
below:

<https://raw.githubusercontent.com/status-im/docs.status.im/master/static/images/status-anatomy.png>

The main take away here is that the chat context itself is actually an
Otto VM jail that executes the javascript you write, and then integrates
that directly with Status. So, we can actually write bots that are
purely javascript-based. Please see
[here](https://github.com/status-im/status-react/tree/develop/bots) for
a full list of all our current bots and their source code.

### My First Status Command

#### Do It Yourself with \`status-dev-cli\`

So, we have set up Status in `debug` mode, added our DApp to it
(hopefully run the `status-dev-cli watch` command to get some
live-reloading going) and learned how to start a conversation with a
simple javascript chatbot. Now it's time to start using the API proper
and start using the provided commands to interact with our users and
help them out.

Navigate back to your `bots` directory and open the `bot.js` file where
we put the `status.addListener` function in the previous tutorials. We
will again pass this fils in as a `bot-url` parameter to
`status-dev-cli`, which will then execute the code in an Otto VM jail.

    cd ~/my-dapp/bots/ && nano bot.js

    # or use 'vi bot.js' if that's your preference

Then, open the file in the text editor of your choice and add in the
code provided. All we want to do is provide our user with a command,
called `hello` that they can issue into the chat and we can respond to.
In order to achieve this effect, we set up a simple `status.command()`
and pass in a name, title and description so as to identify our command
and display it to the user, along with some helpful information about
what it does.

    status.command({
        name: "hello",
        title: "HelloBot",
        description: "Helps you say hello",
        color: "#CCCCCC",
        preview: function (params) {
            var text = status.components.text({
                style: {
                    marginTop: 5,
                    marginHorizontal: 0,
                    fontSize: 14,
                    fontFamily: "font",
                    color: "black"
                }
            }, "Hello from the other side!");

            return {markup: status.components.view({}, [text])};
        }
    });

We then set a color for the command to appear in set up our preview
function. You can read about exactly what the preview function can
achieve in the formal API specification but, in a nutshell, the preview
defines what the user will see returned in the markup - i.e. where the
messages appear.

Here, we are creating a simple text response by setting up a variable
that we pass to `status.components.text`, which the perceptive will
notice is a React Native component - a whole bunch more of which are
available and detailed in the formal specification. Beneath the text, we
are creating a standard response of "Hello from the other side\!", all
of which will be returned as json to the markup. Note again the use of
another React Native component - `status.components.view`.

Go ahead and serve your dapp again:

    http-server

    status-dev-cli list --ip <DEVICE-IP>

    status-dev-cli watch $PWD "<whisper-identity>" --ip <DEVICE-IP>

And there you go - we are now capable of greeting and interacting with
our bot in two ways\! You should be able to see your DApp, navigate to
it, tap the new `/hello` command you see above the text input field and
see your new Dapp respond.

#### Embark

    cd ~/embark_demo/app/js/bot.js && nano bot.js

Here, we are going to create a simple text response by setting up a
variable that we pass to `status.components.text`, which the perceptive
will notice is a React Native component - a whole bunch more of which
are available and detailed in the formal specification. Beneath the
text, we are creating a standard response of "Hello from the other
side\!", all of which will be returned as json to the markup. Note again
the use of another React Native component - `status.components.view`.

    status.command({
        name: "hello",
        title: "HelloBot",
        description: "Helps you say hello",
        color: "#CCCCCC",
        preview: function (params) {
            var text = status.components.text({
                style: {
                    marginTop: 5,
                    marginHorizontal: 0,
                    fontSize: 14,
                    fontFamily: "font",
                    color: "black"
                }
            }, "Hello from the other side!");

            return {markup: status.components.view({}, [text])};
        }
    });

Go ahead and `run` your Embark application again and watch for changes
to your new, interactive bot. We have already made sure that the `js` is
built to, and served in, the right place, so we should just be able to
list the DApps and bots currently in Status and just watch the
"embark-demo-bot" we set up earlier.

    # Only if using an Android device
    adb reverse tcp:8000 tcp:8000

    embark run

    status-dev-cli list --ip <DEVICE-IP>

    status-dev-cli watch $PWD "<whisper-identity>" --ip

And there you go - we are now capable of greeting and interacting with
our bot in two ways\! You should be able to see your DApp, navigate to
it, tap the new `/hello` command you see above the text input field and
see your new Dapp respond.

#### Truffle

    cd ~/truffle-box-status/public/bot/ && nano bot.js

Navigate back to the truffle box `public` directory and open the
`bot.js` file we previously created so as to add the code provided and
display it through truffle.

    status.command({
        name: "hello",
        title: "HelloBot",
        description: "Helps you say hello",
        color: "#CCCCCC",
        preview: function (params) {
            var text = status.components.text({
                style: {
                    marginTop: 5,
                    marginHorizontal: 0,
                    fontSize: 14,
                    fontFamily: "font",
                    color: "black"
                }
            }, "Hello from the other side!");

            return {markup: status.components.view({}, [text])};
        }
    });

Much like we did last time, just find the `whisper-identity` of your
DApp and `watch` for changes.

    status-dev-cli list --ip <DEVICE-IP>
    # dapp-0x74727566666c652d626f782d737461747573 (Name: "truffle-box-status", DApp URL: "http://<MACHINE-IP>:3000")

    # Make sure you're in the truflle-box-status/ directory as that is what $PWD refers to below
    cd ~/truffle-box-status

    # I would use dapp-0x74727566666c652d626f782d737461747573 for <whisper-identity> below, Substitute your own one in.
    status-dev-cli watch $PWD "<whisper-identity>" --ip <DEVICE-IP>

We can now write a 1-1 chatbot that responds when users send messages to
it using the `status.addListener` method provided by the API, but what
if we want to provide our users with a command they can interact with,
rather than just waiting for them to send us some kind of message, or
take some other action?

We need to use the `status.command()` method. As usual, we can do this
ourselves through the barebones `status-dev-cli`, or through the popular
frameworks currently available on Ethereum.

### My First Interactive Suggestions Area

Let's go ahead and make that `hello` command we just created a little
bit more clever.

    cd ~/my-dapp/bots/ && nano hello.js

Instead of the preview parameter we used last time, let's build another
`status.command()` and add in a `params` option with a `suggestions`
object in it. This will create a suggestions area which - if you refer
back to the [Overview](Docs_\(API\)#Overview "wikilink") - is what rolls
up above the keyboard, such as you see when turning on the debugging
server with the options `On` and `Off`.

    status.command({
        name: "greet",
        title: "Greeter",
        description: "Helps you choose greetings",
        color: "#0000ff",
        params: [{
            name: "greet",
            type: status.types.TEXT,
            suggestions: helloSuggestions
        }]
    })

"But wait\!" we hear you cry, "what on earth is that `helloSuggestions`
thing about?\!". Well, let’s make a function to explain. This will
return a `scrollView` with two suggestions: "Hello", and "Goodbye".
Don’t get intimidated by the length, there’s actually not much to it.

First, we set up the helpers that will style the suggestion as it
appears in the suggestions area.

    function suggestionsContainerStyle(suggestionsCount) {
        return {
            marginVertical: 1,
            marginHorizontal: 0,
            keyboardShouldPersistTaps: "always",
            height: Math.min(150, (56 * suggestionsCount)),
            backgroundColor: "white",
            borderRadius: 5,
            flexGrow: 1
        };
    }
    var suggestionSubContainerStyle = {
        height: 56,
        borderBottomWidth: 1,
        borderBottomColor: "#0000001f"
    };

    var valueStyle = {
        marginTop: 9,
        fontSize: 14,
        fontFamily: "font",
        color: "#000000de"
    };

Now, we need to write the main star of our show - that mysterious
`helloSuggestions` function we passed into the `params` of our
`status.command()`. The objective here is to return two touchable
buttons - one for `Hello` and the other for `Goodbye`. We are making
quite extensive use of React native Components here, so we expect at
least some level of familiarity with the framework in order to get the
most out of these tutorials.

    function helloSuggestions() {
        var suggestions = ["Hello", "Goodbye"].map(function(entry) {
            return status.components.touchable(
                {onPress: status.components.dispatch([status.events.SET_VALUE, entry])},
                status.components.view(
                    suggestionsContainerStyle,
                    [status.components.view(
                        suggestionSubContainerStyle,
                        [
                            status.components.text(
                                {style: valueStyle},
                                entry
                            )
                        ]
                    )]
                )
            );
        });

        // Let's wrap those two touchable buttons in a scrollView
        var view = status.components.scrollView(
            suggestionsContainerStyle(2),
            suggestions
        );

        // Give back the whole thing inside an object.
        return {markup: view};
    }

The main point of this example is that your `suggestions` parameter
should accept users’ input, and then return a component to be rendered.

We have already added this contact to Status, so let's se if we can set
up our environment to do some live reloading so that we don't have to
revert to the cli every time we want to change something in the process
of building all the new things.

    http-server

    status-dev-cli watch $PWD "<whisper-identity>" --ip <DEVICE-IP>

You can also work with the `suggestionsTrigger` parameter. Now that
we’ve covered `params` and the possibility of `suggestions`, it’s easy
to see that `suggestionsTrigger` will take a string corresponding to an
event that, when triggered, will show all your `suggestions`. If you
don’t include this parameter, the default is "on-change", so your
suggestions will show when your users selects the command.

It's also worth knowing about the `fullscreen` option. If your command
has `suggestions`, this `param` controls whether that list of
suggestions expands to fill the entire screen. If your command has a lot
of suggestions, you might want to set `fullscreen` to `true`, so that
your users don’t have to pull the list upwards. On the other hand, if
your command has only a few suggestions and you set `fullscreen` to
`true`, your users will have to pull the list downwards to keep it from
hiding the screen. Choose whichever will be most convenient to your
users, considering your command’s suggestions.

OK, so we can build up a basic command, show it to the user and have it
do something (like return text) when the user taps it. That's great, but
what happens if we want to be a bit more dynamic and interactive and
suggest to our users a range of options to choose from when issuing the
command? Let's take a look...

For another full example of an interactive suggestion area, please take
a look over our [Demo
Bot](https://github.com/status-im/status-react/blob/develop/bots/demo_bot/bot.js),
which makes prominent use of the `defineSubscription` method from the
API and may be helpful for those looking to work with things like the
`status.component.slider` React Native Component.

You could also build a location bot following a similar pattern. We
provide the code for you, just because we can. See if you can get it all
working right this time...

    I18n.translations = {
        en: {
            location_title: 'Location',
            location_suggestions_title: 'Send location',
            location_description: 'Share your location',
            location_address: 'address'
    }}

    status.command({
        name: "location",
        title: I18n.t('location_title'),
        description: I18n.t('location_description')
        hideSendButton: true,
        sequentialParams: true,
        params: [{
            name: "address",
            type: status.types.TEXT,
            placeholder: I18n.t('location_address')
        }]
    })

Or, even better, can you get some location suggestions to display?

    function locationsSuggestions (params) {
        var result = {title: "Send location"};
        var seqArg = params.seqArg ? params.seqArg : "";

        if (seqArg == "Dropped pin") {
            result.markup = ["view", {}, ["text", {}, "Dropped pin" + seqArg]];
        } else if (seqArg != "") {
            result.markup = ["view", {}, ["text", {}, "Let's try to find something " + seqArg]];
        } else {
            result.markup = ['current-location', {showMap: true}];
        }

        return result;
    }