\--- weight: 10 title: API Introduction ---

1.  Introduction

DOCUMENTATION STILL WORK IN PROGRESS

Welcome to the Status API\! Tread carefully, for you tread on the dreams
of a better web.

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

1.  Quickstart

If you already have a DApp with a web interface, then this will be the
quickest Quickstart ever (trademark pending). Simply open Status,
navigate to Console, hit the \`@browse\` command and type in the url
where your DApp is running.

Voila\! Users in Status can already see your DApp and interact with it
(on the Ropsten Test Network). Make some mobile optimizations of your
own and you're away. That is the power of decentralized, web3
technologies. Awesome, right?

OK, but what if \`(a)\` I don't have a DApp but want to learn how to
build one on Status, or \`(b)\` I want to use this awesome API to make
the most of an awesome, native mobile UX, or \`(c)\` I know what I'm
doing and want to create fully decentralized, chatbot functionality for
my DApp?

1.  Examples

At Status, we eat our own dogfood\! All commands within Status are
written with our API, take a look at the
\[status-react/bots\](https://github.com/status-im/status-react/tree/develop/bots)
directory for examples.

\--- weight: 30 title: Status API Reference ---

1.  status
(API)

<https://github.com/status-im/status-react/blob/develop/resources/status.js#L160>

1.  1.  status

Remember, this is all about Ethereum. Anywhere.

Below you will find the formal API specification from which you can
glean all the information you need to make your DApp truly mobile, with
native mobile commands and an intuitive chat interface through which
users can actually see, interact with, and pay for the services you
offer.

1.  1.  status.command

\> A really simple template:

\`\`\`js status.command({

`name: "hello",`
`title: "HelloBot",`
`description: "Helps you say hello",`
`color: "#7099e6",`
`preview: function () {`
`return status.components.text({}, "you’re saying hello");`
`}`

}); \`\`\`

\> It is important to note that params are available for any
status.command(), including in params itself. For instance, if your user
sends /hello whatsup, the input "whatsup" will be available in your
command under params.hello. You can add additional params to - for
instance - preview like so:

\`\`\`js params: \[{

`name: "hello",`
`type: status.types.TEXT`
`placeholder: "Why not say hello"`

}\], \`\`\`

\> The placeholder parameter above only applies if your users haven’t
input anything into the command, not even the name. You can use it to
include helpful guidance where necessary, i.e. "Type your password".
Alternatively, you can also include suggestions for your users’ input.
This should return a component to be rendered. For instance, if you are
using the Console DApp and you select the /faucet command, you’ll see
two suggestions to choose from.

\> Example validator function (this specific example will raise an error
if your user doesn’t input a string. Notice that you should return your
message inside one of the status.components):

\`\`\`js validator: function(params) {

`if (!params.hello) {`
`  return status.components.text({}, "Say hello");`
`  }`
`}`
` ``` `

\> Example of the request command to get a user send some amount of
Ether somewhere:

\`\`\`js handler: function (params) {

`       return {`
`           event: "request",`
`           params: [params.amount],`
`           request: {`
`               command: "send",`
`               params: {`
`                   amount: params.amount`
`               }`
`           }`
`       };`
`   },`

\`\`\`

\> Full example of pretty much all the status.command stuff in action:

\`\`\`js status.command({

`   name: "faucet",`
`   title: I18n.t('faucet_title'),`
`   description: I18n.t('faucet_description'),`
`   color: "#7099e6",`
`   registeredOnly: true,`
`   params: [{`
`       name: "url",`
`       type: status.types.TEXT,`
`       suggestions: faucetSuggestions,`
`       placeholder: I18n.t('faucet_placeholder')`
`   }],`
`   preview: function (params) {`
`       return {`
`           markup: status.components.text(`
`               {},`
`               params.url`
`           )`
`       };`
`   },`
`   shortPreview: function (params) {`
`       return {`
`           markup: status.components.text(`
`               {},`
`               I18n.t('faucet_title') + ": " + params.url`
`           )`
`       };`
`   },`
`   validator: function (params, context) {`
`       var f = faucets.map(function (entry) {`
`           return entry.url;`
`       });`

`       if (f.indexOf(params.url) == -1) {`
`           var error = status.components.validationMessage(`
`               I18n.t('faucet_incorrect_title'),`
`               I18n.t('faucet_incorrect_description')`
`           );`

`           return {markup: error};`
`       }`
`   }`

}); \`\`\`

1.  1.  Parameters

Argument | Description --------- | ----------- name | What your users
will type in following a forward slash to invoke the command. For
instance, if you wrote \`name: "hello"\`, your user might invoke
\`/hello\`. There is an additional \`params\` object available on any of
the below commands, including in params itself. title | This is what
will appear in the list of suggestions when a user starts typing a
command. description | Appears below the \`title\` in the list of
suggestions and allows you to provide a description of the command.
validator | Allows you to check your users’ input before sending off the
command. It takes a function, which should return an error if the input
is invalid. color | Defines the background color of the name of your
command as it appears in the list of suggestions. Give commands
different colors to help your users distinguish commands easily, and to
harmonize with your DApp’s brand and color scheme. icon | Define which
icon will appear next to action messages, such as sending Ether or
requesting a location. Think an arrow for sending, a pin for location
etc. params | Defines all the possible inputs to your command. Requires
an array holding an object, with possible parameters \`name\`,
\`placeholder\`, \`suggestions\`, and one of the \`status.types\`, which
are: \`status.types.TEXT\`, \`status.types.NUMBER\`,
\`status.types.PHONE\`, and \`status.types.PASSWORD\`. preview | Defines
what your user will see as a result of \*their\* action, before any
other response. The preview parameter takes a function, which should
return a \`status.component\`. shortPreview | While \`preview\` controls
how your command appears within your DApp’s chat interface,
\`short-preview\` controls how your commands get shown in the list of
chats, before your users tap on your chat. \`short-prview\` expects two
params: \`icon\` and \`params\`. onSend | A self-explanatory param that
takes a function which will be executed when the user presses the "send"
button. It will return a map that should contain the markup value. If
you specify this function, there will be no way to send a command to the
chat and, in this case, the area (it’s called the \`result-box\`) with a
specified markup will be displayed instead. fullscreen | If your command
has suggestions, this param controls whether that list of suggestions
expands to fill the entire screen. See the interactive suggestion area
tutorial for more. request | This will allow you to request any action
from the user, such as a phone number, password, confirmation to send
Ether etc. Used with the \`executeImmediately\` option set to \`true\`,
it will create a message the user can tap on an execute immediately.
executeImmediately (Boolean) | If true, this means that the
\*\*response\*\* will be executed immediately when you press on it. For
instance, when you see a response in a chat, you don’t have to type
something — you just need to press on a response and it will be executed
right after that. sequentialParams (Boolean) | Specifies the way command
arguments will be "requested". The default approach (\`sequentialParams
= false\`) is to type a command this way: \`/send 0.1 ETH somebody\`,
where \`0.1\`, \`ETH\` and \`somebody\` are arguments. However, there
may be situations when you want to ask each argument separately. You can
see the difference by executing \`/password\` command; it asks you for a
password and, only after the user has provided one, requests
confirmation. Currently there is one limitation — you can use argument
types (\`type\` value) only for \`sequentialParams\`, and if you want to
(for example) hide the argument input, you should use
\`sequentialParams\` handler (\!= null) | Of course, you probably want
the command to do something when your users call it\! The \`handler\`
parameter takes a function to accomplish this. For instance, suppose
your user inputs \`/hello howdy\`. "Howdy" is a valid string, and will
pass the \`hello\` validator. From there, your handler could take over
to send this greeting to another user: \`handler:
web3.shh.post(params.hello)\`.

1.  1.  status.response

\> An example response for a confirmation code sent via SMS:

\`\`\`js status.response({

`   name: "confirmation-code",`
`   color: "#7099e6",`
`   description: I18n.t('confirm_description'),`
`   sequentialParams: true,`
`   params: [{`
`       name: "code",`
`       type: status.types.NUMBER`
`   }],`
`   validator: function (params) {`
`       if (!/^[\d]{4}$/.test(params.code)) {`
`           var error = status.components.validationMessage(`
`               I18n.t('confirm_validation_title'),`
`               I18n.t('confirm_validation_description')`
`           );`

`           return {markup: error};`
`       }`
`   }`

}); \`\`\`

Now that you’ve covered all the parameters for \`status.command()\`, you
can easily understand \`status.response()\`. This method takes the same
parameters that \`status.command()\` does. The difference is that, with
this method, you can actively ask a user to issue a command of their
own.

For example, the Status DApp Wallet allows you to \`/request\` money. In
that case, the person you’re requesting money from will see the result
of \`status.response(send)\`. In other words, they’ll be asked to give a
command, \`/send\`, in response to your \`/request\` command. Please
check line 77-158
\[here\](https://github.com/status-im/status-react/blob/30596f743f0a6ac0b7aec575cc1483dd306cc1ef/bots/wallet/bot.js\#L77)
for more code.

The Wallet example illustrates that, as a DApp developer, you may wish
to use \`status.command()\` and \`status.response()\` together to create
dialogues of commands. You can also just use \`status.response()\` by
itself to prompt your users to enter necessary information as part of
the onboarding process for your DApp.

Because \`status.command()\` and \`status.response()\` take the same
parameters, you can sometimes use nearly the same code for both of them.
You simply have to consider when you want to ask a user to issue a
command, and when you want to just make the command available. Most of
the time, you’ll use \`status.command()\`.

1.  1.  status.on

\`\`\`js status.on("init", function(params, context) {

`status.sendMessage("Hello, man!");`

}); \`\`\`

This method allows your DApp to respond to events. It requires an event
name as a string, and a callback function. With the "init" option shown
here, your DApp will trigger \`status.sendMessage()\` when the Status
app loads your DApp — your DApp will greet your users even before they
have clicked on it. Other options include "text-change" and "message"

1.  1.  status.addListener

\`\`\`js status.addListener("on-message-input-change",

`function (params, context) {`
`   return jsSuggestions({code: params.message}, context);`

}); \`\`\`

\`\`\`js status.addListener("init",

`function (params, context) {`
`   return {"text-message": "Hey, man!"};`

}); \`\`\`

\`\`\`js
status.addListener("on-message-send",

`function (params, context) {`
`   if (isNaN(params.message)) {`
`       return {"text-message": "Seems that you don't want to send money :("};`
`   }`

`   var balance = web3.eth.getBalance(context.from);`
`   var value = parseFloat(params.message);`
`   var weiValue = web3.toWei(value, "ether");`
`   if (bn(weiValue).greaterThan(bn(balance))) {`
`       return {"text-message": "No way man, you don't have enough money! :)"};`
`   }`
`   try {`
`       web3.eth.sendTransaction({`
`           from: context.from,`
`           to: context.from,`
`           value: weiValue`
`       });`
`       return {"text-message": "You are the hero, you sent " + value + " ETH to yourself!"};`
`   } catch (err) {`
`       return {"text-message": "Something went wrong :("};`
`   }`

}); \`\`\`

Listener | Description -------------------------- | -----------
on-message-input-change | This is analogous to a \`text-change\` event,
except that it targets the chat’s input. You can find the jsSuggestions
that are passed in in the example
\[here\](https://github.com/status-im/status-react/blob/develop/bots/console/bot.js\#L312).
init | Is called once, when a new session begins ("session" is currently
taken to mean the interval between login and logout from a user's
account). In the example provided the bot will just send the "Hey,
man\!" message to the user, but it could also it could return \`markup\`
which will be shown in the suggestions area, etc. on-message-send | Will
be called when any (not command) message is sent. The example provided
shows you how to send ether to yourself as a neat way of testing the
functionality.

1.  1.  status.localizeNumber

\`\`\`js preview: function (params, context)
{

`       return {`
`           markup: status.components.text(`
`               {},`
`               I18n.t('request_requesting') + " "`
`               + status.localizeNumber(params.amount, context.delimiter, context.separator)`
`               + " ETH"`
`           )`
`       };`
`   },`
` ``` `

A simple method to try and ensure that whatever number the user inputs,
is put into a usable format. The example provided shows it in use to
make sure the number requesting ETH is handled correctly. This is part
of a larger \`status.command()\` object that can be found
\[here\](https://github.com/status-im/status-react/blob/30596f743f0a6ac0b7aec575cc1483dd306cc1ef/bots/wallet/bot.js\#L182).

1.  1.  status.types

\`\`\`js

`   types: {`
`       TEXT: 'text',`
`       NUMBER: 'number',`
`       PHONE: 'phone',`
`       PASSWORD: 'password'`
`   }`

\`\`\`

Types dictate what sort of data your users may input. The type you
specify will determine what kind of keyboard gets shown to the user (if
you have not already defined a custom one) so that they can input their
response easily. It is important to specify them for a smooth, easy, and
intuitive UI.

1.  1.  status.types.TEXT

\`\`\`js

status.command({

`   name: "faucet",`
`   title: I18n.t('faucet_title'),`
`   description: I18n.t('faucet_description'),`
`   color: "#7099e6",`
`   registeredOnly: true,`
`   params: [{`
`       name: "url",`
`       type: status.types.TEXT,`
`       suggestions: faucetSuggestions,`
`       placeholder: I18n.t('faucet_placeholder')`
`   }],`

\`\`\`

If you define a \`text\` input, when the user responds, it will pop up a
normal QWERTY keyboard. The example provided shows the first part of the
\`status.commnad()\` we use to interact with our faucet so that our
users can get some (test) ether to do interesting things with.

1.  1.  status.types.NUMBER

\`\`\`js status.response({

`   name: "confirmation-code",`
`   color: "#7099e6",`
`   description: I18n.t('confirm_description'),`
`   sequentialParams: true,`
`   params: [{`
`       name: "code",`
`       type: status.types.NUMBER`
`   }],`

\`\`\`

Defining \`number\` will result in the keyboard opening to it's number
section. The example provided shows the first part of the
\`status.response()\` object we use to request a confirmation code from
the user when syncing their phone contacts.

1.  1.  status.types.PHONE

\`\`\`js var phoneConfig = {

`   name: "phone",`
`   registeredOnly: true,`
`   icon: "phone_white",`
`   color: "#5bb2a2",`
`   title: I18n.t('phone_title'),`
`   description: I18n.t('phone_description'),`
`   sequentialParams: true,`
`   validator: function (params) {`
`       return {`
`           validationHandler: "phone",`
`           parameters: [params.phone]`
`       };`
`   },`
`   params: [{`
`       name: "phone",`
`       type: status.types.PHONE,`
`       suggestions: phoneSuggestions,`
`       placeholder: I18n.t('phone_placeholder')`
`   }]`

}; \`\`\` Defining \`phone\` will result in the keyboard opening to it's
number section as well. One example should suffice to give you a general
idea of how this works, so we have provided an excerpt of how we do
phone cofirmation using the \`PHONE\` type. You can find the full code
\[here\](https://github.com/status-im/status-react/blob/develop/bots/console/bot.js\#L450)

1.  1.  status.types.PASSWORD

\`\`\`js status.response({

`   name: "password",`
`   color: "#7099e6",`
`   description: I18n.t('password_description'),`
`   icon: "lock_white",`
`   sequentialParams: true,`
`   params: [`
`       {`
`           name: "password",`
`           type: status.types.PASSWORD,`
`           placeholder: I18n.t('password_placeholder'),`
`           hidden: true`
`       },`
`       {`
`           name: "password-confirmation",`
`           type: status.types.PASSWORD,`
`           placeholder: I18n.t('password_placeholder2'),`
`           hidden: true`
`       }`

\`\`\`

Defining \`password\` will result in the keyboard opening to it's
variable character section. The example provided shows the first part of
the \`status.response()\` object we use to request and confirm a
password from our user when they are setting up their account.

1.  1.  status.events

\`\`\`js

`   events: {`
`       SET_VALUE: 'set-value',`
`       SET_COMMAND_ARGUMENT: 'set-command-argument'`
`   }`

\`\`\`

Events are essentially the glue that allow you to commuicate with the
Status API effectively.

1.  1.  status.events.SET_VALUE

\`\`\`js {onPress:
status.components.dispatch(\[status.events.SET_VALUE, entry\])}, \`\`\`

This method will completely update the content of text input. The full
function around the snippet example provided can be found
\[here\](https://github.com/status-im/status-react/blob/30596f743f0a6ac0b7aec575cc1483dd306cc1ef/bots/console/bot.js\#L560).

1.  1.  status.events.SET_COMMAND_ARGUMENT

\`\`\`js {onPress:
status.components.dispatch(\[status.events.SET_COMMAND_ARGUMENT, \[0,
entry.url\]\])}, \`\`\` As opposed to \`SET_VALUE\`, this event will
only update a single argument which you define. The full function around
the snippet example provided can be found
\[here\](https://github.com/status-im/status-react/blob/30596f743f0a6ac0b7aec575cc1483dd306cc1ef/bots/console/bot.js\#L485).

1.  1.  status.components

Create React Native Components yourself or use our pre-fabricated
components.

1.  1.  status.components.view

\`\`\`js function faucetSuggestions(params)
{

`   var suggestions = faucets.map(function (entry) {`
`       return status.components.touchable(`
`           {onPress: status.components.dispatch([status.events.SET_COMMAND_ARGUMENT, [0, entry.url]])},`
`           status.components.view(`
`               suggestionContainerStyle,`
`               [status.components.view(`
`                   suggestionSubContainerStyle,`
`                   [`
`                       status.components.text(`
`                           {style: valueStyle},`
`                           entry.name`
`                       ),`
`                       status.components.text(`
`                           {style: descriptionStyle},`
`                           entry.url`
`                       )`
`                   ]`
`               )]`
`           )`
`       );`
`   });`

\`\`\`

Standard RN component - please see
\[JSCoach\](https://js.coach/react-native) for more. Expects 2 arguments
- \`options\` and \`element\`. The example provided shows how we use the
\`view\` component (in combination with the \`text\` component) to
provide different suggestions about which faucets our users can call to
get some test ether.

1.  1.  status.components.text

Standard RN component - please see
\[JSCoach\](https://js.coach/react-native) for more. Expects 2 arguments
- \`options\` and some array of strings \`s\`.

1.  1.  status.components.slider

Standard RN component - please see
\[JSCoach\](https://js.coach/react-native) for more. Expects only one
argument - \`options\`.

Please see the \`defineSubscription\` method below for an example of
this component in action.

1.  1.  status.components.image

Standard RN component - please see
\[JSCoach\](https://js.coach/react-native) for more. Expects only one
argument - \`options\`.

1.  1.  status.components.touchable

\`\`\`js function jsSuggestions(params, context)
{

`   var suggestions = getJsSuggestions(params.code, context);`
`   var sugestionsMarkup = [];`

`   for (var i = 0; i < suggestions.length; i++) {`
`       var suggestion = suggestions[i];`

`       if (suggestion.title.indexOf('*') >= 0) {`
`           suggestion.title = createMarkupText(suggestion.title);`
`       }`
`       var suggestionMarkup = status.components.view(jsSuggestionContainerStyle,`
`           [status.components.view(jsSubContainerStyle,`
`               [`
`                   status.components.text({style: jsValueStyle},`
`                       suggestion.title),`
`                   status.components.text({style: jsDescriptionStyle},`
`                       suggestion.desc)`
`               ])]);`
`       if (suggestion.pressValue) {`
`           suggestionMarkup = status.components.touchable({`
`                   onPress: status.components.dispatch([status.events.SET_VALUE, suggestion.pressValue])`
`               },`
`               suggestionMarkup`
`           );`
`       }`
`       sugestionsMarkup.push(suggestionMarkup);`
`   }`

`   if (sugestionsMarkup.length > 0) {`
`       var view = status.components.scrollView(jsSuggestionsContainerStyle(sugestionsMarkup.length),`
`           sugestionsMarkup`
`       );`
`       return {markup: view};`
`   }`

} \`\`\`

Standard RN component - please see
\[JSCoach\](https://js.coach/react-native) for more. Expects 2 arguments
- \`options\` and \`element\`. The example provided uses the
\`touchable\` component to make our suggestions that much more native
and-feeling and interactive. It's worth noting that \`touchable\`
components can only wrap \`view\` components, so any touchable text for
example needs to first be wrapped in a \`view\`.

1.  1.  status.components.scrollView

\`\`\`js

`   var view = status.components.scrollView(`
`       suggestionsContainerStyle(ph.length),`
`       suggestions`
`   );`

`   return {markup: view};`

\`\`\`

Standard RN component - please see
\[JSCoach\](https://js.coach/react-native) for more. Expects 2 arguments
- \`options\` and \`elements\`. In the code snippet provided, we have
taken the suggestions from the example above to do \`components.view\`
and now want to inject it into a scrollView component for a better and
smoother UI that our users have more control over.

1.  1.  status.components.webView

Standard RN component - please see
\[JSCoach\](https://js.coach/react-native) for more. Expects only 1
argument - \`url\` and is what we use to display - as expected - the
webView of a given DApp when browsing to it.

1.  1.  status.components.validationMessage

\> An example of using validationMessage within the validator argument
being passed to status.command.

\`\`\`js validator: function (params, context) {

`   var f = faucets.map(function (entry) {`
`       return entry.url;`
`   });`

`   if (f.indexOf(params.url) == -1) {`
`       var error = status.components.validationMessage(`
`           I18n.t('faucet_incorrect_title'),`
`           I18n.t('faucet_incorrect_description')`
`       );`

`       return {markup: error};`
`   }`

} \`\`\`

This is the only custom Status component and it takes just two strings,
and will return them wrapped in text components inside a view. You can
find the full example
\[here\](https://github.com/status-im/status-react/blob/develop/bots/console/bot.js\#L550).

1.  1.  status.components.bridgedWebview

Standard RN component - please see
\[JSCoach\](https://js.coach/react-native) for more. Expects only 1
argument - \`url\`.

1.  1.  status.components.subscribe

A method that allows you to add a subscription in the markup and expects
just 1 argument - \`path\`. The added subscription will get the value
from the bot-db using your specifiied path.

1.  1.  status.components.dispatch

\`\`\`js var view = \["view",
{},

`       ["text", {}, "Balance " + balance + " ETH"],`
`       ["text", {}, ["subscribe", ["doubledValue"]]],`
`       ["slider", {`
`           maximumValue: ["subscribe", ["balance"]],`
`           value: defaultSliderValue,`
`           minimumValue: 0,`
`           onSlidingComplete: ["dispatch", ["set", "sliderValue"]],`
`           step: 0.05`
`       }],`
`       ['touchable',`
`           {onPress: ['dispatch', ["set-value-from-db", "roundedValue"]]},`
`           ["view", {}, ["text", {}, "Set value"]]`
`       ],`
`       ["text", {style: {color: "red"}}, ["subscribe", ["validationText"]]]`
`   ];`

\`\`\`

A method that allows you to specify an event that will be dispatched on
some other event that occurs inside the markup (like pressing button,
text change etc.). By 'markup', we mean - essentially - the view, or the
mobile equivalent of the DOM. In the example provided, the variable
\`view\` is what we mean by markup (and is, in this case, part of our
\`superSuggestion\` function found
\[here\](https://github.com/status-im/status-react/blob/2862af86c960b481ddf64ec1713e14908a366616/bots/demo_bot/bot.js\#L31))
we specify that \`\["set", "sliderValue"\]\` will be dispatched on
\`onSlidingComplete\`. We can’t allow user to write a real js handler
here, so this is our workaround for now.

1.  1.  status.setSuggestions

A method that allows you to set the suggestions that will appear in the
markup, whether this is something simple like sending a message, or
pretty much anything

1.  1.  status.setDefaultDb and status.updateDb

\`\`\`js function superSuggestion(params, context)
{

`   var balance = parseFloat(web3.fromWei(web3.eth.getBalance(context.from), "ether"));`
`   var defaultSliderValue = balance / 2;`

`   var view = ["view", {},`
`       ["text", {}, "Balance " + balance + " ETH"],`
`       ["text", {}, ["subscribe", ["doubledValue"]]],`
`       ["slider", {`
`           maximumValue: ["subscribe", ["balance"]],`
`           value: defaultSliderValue,`
`           minimumValue: 0,`
`           onSlidingComplete: ["dispatch", ["set", "sliderValue"]],`
`           step: 0.05`
`       }],`
`       ['touchable',`
`           {onPress: ['dispatch', ["set-value-from-db", "roundedValue"]]},`
`           ["view", {}, ["text", {}, "Set value"]]`
`       ],`
`       ["text", {style: {color: "red"}}, ["subscribe", ["validationText"]]]`
`   ];`

`   status.setDefaultDb({`
`       sliderValue: defaultSliderValue,`
`       doubledValue: doubledValueLabel({value: defaultSliderValue})`
`   });`

`   var validationText = "";`

`   if (isNaN(params.message)) {`
`       validationText = "That's not a float number!";`
`   } else if (parseFloat(params.message) > balance) {`
`       validationText =`
`           "Input value is too big!" +`
`           " You have only " + balance + " ETH on your balance!";`
`   }`

`   status.updateDb({`
`       balance: balance,`
`       validationText: validationText`
`   });`

`   return {markup: view};`

}; \`\`\`

It is easiest to show you an example of these two methods at work in our
\[demo-bot\](https://github.com/status-im/status-react/blob/develop/bots/demo_bot/bot.js\#L31)
within a superSuggestions object we have created.

\`status.setDefaultDb\` is a method that can be used to add default
values to bot-db. These values will be applied to app-db only when
markup is rendered the first time. For instance, on each change in
command input suggestions handler for particular command's parameter is
called, and this handler returns some markup. If markup wasn't changed
between calls values passed to setDefaultDb will not be applied to
bot-db

\`status.updateDb\` is a method that updates the bot-db, even if the
markup doesn't contain any changes. It too expects just 1 argument -
\`map\`

1.  1.  status.sendMessage

This has just been implemented in the latest nightlies\! Go and grab one
of those and you'll have access to this route, as demonstrated by our
updated \[demo
bot\](https://github.com/status-im/status-react/blob/develop/bots/demo_bot/bot.js\#L106).
The snippet provided also reveals how to work with the javascript
concept of \`localStorage\`.

\`\`\`js status.addListener("on-message-send", function (params,
context)
{

`   var cnt = localStorage.getItem("cnt");`
`   if(!cnt) {`
`       cnt = 0;`
`   }`

`   cnt++;`

`   localStorage.setItem("cnt", cnt);`
`   if (isNaN(params.message)) {`
`       return {"text-message": "Seems that you don't want to send money :(. cnt = " + cnt};`
`   }`

`   var balance = web3.eth.getBalance(context.from);`
`   var value = parseFloat(params.message);`
`   var weiValue = web3.toWei(value, "ether");`
`   if (bn(weiValue).greaterThan(bn(balance))) {`
`       return {"text-message": "No way man, you don't have enough money! :)"};`
`   }`
`   web3.eth.sendTransaction({`
`       from: context.from,`
`       to: context.from,`
`       value: weiValue`
`   }, function (error, hash) {`
`       if (error) {`
`           status.sendMessage("Something went wrong, try again :(");`
`           status.showSuggestions(demoSuggestions(params, context).markup);`
`       } else {`
`           status.sendMessage("You are the hero, you sent " + value + " ETH to yourself!")`
`       }`
`   });`

}); \`\`\`

1.  1.  status.defineSubscription

\`\`\`js function round(n) {

`   return Math.round(n * 100) / 100;`

}

function doubledValueLabel(params)
{

`   var value = round(params.value);`
`   return "sliderValue = " + value +`
`       "; (2 * sliderValue) = " + (2 * value);`

}

status.defineSubscription(

`   // the name of subscription and the name of the value in bot-db`
`   // associated with this subscription`
`   "doubledValue",`
`   // the map of values on which subscription depends: keys are arbitrary names`
`   // and values are db paths to another value`
`   {value: ["sliderValue"]},`
`   // the function which will be called as reaction on changes of values above,`
`   // should be pure. Returned result will be associated with subscription in bot-db`
`   doubledValueLabel`

);

status.defineSubscription(

`   "roundedValue",`
`   {value: ["sliderValue"]},`
`   function (params) {`
`       return round(params.value);`
`   }`

); \`\`\` This method was added to allow a bot to react on changes in
the bot-db and add another calculated value to that bot-db in the
result. It expects 3 arguments: the name of subscription, a map of
values on which the subscription depends, and a callback function.

\--- weight: 20 title: Tutorials ---

1.  Tutorials

<!-- end list -->

1.  1.  Overview

Just before we get started, it's well worth acquainting yourself with
some of our terminology so you'll be able to make sense of it all. This
anatomy establishes the different sections of the chat interface and
establishes a common verbiage. The main components are:

  - Message
  - Input
  - Keyboard
  - Suggestions

\!\[Chat Anatomy\](images/chat-anatomy.png)

Please take some time to familiarize yourself with all the areas and the
different configurations possible depending on what you want to do.
Missing from the above is what we refer to throughout this documentation
as the 'markup', by which we mean the mobile equivalent of the 'view',
i.e. where the messages appear.

Creating all of these native setups is totally possible through the API
- just read on.

1.  1.  Installing Status

OK, let's learn how to build our first DApp on Status (mobile-first
ftw\!). To progress further, you need to have Status running either:

  - on a real phone,

<!-- end list -->

  - in an Android simulator, or

<!-- end list -->

  - in an iOS simulator.

You can go to [1](https://test.status.im)(https://test.status.im) to
download for Android. Or you can DM @jarradhope with your email in our
\[Slack\](status-im.slack) for ios Testflight.

<aside class="success">

Please note that these documents are intended for the latest version of
"status-dev-cli" (^3.2.8) and the latest release of Status app (^0.9.8).
To update \`status-dev-cli\` please run "npm uninstall -g
status-dev-cli" and then "npm i -g status-dev-cli".

</aside>

You can build Status yourself for either Android or iOS by following
\[these
guidelines\](https://wiki.status.im/contributing/development/building-status/).
There, you will find instructions for installing an Android simulator,
or starting up Status in the Xcode simulator.

Then, connect your phone to your Wi-Fi network (do not use USB).
Development machine and phone should be in the same network. Navigate to
the Console, select the \`/debug\` command and choose the \`On\`
suggestion. You’ll get back a message telling you that debugging is on,
and that you can use the
\[status-dev-cli\](https://github.com/status-im/status-dev-cli) tools.

You also need to install \`status-dev-cli\` to make talking between
Status and your machine a breeze.

\`\`\`shell npm i -g status-dev-cli \`\`\`

\!\[With your phone connected, /debug "On"\](images/starting.png)

  - With your phone connected, /debug "On"\*

<!-- end list -->

1.  1.  Debugging

OK, so Status is installed, but how do we interact with it?

You can check \`<DEVICE-IP>\` by running the \`scan\` command in the
terminal. Another useful command we will be using a lot later on is
\`list\` which will return a list of all DApps you have added to Status.

\`\`\`shell status-dev-cli scan status-dev-cli list --ip <DEVICE-IP>
\#be sure to use the IPv4 address \`\`\`

You will see that these tutorials also use \`<MACHINE-IP>\` throughout.
This refers to the INTERNAL IPv4 address of your development machine.
You can find that by running the command provided.

\`\`\`shell ifconfig | grep inet

1.  Look for this line - i.e. your IPv4 address - and use the equivalent
    of 192.168.0.103

\--\> inet 192.168.0.103 netmask 0xffffff00 broadcast 192.168.0.255
\`\`\`

Even more importantly though, how can we actually debug the code we
write? We know how important this is for good developers and we're still
working on it. Currently, the options are limited, but we ask you to
work with us here as it will improve rapidly over the next little while.

The first and the most powerful option is the development tools included
in your browser. Chrome supports remote debugging, and you can easily
connect to any web view on your mobile and debug it right from the
browser. The detailed instructions can be found
\[here\](https://developers.google.com/web/tools/chrome-devtools/remote-debugging/).

If you use iOS, you can use Safari on your Mac to debug any remote web
page.
\[Here\](http://developer.telerik.com/featured/a-concise-guide-to-remote-debugging-on-ios-android-and-windows-phone/)
is a nice guide that will explain everything to you.

Unfortunately, you cannot use your browser's development tools to debug
bots, as switching the node only works for DApps currently. So, the
option we provide for bots is the ability to extract logs.

\`\`\`shell status-dev-cli log <DAPP-IDENTITY> --ip <DEVICE-IP> \`\`\`

1.  1.  Networking

We give instructions here for Genymotion (a popular Android emulator).

1\. Install genymotion 1. Create a genymotion virtual device 1. Switch
to network bridge mode (in the settings of your virtual device)

\!\[Switch to "Bridge"\](images/networking.png)

1\. Start virtual device 1. Install status.im apk from nightly builds by
dragging onto emulator window 1. Start status.im app on virtual device
1. Turn on debugging in console (/debug On). 1. Open terminal and run
\`status-dev-cli scan\` it returns two \`<DEVICE-IP>\` addresses, use
\`192.168.1.\*\`, and ignore \`192.168.56.\*\`. Alternately retrieve the
device ip from the emulator window \`Settings \> About phone \> Status
\> IP address\`. 1. Run \`status-dev-cli add \[dapp\] --ip
<DEVICE-IP>\`. \*\*Replace\*\* \`\[dapp\]\` with your dapp details
(\`whisper-identity\`, etc) as stringified json or \*\*remove\*\*
\`\[dapp\]\` if those details are specified in a \`package.json\` in the
current directory.

1.  1.  My First DApp

{{% tabs DIY Embark Truffle %}} {{% tab DIY %}}

1.  1.  Do It Yourself with \`status-dev-cli\`

First, install the \`status-dev-cli\` tools globally using NPM. Then,
create a directory for your app to live in, switch into it, and create
an \`index.html\` file and an \`app.js\` file.

\`\`\`shell npm install -g status-dev-cli mkdir ~/my-dapp && cd my-dapp
touch index.html app.js \`\`\`

The index.html will be really simple. We are going to add several meta
tags to make our DApp look good on the small screens of mobile phones,
and add a span that will be used by our JavaScript.

Our \`app.js\` file will also be simple. We are going to display the
information about your account inside a span with \`account id\`. Status
injects \`web3.js\` automatically, so you have an access to web3
variable from everywhere and you don’t need to care about including
\`web3.js\` manually. However, you can do this and most probably you
want to do this if you want to make your DApps work on other platforms.

\> In index.html,
add:

\`\`\`html

<html>

<head>

<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">

<meta name="HandheldFriendly" content="True">

<meta name="MobileOptimized" content="320">

<meta name="viewport" content="width=device-width,minimum-scale=1,maximum-scale=1">

<title>

My DApp

</title>

<script src="app.js">

</script>

</head>

<body>

<h1>

Hello world\!

</h1>

`Web3 account: `<span id="account"></span>

</body>

</html>

\`\`\` \> In app.js, add:

\`\`\`js function onContentLoaded() {

` var accountSpan = document.getElementById("account");`
` accountSpan.innerHTML =  `
`   (typeof web3 === "undefined" ? "undefined" : web3.eth.accounts);`

} document.addEventListener("DOMContentLoaded", onContentLoaded); \`\`\`

You then need to install a really simple \`http-server\` from NPM (we
recommend it for ease-of-use), and start it in the \`my-dapp\` directory
we just created.

Open a new terminal session, navigate back to your \`my-dapp\`
directory, and go ahead and add your dapp to Status\! Make sure to pass
in the \`--ip\` flag using the address returned to you by Console, when
you \[enabled debugging\](\#enabling-debugging).

\`<MACHINE-IP>\` needs to be the internal IPv4 address returned when you
run \`ifconfig | grep inet\`.

\`\`\`shell npm install http-server -g http-server \`\`\`

\> In a new terminal window run the port forwarding and DApp connection
stuff. It is important to pass in the \`--ip\` flag with the IP address
listed by Console once you have selected the \`debug\` option and turned
it on.

\`\`\`shell status-dev-cli add '{"whisper-identity": "hello-bot",
"dapp-url": "<http://><MACHINE-IP>:8080/", "name": "Hello"}' --ip
<DEVICE-IP> \`\`\`

That's it\! You should be able to see your DApp in the chats, and
opening it should browse automatically to a page that shows your account
information. You can also do live-reloading once you're happy with this
by running \`status-dev-cli watch $PWD hello-bot --ip <DEVICE-IP>\`

  - Known Issues

1\) You may need to escape the \`json\` information you are passing into
\`status-dev-cli\` if you are on a windows machine, like so:
\`status-dev-cli add '{\\"whisper-identity\\": \\"my-dapp5\\",
\\"dapp-url\\": \\"<http://><MACHINE-IP>:8 080\\",\\"name\\": \\"My
DApp\\"}' --ip <DEVICE-IP>\`

2\) If using a real device, you also need to ensure that it is set to
use MTP (media transfer Protocol) in the USB-debugging settings. Open
the equivalent of your settings or develop options by pulling down the
top menu and clicking in the debugging options.

3\) The new version of \`status-dev-cli\` does not require you to do any
\`adb\` port forwarding any more and will, in fact, fail if you do. Just
use the \`scan\` and \`list\` commands as and when you need them.

Happy
travels\!

<aside class="success">

` You need not use NPM's http-server to serve content, there are innumerable ways of doing this. Just pick whatever is best for your environment.`

</aside>

{{% /tab %}}

{{% tab Truffle %}}

1.  1.  Truffle

OK, so we can write a little HTML5 DApp that displays information to
Status users through a simple webView component. However, we want to do
so much more\! We want to write smart contracts, deploy them, and
interact with them through a fully mobile user interface; and we want to
build decentralized chatbots that live within Status and make friends
with all the humans.

If you want to get straight to making chatbots, please go
\[here\](\#my-first-1-1-chatbot).

Frameworks can lighten the load of developing considerably, and so we
include here some quick examples to get you up and running with the two
most popular Ethereum frameworks currently available - Truffle and
Embark.

Firstly, we'll need a network to develop against, so let's go get
\`testrpc\` - a neat little simulation of the rules and logic of the
Ethereum network that runs much faster than the live network (because
it's only simulating things) and is very useful for quick and dirty
iterations.

\`\`\`shell npm install -g ethereumjs-testrpc testrpc -p 8546 \`\`\`

That’s it\! It will show you a list of available accounts, private keys,
your HD wallet and mnemonic. Please note that we're running testrpc on
(non-default) RPC port 8546, just to avoid potential conflict with the
node running inside Status. If using android, you need to open the
connection from your PC to your phone on that port, so Status can listen
for changes there.

We also need to make sure that our Status client is listening to our new
\`testrpc\` network, rather than Ropsten, so that we can get the
information we need about our contracts etc. Luckily, this is as easy as
running:

\`\`\`shell status-dev-cli switch-node "<http://><MACHINE-IP>:8546" --ip
<DEVICE-IP>

1.  Of course, there are options. You can use go-ethereum instead of
    TestRPC, and instead of port forwarding you can switch to any other
    accessible node using its IP
address.

\`\`\`

<aside class="success">

`   Please note that "switch-node" only works for DApps, and NOT for bots. You javascript is still confined to the Otto VM jail for now. We are working on a solution, but please use "dapp-url" if developing against a local network for now.`

</aside>

Open a new shell (i.e. a new Terminal window or tab) for the next part.
You’ll leave \`testrpc\` running in the first window, and use the second
window for the rest of the tutorial.

Now that you have \`testrpc\` is going, and a new shell is open, run:

\`\`\`shell npm install -g truffle \# Version 3.0.5+ required. \`\`\`

This installs the Truffle framework, and you can find its GitHub \[page
here\](https://github.com/trufflesuite/truffle).

With Truffle installed, we can grab the Status Truffle Box, and get a
basic DApp running. All the Truffle Boxes also include the app
frameworks React and Redux, which were designed by Facebook and are
widely used by app developers. You can find the other Truffle Boxes
\[here\](https://truffle-box.github.io/).

To install the Status Truffle box, all you have to do is run this
command in the same Terminal window:

\`\`\`shell git clone
<https://github.com/status-im/truffle-box-status.git>

1.  Change into the truffle box directory and install the node
    dependencies

cd truffle-box-status && npm install

1.  Compile the contracts from Solidity (much like JavaScript) into
    runnable EVM code

`truffle compile`

1.  Publish the compiled contracts to your network. testrpc must already
    be running

`truffle migrate`

\`\`\`

\!\[Example on OS X: testrpc running on the left, and installing Truffle
on the right\](images/starting-a-dapp-on-status-with-frameworks_02.png)

  - Example on OS X: testrpc running on the left, and installing Truffle
    on the right\*

As you run the \`migrate\` command - which is what deploys your
contracts to the network - you can look at the window with \`testrpc\`
running, and you’ll see your transactions being published. If you get
\`Error: Invalid JSON RPC response\`, you probably forgot to run
\`testrpc\`.

Now we are ready to see our DApp running on Status. From within your
DApp directory, run:

\`\`\`shell

1.  Run your Javascript

npm run start

1.  (Remember to set ENV variable if working with real device)

`IP=`<DEVICE-IP>` npm run start`

\`\`\`

This should tell you that the the app is running, and that the DApp has
been added to the Status Contacts.

\!\[The DApp added to the default
Contacts\](images/starting-a-dapp-on-status-with-frameworks_03.png)

  - The DApp added to the default Contacts\*

Again, the DApp should appear in your chats screen, and navigating to it
should automatically open a browser that reveals all the goodies hidden
in this truffle box. It should also display the stored value of \`5\`
from the SimpleStorage contract that we deployed to \`testrpc\` by
running \`truffle migrate\` above.

If you would like to change the name that appears for your bot when it
gets added to Status, simply edit the \`package.json\` and update the
name there.

Once everything is running, leave it as is and move on directly to the
next truffle tutorial to see the power of the \`status-dev-cli watch\`
command.

  - Known problems and Notes:\*

1\) If you are running on a real iOS device, you need to configure
Truffle Box to use the network on your computer. In \`truffle.js\`,
change \`host\` to the IP of your computer:

\`\`\`js module.exports = {

` migrations_directory: "./migrations",`
` networks: {`
`   development: {`
`     host: "`<MACHINE-IP>`",`
`     port: 8546,`
`     network_id: "*" // Match any network id`
`   }`
` }`

}; \`\`\`

2\) You can always use \`localhost\` instead of \`<MACHINE-IP>\`, but
the application won't be accessible automatically, since it runs on port
3000 and your device/emulator knows nothing about it. Execute the
following to induce black magic and make the web application accessible:

\`\`\`shell adb reverse tcp:8546 tcp:8546 adb reverse tcp:3000 tcp:3000
\`\`\`

{{% /tab %}}

{{% tab Embark %}}

1.  1.  Embark

OK, so Truffle is not your favourite and you prefer using Embark. Fine
with us. As always, make sure you have installed the framework first.

\`\`\`shell npm -g install embark \`\`\`

Embark makes it super easy to set up a demo and get up and running fast:

\`\`\`shell embark demo cd embark_demo \`\`\`

Next we want to start the network. You can run a full Ethereum node with
\`embark blockchain\`, but for development you probably want to run
\`testrpc\`. We want it to run on port 8546 though, so it doesn't
interfere with Status, so we need to edit the \`blockchain.js\`:

\`\`\`shell nano config/blockchain.json

1.  Now edit the 'Development' Network and change only the rpcPort field
    to 8546:

"development": {

`   "enabled": true,`
`   "networkType": "custom",`
`   "genesisBlock": "config/development/genesis.json",`
`   "datadir": ".embark/development/datadir",`
`   "mineWhenNeeded": true,`
`   "nodiscover": true,`
`   "maxpeers": 0,`
`   "rpcHost": "`<MACHINE-IP>`",`
`   "rpcPort": 8546,`
`   "rpcCorsDomain": "`<http://><MACHINE-IP>`:8000",`
`   "account": {`
`     "password": "config/development/password"`
`   }`
` }`

1.  ctrl+O will write your changes to file, ctrl+X will exit the file.
2.  Now, navigate back to the root of your project and start testrpc

cd .. && embark simulator \`\`\`

Open a new shell tab and switch the RPC node. Once that is done, you
need to install the \`embark-status\` package.

\`\`\`shell status-dev-cli switch-node "<http://><MACHINE-IP>:8546" --ip
<DEVICE-IP> npm install embark-status --save \`\`\`

When using a real device, you will now need to insert the relevant IP
into two config files.

Open the file \`embark.json\` and edit the \`plugins\` key to reflects
your DEVICE's IP address:

\`\`\`js "plugins": {

` "embark-status": {`
`   "deviceIp": "`<DEVICE-IP>`",`
`   "whisperIdentity": "dapp-embark-test",`
`   "name": "MyEmbarkDapp"`
` }`

} \`\`\`

Navigate to and change the \`config/webserver.js\` file so that it
reflects your development machine's IP address:

\`\`\`js {

` "enabled": true,`
` "host": "`<MACHINE-IP>`",`
` "port": 8000`

} \`\`\`

Now we’re ready to run the DApp on Status. From within your DApp
directory, run:

\`\`\`shell embark run \`\`\`

The Embark console will appear within your shell with useful information
about the SimpleStorage contract it has created, compiled, and deployed.
Now just add your DApp, and have some fun\!

\`\`\`shell status-dev-cli add '{"whisper-identity": "embark",
"dapp-url": "<http://><MACHINE-IP>:8000/", "name": "Embark"}' --ip
<DEVICE-IP> \`\`\`

\!\[\](images/starting-a-dapp-on-status-with-frameworks_04.png)

\!\[The Embark simulator runs in one Terminal window on the left, and
the Embark console on the
right\](images/starting-a-dapp-on-status-with-frameworks_05.png)

  - The Embark simulator runs in one Terminal window on the top, and the
    Embark console on the bottom\*

<!-- end list -->

  - Known Issues and Notes:\*

1\) To deploy the DApp successfully on a device you may need to patch
\[this
line\](https://github.com/status-im/embark-status/blob/master/index.js\#L13)
in embark-status to include \`+ " --dapp-port 5561"\`.

2\) You can always use \`localhost\` instead of \`<MACHINE-IP>\`, but
the application won't be accessible automatically, since it runs on port
8000 and your device/emulator knows nothing about it. Execute the
following to induce black magic and make the web application accessible:

\`\`\`shell adb reverse tcp:8546 tcp:8546 adb reverse tcp:8000 tcp:8000
\`\`\`

{{% /tab %}}

{{% /tabs %}}

If you want to remove your DApp because you're unhappy with it for some
reason, just run \`status-dev-cli list --ip <DEVICE-IP>\` and then
\`status-dev-cli remove --ip <DEVICE-IP> \[my-dapp\]\` where \[my-dapp\]
is the \`whisper-identity\` returned by the \`list\` command.

Using Status, you can now develop mobile DApps as easily as developing
for MetaMask or Mist\! But Status offers extra goodies as well.

In particular, Status will help you allow your users to chat with your
DApp\! The chat interface will let your users easily and intuitively
accomplish tasks. In the future, your users will be able to hold group
conversations where all the other participants are DApps, which is kind
of amazing.

Later we’ll have an easy mechanism to make your DApp available for
others to use on Status, but for now please just submit a pull request
using our \[guide on adding
DApps\](http://wiki.status.im/contributing/development/adding-dapps/).

1.  1.  My First 1-1 Chatbot

{{% tabs DIY Embark Truffle %}} {{% tab DIY %}}

1.  1.  Do It Yourself

First, we're going to create a new \`bots\` directory and add file to
keep our javascript in.

\`\`\`shell cd ~/my-dapp mkdir bots && cd bots && touch bot.js \`\`\`

Instead of adding a new DApp, we can now include a \`bot-url\` parameter
in our call to \`status-dev-cli\`. The chatbot url targets a \`js\`
file, and this file will be loaded in the Otto VM shown in the anatomy.
Code in this file сan interact with the input field, messages and react
however we program it to.

Add the following code snippet to the newly-created \`bot.js\` file.

\`\`\`js status.addListener("on-message-send", function (params,
context) {

`   var result = {`
`           err: null,`
`           data: null,`
`           messages: []`
`       };`

`   try {`
`       result["text-message"] = "You're amazing, master!";`
`   } catch (e) {`
`       result.err = e;`
`   }`

`   return result;`

}); \`\`\`

Then, navigate back to the \`bots\` directory, do the necessary Android
steps if you are using that platform, and start the http server again.
Then, open a new shell window and add your new bot. It's important to
note a few things here. We are \`add\`ing a new new bot, rather than
just watching the DApp we built earlier for changes. Also, we are only
passing in a \`bot-url\`, rather than a \`dapp-url\` - this will ensure
that, when you open your new bot contact, it won't automatically launch
a browser a window.

\`\`\`shell

1.  make sure you're in my-dapp or the equivalent

cd ~/my-dapp

1.  start the server and then open a new shell window

http-server \`\`\`

\`\`\`shell status-dev-cli add '{"whisper-identity": "botler", "name":
"Botler" ,"bot-url": "<http://><MACHINE-IP>:8080/bots/bot.js"}' --ip
<DEVICE-IP> \`\`\`

This is pretty much the simplest responsive chatbot we can write, using
only the \`addListener\` function from the API. All it's listening for
is a message-send event, to which it will try to respond with the test
\`You're amazing, master\!\`.

Obviously, there's much more we can do than simply listen for messages
and send flattering responses. All will be revealed in the next
tutorial. If you're feeling impatient, you can find a full Demo Bot
\[here\](https://github.com/status-im/status-react/tree/34b77022f7926dbabbb0e8c5e8471eaea5ed812f/bots/demo_bot).

  - Known Issues and Notes:\*

1\) You can always use \`localhost\` instead of \`<MACHINE-IP>\`, but
the application won't be accessible automatically, since it runs on port
8080 and your device/emulator knows nothing about it. Execute the
following to induce black magic and make the web application accessible:

\`\`\`shell adb reverse tcp:8080 tcp:8080 \`\`\`

{{% /tab %}}

{{% tab Embark %}}

1.  1.  Embark

OK, so even though \`status-dev-cli\` is lightweight and awesome, the
frameworks offer some really cool features and make development
significantly easier for many projects. Let's see what it's like to add
the same chatbot to Status using Embark.

First, go back to the \`embark_demo/\` directory we created
\[earlier\](\#my-first-dapp), where we set up the correct configuration
for Embark and Status. Essentially, you just want to make sure - as with
the Truffle Box example - that you put the javascript file in the right
place so that you can reference it correctly.

Instead of adding a new DApp, we can now include a \`bot-url\` parameter
in our call to \`status-dev-cli\`. The chatbot url targets a \`js\`
file, and this file will be loaded in the Otto VM shown in the anatomy.
Code in this file сan interact with the input field, messages and react
however we program it to.

Let's create a bot.js in the \`/embark_demo/app/js/\` directory and put
the short snippet of javascript we need into it.

\`\`\`shell touch ~/embark_demo/app/js/bot.js nano
~/embark_demo/app/js/bot.js \`\`\`

\`\`\`js status.addListener("on-message-send", function (params,
context) {

`   var result = {`
`           err: null,`
`           data: null,`
`           messages: []`
`       };`

`   try {`
`       result["text-message"] = "You're amazing, master!";`
`   } catch (e) {`
`       result.err = e;`
`   }`

`   return result;`

}); \`\`\` This is pretty much the simplest responsive chatbot we can
write, using only the \`addListener\` function from the API. All it's
listening for is a message-send event, to which it will try to respond
with the test \`You're amazing, master\!\`.

Obviously, there's much more we can do than simply listen for messages
and send flattering responses. All will be revealed in the next
tutorial. If you're feeling impatient, you can find a full Demo Bot
\[here\](https://github.com/status-im/status-react/tree/34b77022f7926dbabbb0e8c5e8471eaea5ed812f/bots/demo_bot).

We now need to update the \`config/blockchain.json\` so it copies over
our bot into a public directory (see the last line inside app
object).

\`\`\`js

` "contracts": ["app/contracts/**"],`
` "app": {`
`   "css/app.css": ["app/css/**"],`
`   "images/": ["app/images/**"],`
`   "js/app.js": ["embark.js", "app/js/_vendor/jquery.min.js", "app/js/_vendor/bootstrap.min.js", "app/js/**"],`
`   "index.html": "app/index.html",`
`   "js/bot.js": "app/js/bot.js"   # Add this line in #`
` },`
` "buildDir": "dist/",`

\`\`\`

That should serve the \`js\` files you need, so the last thing you'll
need to do is add this bot to Status as per usual.

\`\`\`shell status-dev-cli add '{"whisper-identity":"embark-demo-bot",
"name":"Embark-Demo-Bot",
"bot-url":"<http://><MACHINE-IP>:8000/js/bot.js"}' --ip <DEVICE-IP>
\`\`\`

Again, you could always use \`localhost\` instead if you run into
issues, and just make sure to run \`adb reverse tcp:8000 tcp:8000\`.

{{% /tab %}}

{{% tab Truffle
%}}

1.  1.  Truffle

``Let's see what it looks like to add the same, flattering little chatbot to Status through the Truffle Box we set up earlier. We assume here that you still have `testrpc` running from the previous tutorial and that you are still running the `npm start` script. If not, navigate back to that directory, ensure that `testrpc` is switched on and that you have opened all the right ports if you're on Android.``

`> Only if you have switched off everything from the previous tutorial.`

\`\`\`shell testrpc -p 8546

status-dev-cli switch-node "<http://><MACINE-IP>:8546" --ip <DEVICE-IP>

cd ~/truffle-box-status

1.  If you want to be extra sure your contracts are there, run:

truffle migrate --reset \`\`\`

So, all we need to do here is add a \`bot\` directory in Truffle's
\`public\` directory (i.e. the one accessible from the web) and write
some simple javascript and - hey presto\! - we'll have a decentralized
chatbot. Copy the code provided into another \`bot.js\` file. The
\`bot-url\` targets a \`js\` file, and this file will be loaded in the
Otto VM shown in the anatomy. Code in this file сan interact with the
input field, messages and react however we program it to.

\`\`\`shell cd ~/truffle-box-status/public/ && mkdir bot/ touch
bot/bot.js nano bot/bot.js \`\`\`

\`\`\`js status.addListener("on-message-send", function (params,
context) {

`   var result = {`
`           err: null,`
`           data: null,`
`           messages: []`
`       };`

`   try {`
`       result["text-message"] = "You're amazing, master!";`
`   } catch (e) {`
`       result.err = e;`
`   }`

`   return result;`

}); \`\`\`

If you look on line 42-43 of the \`npm start\` script, you'll see that
we already passed in a \`bot-url\` when adding the Truffle Box in the
previous tutorial. So, all we need to do now is \`status-dev-cli watch\`
that same DApp to activate the live-reloading and start chatting to our
bot through the chat, or interacting with our contracts through the
webView. However, we need to know what to tell \`status-dev-cli\` to
watch for, so let's try \`list\` our DApps currently in Status.

\`\`\`shell status-dev-cli list --ip <DEVICE-IP>

1.  dapp-0x74727566666c652d626f782d737461747573 (Name:
    "truffle-box-status", DApp URL: "<http://><MACINE-IP>:3000")

\`\`\`

The first value returned there is the DApp's \`whisper-identity\`, which
is what we need to pass to the \`watch\` command to tell it what to look
for. Using it, we can now tell \`status-dev-cli\` to watch for our
changes and go find that brand new bot.

\`\`\`shell

1.  Make sure you're in the truflle-box-status/ directory as that is
    what $PWD refers to below

cd ~/truffle-box-status

1.  I would use dapp-0x74727566666c652d626f782d737461747573 for
    <whisper-identity> below, Substitute your own one in.

status-dev-cli watch $PWD "<whisper-identity>" --ip <DEVICE-IP> \`\`\`

And you're away\! You should be able to see your DApp, browse to the
same site as before, and chat with the repetitively flattering greeter
bot.

{{% /tab %}}

{{% /tabs %}}

Once you have worked through the first tutorials and understood the
basic steps to building a DApp and adding it into Status, it's time to
get our hands a little more dirty by actually writing a simple,
one-response chatbot that will begin to utilise the awesome power of the
Status API.

We kind of cheated a little in the previous tutorials. While it is
totally possible to have your html5 DApp work perfectly in Status via
\`webView\`, it's obviously not the most optimal way to do things. There
are essentailly two different ways for developers to interact with
Status, best illustrated by the Status Anatomy below:

\!\[status-anatomy.png\](images/status-anatomy.png)

The main take away here is that the chat context itself is actually an
Otto VM jail that executes the javascript you write, and then integrates
that directly with Status. So, we can actually write bots that are
purely javascript-based. Please see
\[here\](https://github.com/status-im/status-react/tree/develop/bots)
for a full list of all our current bots and their source code.

1.  1.  My First Status Command

{{% tabs DIY Embark Truffle %}}

{{% tab DIY %}}

1.  1.  Do It Yourself with \`status-dev-cli\`

So, we have set up Status in \`debug\` mode, added our DApp to it
(hopefully run the \`status-dev-cli watch\` command to get some
live-reloading going) and learned how to start a conversation with a
simple javascript chatbot. Now it's time to start using the API proper
and start using the provided commands to interact with our users and
help them out.

Navigate back to your \`bots\` directory and open the \`bot.js\` file
where we put the \`status.addListener\` function in the previous
tutorials. We will again pass this fils in as a \`bot-url\` parameter to
\`status-dev-cli\`, which will then execute the code in an Otto VM jail.

\`\`\`shell cd ~/my-dapp/bots/ && nano bot.js

1.  or use 'vi bot.js' if that's your preference

\`\`\`

Then, open the file in the text editor of your choice and add in the
code provided. All we want to do is provide our user with a command,
called \`hello\` that they can issue into the chat and we can respond
to. In order to achieve this effect, we set up a simple
\`status.command()\` and pass in a name, title and description so as to
identify our command and display it to the user, along with some helpful
information about what it does.

\`\`\`js status.command({

`    name: "hello",`
`    title: "HelloBot",`
`    description: "Helps you say hello",`
`    color: "#CCCCCC",`
`    preview: function (params) {`
`            var text = status.components.text(`
`                {`
`                    style: {`
`                        marginTop: 5,`
`                        marginHorizontal: 0,`
`                        fontSize: 14,`
`                        fontFamily: "font",`
`                        color: "black"`
`                    }`
`                }, "Hello from the other side!");`

`            return {markup: status.components.view({}, [text])};`
`        }`
`});`

\`\`\`

We then set a color for the command to appear in set up our preview
function. You can read about exactly what the preview function can
achieve in the formal API specififcation but, in a nutshell, the preview
defines what the user will see returned in the markup - i.e. where the
messages appear.

Here, we are creating a simple text response by setting up a variable
that we pass to \`status.components.text\`, which the perceptive will
notice is a React Native component - a whole bunch more of which are
available and detailed in the formal specification. Beneath the text, we
are creating a standard response of "Hello from the other side\!", all
of which will be returned as json to the markup. Note again the use of
another React Native component - \`status.components.view\`.

Go ahead and serve your dapp again:

\`\`\`shell http-server

status-dev-cli list --ip <DEVICE-IP>

status-dev-cli watch $PWD "<whisper-identity>" --ip <DEVICE-IP> \`\`\`

And there you go - we are now capable of greeting and interacting with
our bot in two ways\! You should be able to see your DApp, navigate to
it, tap the new \`/hello\` command you see above the text input field
and see your new Dapp respond.

{{% /tab %}}

{{% tab Embark %}}

1.  1.  Embark

\`\`\`shell cd ~/embark_demo/app/js/bot.js && nano bot.js \`\`\` Here,
we are going to create a simple text response by setting up a variable
that we pass to \`status.components.text\`, which the perceptive will
notice is a React Native component - a whole bunch more of which are
available and detailed in the formal specification. Beneath the text, we
are creating a standard response of "Hello from the other side\!", all
of which will be returned as json to the markup. Note again the use of
another React Native component - \`status.components.view\`.

\`\`\`js status.command({

`    name: "hello",`
`    title: "HelloBot",`
`    description: "Helps you say hello",`
`    color: "#CCCCCC",`
`    preview: function (params) {`
`            var text = status.components.text(`
`                {`
`                    style: {`
`                        marginTop: 5,`
`                        marginHorizontal: 0,`
`                        fontSize: 14,`
`                        fontFamily: "font",`
`                        color: "black"`
`                    }`
`                }, "Hello from the other side!");`

`            return {markup: status.components.view({}, [text])};`
`        }`
`});`

\`\`\` Go ahead and \`run\` your Embark application again and watch for
changes to your new, interactive bot. We have already made sure that the
\`js\` is built to, and served in, the right place, so we should just be
able to list the DApps and bots currently in Status and just watch the
"embark-demo-bot" we set up earlier.

\`\`\`shell

1.  Only if using an Android device

adb reverse tcp:8000 tcp:8000

embark run

status-dev-cli list --ip <DEVICE-IP>

status-dev-cli watch $PWD "<whisper-identity>" --ip \`\`\`

And there you go - we are now capable of greeting and interacting with
our bot in two ways\! You should be able to see your DApp, navigate to
it, tap the new \`/hello\` command you see above the text input field
and see your new Dapp respond.

{{% /tab %}}

{{% tab Truffle %}}

1.  1.  Truffle

\`\`\`shell cd ~/truffle-box-status/public/bot/ && nano bot.js \`\`\`

Navigate back to the truffle box \`public\` directory and open the
\`bot.js\` file we previously created so as to add the code provided and
display it through truffle.

\`\`\`js status.command({

`    name: "hello",`
`    title: "HelloBot",`
`    description: "Helps you say hello",`
`    color: "#CCCCCC",`
`    preview: function (params) {`
`            var text = status.components.text(`
`                {`
`                    style: {`
`                        marginTop: 5,`
`                        marginHorizontal: 0,`
`                        fontSize: 14,`
`                        fontFamily: "font",`
`                        color: "black"`
`                    }`
`                }, "Hello from the other side!");`
`            return {markup: status.components.view({}, [text])};`
`        }`
`});`

\`\`\`

Much like we did last time, just find the \`whisper-identity\` of your
DApp and \`watch\` for changes.

\`\`\`shell status-dev-cli list --ip <DEVICE-IP>

1.  dapp-0x74727566666c652d626f782d737461747573 (Name:
    "truffle-box-status", DApp URL: "<http://><MACHINE-IP>:3000")

<!-- end list -->

1.  Make sure you're in the truflle-box-status/ directory as that is
    what $PWD refers to below

cd ~/truffle-box-status

1.  I would use dapp-0x74727566666c652d626f782d737461747573 for
    <whisper-identity> below, Substitute your own one in.

status-dev-cli watch $PWD "<whisper-identity>" --ip <DEVICE-IP> \`\`\`

{{% /tab %}}

{{% /tabs %}}

We can now write a 1-1 chatbot that responds when users send messages to
it using the \`status.addListener\` method provided by the API, but what
if we want to provide our users with a command they can interact with,
rather than just waiting for them to send us some kind of message, or
take some other action?

We need to use the \`status.command()\` method. As usual, we can do this
ourselves through the barebones \`status-dev-cli\`, or through the
popular frameworks currently available on Ethereum.

1.  1.  My First Interactive Suggestions Area

Let's go ahead and make that \`hello\` command we just created a little
bit more clever.

\`\`\`shell cd ~/my-dapp/bots/ && nano hello.js \`\`\` Instead of the
preview parameter we used last time, let's build another
\`status.command()\` and add in a \`params\` option with a
\`suggestions\` object in it. This will create a suggestions area which
- if you refer back to the \[Chat Anatomy\](\#overview) - is what rolls
up above the keyboard, such as you see when turning on the debugging
server with the options \`On\` and \`Off\`.

\`\`\`js status.command({

`    name: "greet",`
`    title: "Greeter",`
`    description: "Helps you choose greetings",`
`    color: "#0000ff",`
`    params: [{`
`             name: "greet",`
`             type: status.types.TEXT,`
`             suggestions: helloSuggestions`
`            }]`
`})`

\`\`\`

"But wait\!" we hear you cry, "what on earth is that
\`helloSuggestions\` thing about?\!". Well, let’s make a function to
explain. This will return a \`scrollView\` with two suggestions:
"Hello", and "Goodbye". Don’t get intimidated by the length, there’s
actually not much to it.

First, we set up the helpers that will style the suggestion as it
appears in the suggestions area.

\`\`\`js function suggestionsContainerStyle(suggestionsCount) {

`   return {`
`       marginVertical: 1,`
`       marginHorizontal: 0,`
`       keyboardShouldPersistTaps: "always",`
`       height: Math.min(150, (56 * suggestionsCount)),`
`       backgroundColor: "white",`
`       borderRadius: 5,`
`       flexGrow: 1`
`   };`

} var suggestionSubContainerStyle = {

`   height: 56,`
`   borderBottomWidth: 1,`
`   borderBottomColor: "#0000001f"`

};

var valueStyle = {

`   marginTop: 9,`
`   fontSize: 14,`
`   fontFamily: "font",`
`   color: "#000000de"`

}; \`\`\`

Now, we need to write the main star of our show - that mysterious
\`helloSuggestions\` function we passed into the \`params\` of our
\`status.command()\`. The objective here is to return two touchable
buttons - one for \`Hello\` and the other for \`Goodbye\`. We are making
quite extensive use of React native Components here, so we expect at
least some level of familiarity with the framework in order to get the
most out of these tutorials.

\`\`\`js function helloSuggestions()
{

`   var suggestions = ["Hello", "Goodbye"].map(function(entry) {`
`       return status.components.touchable(`
`           {onPress: status.components.dispatch([status.events.SET_VALUE, entry])},`
`           status.components.view(`
`               suggestionsContainerStyle,`
`               [status.components.view(`
`                   suggestionSubContainerStyle,`
`                   [`
`                       status.components.text(`
`                           {style: valueStyle},`
`                           entry`
`                       )`
`                   ]`
`               )]`
`           )`
`       );`
`   });`

`   // Let's wrap those two touchable buttons in a scrollView`
`   var view = status.components.scrollView(`
`       suggestionsContainerStyle(2),`
`       suggestions`
`   );`

`   // Give back the whole thing inside an object.`
`   return {markup: view};`

} \`\`\`

The main point of this example is that your \`suggestions\` parameter
should accept users’ input, and then return a component to be rendered.

We have already added this contact to Status, so let's se if we can set
up our environment to do some live reloading so that we don't have to
revert to the cli every time we want to change something in the process
of building all the new things.

\`\`\`shell http-server

status-dev-cli watch $PWD "<whisper-identity>" --ip <DEVICE-IP> \`\`\`

You can also work with the \`suggestionsTrigger\` parameter. Now that
we’ve covered \`params\` and the possibility of \`suggestions\`, it’s
easy to see that \`suggestionsTrigger\` will take a string corresponding
to an event that, when triggered, will show all your \`suggestions\`. If
you don’t include this parameter, the default is "on-change", so your
suggestions will show when your users selects the command.

It's also worth knowing about the \`fullscreen\` option. If your command
has \`suggestions\`, this \`param\` controls whether that list of
suggestions expands to fill the entire screen. If your command has a lot
of suggestions, you might want to set \`fullscreen\` to \`true\`, so
that your users don’t have to pull the list upwards. On the other hand,
if your command has only a few suggestions and you set \`fullscreen\` to
\`true\`, your users will have to pull the list downwards to keep it
from hiding the screen. Choose whichever will be most convenient to your
users, considering your command’s suggestions.

OK, so we can build up a basic command, show it to the user and have it
do something (like return text) when the user taps it. That's great, but
what happens if we want to be a bit more dynamic and interactive and
suggest to our users a range of options to choose from when issuing the
command? Let's take a look...

For another full example of an interactive suggestion area, please take
a look over our \[Demo
Bot\](https://github.com/status-im/status-react/blob/develop/bots/demo_bot/bot.js),
which makes prominent use of the \`defineSubscription\` method from the
API and may be helpful for those looking to work with things like the
\`status.component.slider\` React Native Component.

You could also build a location bot following a similar pattern. We
provide the code for you, just because we can. See if you can get it all
working right this time...

\`\`\`js I18n.translations =
{

`   en: {`
`       location_title: 'Location',`
`       location_suggestions_title: 'Send location',`
`       location_description: 'Share your location',`
`       location_address: 'address'`

}}

status.command({

`   name: "location",`
`   title: I18n.t('location_title'),`
`   description: I18n.t('location_description')`
`   hideSendButton: true,`
`   sequentialParams: true,`
`   params: [{`
`       name: "address",`
`       type: status.types.TEXT,`
`       placeholder: I18n.t('location_address')`
`   }]`
`})`
` ``` `

`Or, even better, can you get some location suggestions to display?`

` ```js`
`function locationsSuggestions (params) {`
`   var result = {title: "Send location"};`
`   var seqArg = params.seqArg ? params.seqArg : "";`

`   if (seqArg == "Dropped pin")`
`   {`
`       result.markup = ["view", {}, ["text", {}, "Dropped pin" + seqArg]];`
`   }`
`   else if (seqArg != "")`
`   {`
`       result.markup = ["view", {}, ["text", {}, "Let's try to find something " + seqArg]];`
`   }`
`   else`
`   {`
`       result.markup = ['current-location', {showMap: true}];`
`   }`

`   return result;`

} \`\`\`

1.  1.  Frequently Asked Questions (FAQ)

<!-- end list -->

1.  1.  My bot has been deployed but it does not appear to work.

Make sure that the \`bot-url\` you provided when adding your bot is
accessible from your device. For example, open it in mobile browser.

1.  1.  Is there a way to debug my bot?

\`status-dev-cli log\` allows to access outputs from \`console.log()\`.
So, just add in the necessary \`console.log()\`s and read them in your
terminal. The team is working actively to make this easier.

1.  1.  Can I connect to a different RPC server?

\`status-dev-cli switch-node\` only works for \`dapp-url\`s at the
moment. Due to the restrictions of the Otto VM jail that the bots \`js\`
is run in, it still connects by default to Ropsten and your Status
account. The team is actively working on ways around this.

1.  1.  Can custom commands be added to normal chat?

Currently they are only available to 1-1 chat with bots. At some stage
in the future we will implement group chats and all your dreams will
come true.

1.  1.  Status API FAQs

<!-- end list -->

1.  1.  How can I set the text input value?

You can use \`status.components.dispatch\` in tandem with the
\`SET_VALUE\` object. For instance, if you wanted to sugeest a url, you
would do the following:
\`status.components.dispatch(\[status.events.SET_VALUE, "@browse
url"\])}\`. A more complete example is also given alongside.

\`\`\`javascript return {markup:
status.components.touchable(

`           {onPress: status.components.dispatch([status.events.SET_VALUE, "@browse url"])},`
`           status.components.view( {},`
`                       status.components.text(`
`                           {},`
`                           "Click me"`
`                       )`
`           )`
`       )};`

\`\`\`

1.  1.  How can I send a command a display result back?

Here we use the \`onSend\` function to direct a user to a website based
on their initial input.

\`\`\`javascript
status.command({

`   name: "google_or_yahoo",`
`   title: 'TITLE',`
`   description: 'DESCRIPTION',`
`   fullscreen: true,`
`   params: [{`
`       name: "option",`
`       type: status.types.TEXT,`
`       placeholder: "either type G or Y"`
`   }],`
`   onSend: function(params){`
`     console.log('***', params);`
`     if (params.option == 'g') {`
`       url = '`<http://www.google.com>`'`
`     }else{`
`       url = '`<http://www.yahoo.com>`'`
`     }`
`     return {`
`             title: "Browser",`
`             dynamicTitle: true,`
`             singleLineInput: true,`
`             actions: [ { type: status.actions.FULLSCREEN } ],`
`             markup: status.components.bridgedWebView("`<http://google.com/>`")`
`     };`
`   }`

}); \`\`\`

1.  1.  Can I send a message asynchrounously after a request has been
        sent?

Use the latest nightlies and \`status.sendMessage\` will now be
available.

\`\`\`javascript

`status.command({`
`   name: "test",`
`   icon: "test",`
`   title: "test",`
`   description: "test",`
`   color: "#a187d5",`
`   sequentialParams: true,`
`   params: [{`
`       name: "address",`
`       type: status.types.TEXT,`
`       placeholder: "address"`
`   }],`
`   handler: function (params) {`
`       status.sendMessage("Address " + params.address);`
`   }`

}); \`\`\`

1.  1.  How can I add an image to my reactive component?

It's easy enough to achieve this by using \`status.components.image\`.

\`\`\`js status.components.image({style: {width: 50, height: 50},
source: {uri: '<https://facebook.github.io/react/img/logo_og.png>'}}),
\`\`\`

1.  1.  JS and web3 API FAQ

<!-- end list -->

1.  1.  How can data be stored and retrieved?

Yes it can be, using \`localStorage\`.

\`\`\`js status.addListener("on-message-send", function (params,
context)
{

`   var cnt = localStorage.getItem("cnt");`
`   if(!cnt) {`
`       cnt = 0;`
`   }`

`   cnt++;`

`   localStorage.setItem("cnt", cnt);`
`   if (isNaN(params.message)) {`
`       return {"text-message": "Seems that you don't want to send money :(. cnt = " + cnt};`
`   }`

`   var balance = web3.eth.getBalance(context.from);`
`   var value = parseFloat(params.message);`
`   var weiValue = web3.toWei(value, "ether");`
`   if (bn(weiValue).greaterThan(bn(balance))) {`
`       return {"text-message": "No way man, you don't have enough money! :)"};`
`   }`
`   web3.eth.sendTransaction({`
`       from: context.from,`
`       to: context.from,`
`       value: weiValue`
`   }, function (error, hash) {`
`       if (error) {`
`           status.sendMessage("Something went wrong, try again :(");`
`           status.showSuggestions(demoSuggestions(params, context).markup);`
`       } else {`
`           status.sendMessage("You are the hero, you sent " + value + " ETH to yourself!")`
`       }`
`   });`

}); \`\`\`

1.  1.  Can I interract with \`web3\` object?

Yes\! The snippet provided showcases both \`localStrage\` and the
\`web3\` object operating in tandem.

1.  1.  Can I parse JSON data?

Yes JSON.parse()is supported.

1.  1.  Is XMLHttpRequest supported?

Not yet.

1.  1.  Any examples
    ?

<!-- end list -->

  - <https://github.com/status-im/status-react/blob/develop/bots/demo_bot/bot.js>
  - <https://github.com/status-im/status-react/blob/develop/bots/console/bot.js>

\--- weight: 40 title: Web3 API Reference ---

1.  web3 (API)

To make your Ðapp work on Ethereum, you can use the \`web3\` object
provided by the \[web3.js
library\](https://github.com/ethereum/web3.js). Under the hood it
communicates to a local node through \[RPC
calls\](https://github.com/ethereum/wiki/wiki/JSON-RPC). web3.js works
with any Ethereum node, which exposes an RPC layer.

\`web3\` contains the \`eth\` object - \`web3.eth\` (for specifically
Ethereum blockchain interactions) and the \`shh\` object - \`web3.shh\`
(for Whisper interaction). Over time we'll introduce other objects for
each of the other web3 protocols. Working \[examples can be found
here\](https://github.com/ethereum/web3.js/tree/master/example).

If you want to look at some more sophisticated examples using web3.js
check out these \[useful Ðapp
patterns\](https://github.com/ethereum/wiki/wiki/Useful-Ðapp-Patterns).

1.  1.  Getting Started

<aside class="notice">

Status already provides the `web3` object in both HTML5 DApps and
Chatbots, so you can safely skip this step.

</aside>

TODO check how metamask suggests to embed

\`\`\`js if (typeof web3 \!== 'undefined') {

` web3 = new Web3(web3.currentProvider);`

} else
{

` // set the provider you want from Web3.providers`
` web3 = new Web3(new Web3.providers.HttpProvider("`<http://localhost:8545>`"));`

} \`\`\`

First you need to get web3.js into your project. This can be done using
the following methods:

\- npm: \`npm install web3\` - bower: \`bower install web3\` - meteor:
\`meteor add ethereum:web3\` - vanilla: link the \`dist./web3.min.js\`

Then you need to create a web3 instance, setting a provider. To make
sure you don't overwrite the already set provider when in mist, check
first if the web3 is available:

After that you can use the API of the \`web3\` object.

1.  1.  Using callbacks

\`\`\`js web3.eth.getBlock(48, function(error, result){

`   if(!error)`
`       console.log(result)`
`   else`
`       console.error(error);`

}) \`\`\`

As this API is designed to work with a local RPC node, all its functions
use synchronous HTTP requests by default.

If you want to make an asynchronous request, you can pass an optional
callback as the last parameter to most functions. All callbacks are
using an \[error first
callback\](http://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js/)
style:

1.  1.  Batch requests

\`\`\`js var batch = web3.createBatch();
batch.add(web3.eth.getBalance.request('0x0000000000000000000000000000000000000000',
'latest', callback));
batch.add(web3.eth.contract(abi).at(address).balance.request(address,
callback2)); batch.execute(); \`\`\`

Batch requests allow queuing up requests and processing them at once.

  -   - Note\*\* Batch requests are not faster\! In fact making many
        requests at once will in some cases be faster, as requests are
        processed asynchronous. Batch requests are mainly useful to
        ensure the serial processing of requests.

<!-- end list -->

1.  1.  web3 & BigNumber

\`\`\`js "101010100324325345346456456456456456456" //
"101010100324325345346456456456456456456"
101010100324325345346456456456456456456 // 1.0101010032432535e+38 \`\`\`

You will always get a BigNumber object for number values as JavaScript
is not able to handle big numbers correctly. Look at the following
examples:

web3.js depends on the \[BigNumber
Library\](https://github.com/MikeMcl/bignumber.js/) and adds it
automatically.

\`\`\`js var balance = new
BigNumber('131242344353464564564574574567456'); // or var balance =
web3.eth.getBalance(someAddress);

// toString(10) converts it to a number string
balance.plus(21).toString(10); // "131242344353464564564574574567477"
\`\`\`

The next example wouldn't work as we have more than 20 floating points,
therefore it is recommended to keep you balance always in \*wei\* and
only transform it to other units when presenting to the user: \`\`\`js
var balance = new BigNumber('13124.234435346456466666457455567456');

// toString(10) converts it to a number string, but can only show max 20
floating points balance.plus(21).toString(10); //
"13145.23443534645646666646" // you number would be cut after the 20
floating point \`\`\`

1.  1.  web3

<!-- end list -->

1.  1.  web3.version.api

<!-- end list -->

1.  1.  Returns

\`String\` - The ethereum js api version.

\`\`\`js var version = web3.version.api; console.log(version); //
"0.2.0" \`\`\`

1.  1.  web3.version.node

`   web3.version.node`
`   // or async`
`   web3.version.getNode(callback(error, result){ ... })`

1.  1.  Returns

\`String\` - The client/node version.

\`\`\`js var version = web3.version.node; console.log(version); //
"Mist/v0.9.3/darwin/go1.4.1" \`\`\`

1.  1.  web3.version.network

`   web3.version.network`
`   // or async`
`   web3.version.getNetwork(callback(error, result){ ... })`

1.  1.  Returns

\`String\` - The network protocol version.

\`\`\`js var version = web3.version.network; console.log(version); // 54
\`\`\`

1.  1.  web3.version.ethereum

`   web3.version.ethereum`
`   // or async`
`   web3.version.getEthereum(callback(error, result){ ... })`

1.  1.  Returns

\`String\` - The ethereum protocol version.

\`\`\`js var version = web3.version.ethereum; console.log(version); //
60 \`\`\`

1.  1.  web3.version.whisper

`   web3.version.whisper`
`   // or async`
`   web3.version.getWhisper(callback(error, result){ ... })`

1.  1.  Returns

\`String\` - The whisper protocol version.

\`\`\`js var version = web3.version.whisper; console.log(version); // 20
\`\`\`

  -   -
<!-- end list -->

1.  1.  web3.isConnected

`   web3.isConnected()`

Should be called to check if a connection to a node exists

1.  1.  Parameters

none

1.  1.  Returns

\`Boolean\`

\`\`\`js if(\!web3.isConnected()) {

`  // show some dialog to ask the user to start a node`

} else {

`  // start web3 filters, calls, etc`
` `

} \`\`\`

1.  1.  web3.setProvider

`   web3.setProvider(provider)`

Should be called to set provider.

1.  1.  Parameters

none

1.  1.  Returns

\`undefined\`

\`\`\`js web3.setProvider(new
web3.providers.HttpProvider('<http://localhost:8545>')); // 8080 for
cpp/AZ, 8545 for go/mist \`\`\`

1.  1.  web3.currentProvider

`   web3.currentProvider`

Will contain the current provider, if one is set. This can be used to
check if mist etc. set already a provider.

1.  1.  Returns

\`Object\` - The provider set or \`null\`;

\`\`\`js // Check if mist etc. already set a provider
if(\!web3.currentProvider)

`   web3.setProvider(new web3.providers.HttpProvider("`<http://localhost:8545>`"));`

\`\`\`

1.  1.  web3.reset

`   web3.reset(keepIsSyncing)`

Should be called to reset state of web3. Resets everything except
manager. Uninstalls all filters. Stops polling.

1.  1.  Parameters

1\. \`Boolean\` - If \`true\` it will uninstall all filters, but will
keep the \[web3.eth.isSyncing()\](\#web3ethissyncing) polls

1.  1.  Returns

\`undefined\`

\`\`\`js web3.reset(); \`\`\`

1.  1.  web3.sha3

`   web3.sha3(string, options)`

1.  1.  Parameters

1\. \`String\` - The string to hash using the Keccak-256 SHA3 algorithm
1. \`Object\` - Set \`encoding\` to \`hex\` if the string to hash is
encoded in hex. A leading \`0x\` will be automatically ignored.

1.  1.  Returns

\`String\` - The Keccak-256 SHA3 of the given data.

\`\`\`js var hash = web3.sha3("Some string to be hashed");
console.log(hash); //
"0xed973b234cf2238052c9ac87072c71bcf33abc1bbd721018e0cca448ef79b379"

var hashOfHash = web3.sha3(hash, {encoding: 'hex'});
console.log(hashOfHash); //
"0x85dd39c91a64167ba20732b228251e67caed1462d4bcf036af88dc6856d0fdcc"
\`\`\`

1.  1.  web3.toHex

`   web3.toHex(mixed);`

Converts any value into HEX.

1.  1.  Parameters

1\. \`String|Number|Object|Array|BigNumber\` - The value to parse to
HEX. If its an object or array it will be \`JSON.stringify\` first. If
its a BigNumber it will make it the HEX value of a number.

1.  1.  Returns

\`String\` - The hex string of \`mixed\`.

\`\`\`js var str = web3.toHex({test: 'test'}); console.log(str); //
'0x7b2274657374223a2274657374227d' \`\`\`

1.  1.  web3.toAscii

`   web3.toAscii(hexString);`

Converts a HEX string into a ASCII string.

1.  1.  Parameters

1\. \`String\` - A HEX string to be converted to ascii.

1.  1.  Returns

\`String\` - An ASCII string made from the given \`hexString\`.

\`\`\`js var str =
web3.toAscii("0x657468657265756d000000000000000000000000000000000000000000000000");
console.log(str); // "ethereum" \`\`\`

1.  1.  web3.fromAscii

`   web3.fromAscii(string [, padding]);`

Converts any ASCII string to a HEX string.

1.  1.  Parameters

1\. \`String\` - An ASCII string to be converted to HEX. 2. \`Number\` -
The number of bytes the returned HEX string should have.

1.  1.  Returns

\`String\` - The converted HEX string.

\`\`\`js var str = web3.fromAscii('ethereum'); console.log(str); //
"0x657468657265756d"

var str2 = web3.fromAscii('ethereum', 32); console.log(str2); //
"0x657468657265756d000000000000000000000000000000000000000000000000"
\`\`\`

1.  1.  web3.toDecimal

`   web3.toDecimal(hexString);`

Converts a HEX string to its number representation.

1.  1.  Parameters

1\. \`String\` - An HEX string to be converted to a number.

1.  1.  Returns

\`Number\` - The number representing the data \`hexString\`.

\`\`\`js var number = web3.toDecimal('0x15'); console.log(number); // 21
\`\`\`

1.  1.  web3.fromDecimal

`   web3.fromDecimal(number);`

Converts a number or number string to its HEX representation.

1.  1.  Parameters

1\. \`Number|String\` - A number to be converted to a HEX string.

1.  1.  Returns

\`String\` - The HEX string representing of the given \`number\`.

\`\`\`js var value = web3.fromDecimal('21'); console.log(value); //
"0x15" \`\`\`

1.  1.  web3.fromWei

`   web3.fromWei(number, unit)`

Converts a number of wei into the following ethereum units:

\- \`kwei\`/\`ada\` - \`mwei\`/\`babbage\` - \`gwei\`/\`shannon\` -
\`szabo\` - \`finney\` - \`ether\` - \`kether\`/\`grand\`/\`einstein\` -
\`mether\` - \`gether\` - \`tether\`

1.  1.  Parameters

1\. \`Number|String|BigNumber\` - A number or BigNumber instance. 2.
\`String\` - One of the above ether units.

1.  1.  Returns

\`String|BigNumber\` - Either a number string, or a BigNumber instance,
depending on the given \`number\` parameter.

\`\`\`js var value = web3.fromWei('21000000000000', 'finney');
console.log(value); // "0.021" \`\`\`

1.  1.  web3.toWei

`   web3.toWei(number, unit)`

Converts an ethereum unit into wei. Possible units are:

\- \`kwei\`/\`ada\` - \`mwei\`/\`babbage\` - \`gwei\`/\`shannon\` -
\`szabo\` - \`finney\` - \`ether\` - \`kether\`/\`grand\`/\`einstein\` -
\`mether\` - \`gether\` - \`tether\`

1.  1.  Parameters

1\. \`Number|String|BigNumber\` - A number or BigNumber instance. 2.
\`String\` - One of the above ether units.

1.  1.  Returns

\`String|BigNumber\` - Either a number string, or a BigNumber instance,
depending on the given \`number\` parameter.

\`\`\`js var value = web3.toWei('1', 'ether'); console.log(value); //
"1000000000000000000" \`\`\`

1.  1.  web3.toBigNumber

`   web3.toBigNumber(numberOrHexString);`

Converts a given number into a BigNumber instance.

See the \[note on BigNumber\](\#a-note-on-big-numbers-in-web3js).

1.  1.  Parameters

1\. \`Number|String\` - A number, number string or HEX string of a
number.

1.  1.  Returns

\`BigNumber\` - A BigNumber instance representing the given value.

\`\`\`js var value = web3.toBigNumber('200000000000000000000001');
console.log(value); // instanceOf BigNumber
console.log(value.toNumber()); // 2.0000000000000002e+23
console.log(value.toString(10)); // '200000000000000000000001' \`\`\`

  -   -
<!-- end list -->

1.  1.  web3.net

<!-- end list -->

1.  1.  web3.net.listening

`   web3.net.listening`
`   // or async`
`   web3.net.getListening(callback(error, result){ ... })`

This property is read only and says whether the node is actively
listening for network connections or not.

1.  1.  Returns

\`Boolean\` - \`true\` if the client is actively listening for network
connections, otherwise \`false\`.

\`\`\`js var listening = web3.net.listening; console.log(listening); //
true of false \`\`\`

1.  1.  web3.net.peerCount

`   web3.net.peerCount`
`   // or async`
`   web3.net.getPeerCount(callback(error, result){ ... })`

This property is read only and returns the number of connected peers.

1.  1.  Returns

\`Number\` - The number of peers currently connected to the client.

\`\`\`js var peerCount = web3.net.peerCount; console.log(peerCount); //
4 \`\`\`

  -   -
<!-- end list -->

1.  1.  web3.eth

Contains the ethereum blockchain related methods.

\`\`\`js var eth = web3.eth; \`\`\`

1.  1.  web3.eth.defaultAccount

`   web3.eth.defaultAccount`

This default address is used for the following methods (optionally you
can overwrite it by specifying the \`from\` property):

\- \[web3.eth.sendTransaction()\](\#web3ethsendtransaction) -
\[web3.eth.call()\](\#web3ethcall)

1.  1.  Values

\`String\`, 20 Bytes - Any address you own, or where you have the
private key for.

  - Default is\* \`undefined\`.

<!-- end list -->

1.  1.  Returns

\`String\`, 20 Bytes - The currently set default address.

\`\`\`js var defaultAccount = web3.eth.defaultAccount;
console.log(defaultAccount); // ''

// set the default account web3.eth.defaultAccount =
'0x8888f1f195afa192cfee860698584c030f4c9db1'; \`\`\`

1.  1.  web3.eth.defaultBlock

`   web3.eth.defaultBlock`

This default block is used for the following methods (optionally you can
override it by passing the defaultBlock parameter):

\- \[web3.eth.getBalance()\](\#web3ethgetbalance) -
\[web3.eth.getCode()\](\#web3ethgetcode) -
\[web3.eth.getTransactionCount()\](\#web3ethgettransactioncount) -
\[web3.eth.getStorageAt()\](\#web3ethgetstorageat) -
\[web3.eth.call()\](\#web3ethcall) -
\[contract.myMethod.call()\](\#contract-methods) -
\[contract.myMethod.estimateGas()\](\#contract-methods)

1.  1.  Values

Default block parameters can be one of the following:

\- \`Number\` - a block number - \`String\` - \`"earliest"\`, the
genisis block - \`String\` - \`"latest"\`, the latest block (current
head of the blockchain) - \`String\` - \`"pending"\`, the currently
mined block (including pending transactions)

  - Default is\* \`latest\`

<!-- end list -->

1.  1.  Returns

\`Number|String\` - The default block number to use when querying a
state.

\`\`\`js var defaultBlock = web3.eth.defaultBlock;
console.log(defaultBlock); // 'latest'

// set the default block web3.eth.defaultBlock = 231; \`\`\`

1.  1.  web3.eth.syncing

`   web3.eth.syncing`
`   // or async`
`   web3.eth.getSyncing(callback(error, result){ ... })`

This property is read only and returns the either a sync object, when
the node is syncing or \`false\`.

1.  1.  Returns

\`Object|Boolean\` - A sync object as follows, when the node is
currently syncing or
\`false\`:

``  - `startingBlock`: `Number` - The block number where the sync started.``
``  - `currentBlock`: `Number` - The block number where at which block the node currently synced to already.``
``  - `highestBlock`: `Number` - The estimated block number to sync to.``

\`\`\`js var sync = web3.eth.syncing; console.log(sync); /\* {

`  startingBlock: 300,`
`  currentBlock: 312,`
`  highestBlock: 512`

}

  - /

\`\`\`

1.  1.  web3.eth.isSyncing

`   web3.eth.isSyncing(callback);`

This convenience function calls the \`callback\` everytime a sync
starts, updates and stops.

1.  1.  Returns

\`Object\` - a isSyncing object with the following
methods:

`` * `syncing.addCallback()`: Adds another callback, which will be called when the node starts or stops syncing.``
`` * `syncing.stopWatching()`: Stops the syncing callbacks.``

1.  1.  Callback return value

\- \`Boolean\` - The callback will be fired with \`true\` when the
syncing starts and with \`false\` when it stopped. - \`Object\` - While
syncing it will return the syncing
object:

``  - `startingBlock`: `Number` - The block number where the sync started.``
``  - `currentBlock`: `Number` - The block number where at which block the node currently synced to already.``
``  - `highestBlock`: `Number` - The estimated block number to sync to.``

\`\`\`js web3.eth.isSyncing(function(error,
sync){

`   if(!error) {`
`       // stop all app activity`
`       if(sync === true) {`
``          // we use `true`, so it stops all filters, but not the web3.eth.syncing polling``
`          web3.reset(true);`
`       `
`       // show sync info`
`       } else if(sync) {`
`          console.log(sync.currentBlock);`
`       `
`       // re-gain app operation`
`       } else {`
`           // run your app init function...`
`       }`
`   }`

}); \`\`\`

1.  1.  web3.eth.coinbase

`   web3.eth.coinbase`
`   // or async`
`   web3.eth.getCoinbase(callback(error, result){ ... })`

This property is read only and returns the coinbase address were the
mining rewards go to.

1.  1.  Returns

\`String\` - The coinbase address of the client.

\`\`\`js var coinbase = web3.eth.coinbase; console.log(coinbase); //
"0x407d73d8a49eeb85d32cf465507dd71d507100c1" \`\`\`

1.  1.  web3.eth.mining

`   web3.eth.mining`
`   // or async`
`   web3.eth.getMining(callback(error, result){ ... })`

This property is read only and says whether the node is mining or not.

1.  1.  Returns

\`Boolean\` - \`true\` if the client is mining, otherwise \`false\`.

\`\`\`js var mining = web3.eth.mining; console.log(mining); // true or
false \`\`\`

1.  1.  web3.eth.hashrate

`   web3.eth.hashrate`
`   // or async`
`   web3.eth.getHashrate(callback(error, result){ ... })`

This property is read only and returns the number of hashes per second
that the node is mining with.

1.  1.  Returns

\`Number\` - number of hashes per second.

\`\`\`js var hashrate = web3.eth.hashrate; console.log(hashrate); //
493736 \`\`\`

1.  1.  web3.eth.gasPrice

`   web3.eth.gasPrice`
`   // or async`
`   web3.eth.getGasPrice(callback(error, result){ ... })`

This property is read only and returns the current gas price. The gas
price is determined by the x latest blocks median gas price.

1.  1.  Returns

\`BigNumber\` - A BigNumber instance of the current gas price in wei.

See the \[note on BigNumber\](\#a-note-on-big-numbers-in-javascript).

\`\`\`js var gasPrice = web3.eth.gasPrice;
console.log(gasPrice.toString(10)); // "10000000000000" \`\`\`

1.  1.  web3.eth.accounts

`   web3.eth.accounts`
`   // or async`
`   web3.eth.getAccounts(callback(error, result){ ... })`

This property is read only and returns a list of accounts the node
controls.

1.  1.  Returns

\`Array\` - An array of addresses controlled by client.

\`\`\`js var accounts = web3.eth.accounts; console.log(accounts); //
\["0x407d73d8a49eeb85d32cf465507dd71d507100c1"\] \`\`\`

1.  1.  web3.eth.blockNumber

`   web3.eth.blockNumber`
`   // or async`
`   web3.eth.getBlockNumber(callback(error, result){ ... })`

This property is read only and returns the current block number.

1.  1.  Returns

\`Number\` - The number of the most recent block.

\`\`\`js var number = web3.eth.blockNumber; console.log(number); // 2744
\`\`\`

1.  1.  web3.eth.register

`   web3.eth.register(addressHexString [, callback])`

(Not Implemented yet) Registers the given address to be included in
\`web3.eth.accounts\`. This allows non-private-key owned accounts to be
associated as an owned account (e.g., contract wallets).

1.  1.  Parameters

1\. \`String\` - The address to register 2. \`Function\` - (optional) If
you pass a callback the HTTP request is made asynchronous. See \[this
note\](\#using-callbacks) for details.

1.  1.  Returns

?

\`\`\`js web3.eth.register("0x407d73d8a49eeb85d32cf465507dd71d507100ca")
\`\`\`

1.  1.  web3.eth.unRegister

`    web3.eth.unRegister(addressHexString [, callback])`

(Not Implemented yet) Unregisters a given address.

1.  1.  Parameters

1\. \`String\` - The address to unregister. 2. \`Function\` - (optional)
If you pass a callback the HTTP request is made asynchronous. See \[this
note\](\#using-callbacks) for details.

1.  1.  Returns

?

\`\`\`js
web3.eth.unregister("0x407d73d8a49eeb85d32cf465507dd71d507100ca") \`\`\`

1.  1.  web3.eth.getBalance

`   web3.eth.getBalance(addressHexString [, defaultBlock] [, callback])`

Get the balance of an address at a given block.

1.  1.  Parameters

1\. \`String\` - The address to get the balance of. 2. \`Number|String\`
- (optional) If you pass this parameter it will not use the default
block set with \[web3.eth.defaultBlock\](\#web3ethdefaultblock). 3.
\`Function\` - (optional) If you pass a callback the HTTP request is
made asynchronous. See \[this note\](\#using-callbacks) for details.

1.  1.  Returns

\`String\` - A BigNumber instance of the current balance for the given
address in wei.

See the \[note on BigNumber\](\#a-note-on-big-numbers-in-javascript).

\`\`\`js var balance =
web3.eth.getBalance("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
console.log(balance); // instanceof BigNumber
console.log(balance.toString(10)); // '1000000000000'
console.log(balance.toNumber()); // 1000000000000
\`\`\`

1.  1.  web3.eth.getStorageAt

`   web3.eth.getStorageAt(addressHexString, position [, defaultBlock] [, callback])`

Get the storage at a specific position of an address.

1.  1.  Parameters

1\. \`String\` - The address to get the storage from. 2. \`Number\` -
The index position of the storage. 3. \`Number|String\` - (optional) If
you pass this parameter it will not use the default block set with
\[web3.eth.defaultBlock\](\#web3ethdefaultblock). 4. \`Function\` -
(optional) If you pass a callback the HTTP request is made asynchronous.
See \[this note\](\#using-callbacks) for details.

1.  1.  Returns

\`String\` - The value in storage at the given position.

\`\`\`js var state =
web3.eth.getStorageAt("0x407d73d8a49eeb85d32cf465507dd71d507100c1", 0);
console.log(state); // "0x03" \`\`\`

1.  1.  web3.eth.getCode

`   web3.eth.getCode(addressHexString [, defaultBlock] [, callback])`

Get the code at a specific address.

1.  1.  Parameters

1\. \`String\` - The address to get the code from. 2. \`Number|String\`
- (optional) If you pass this parameter it will not use the default
block set with \[web3.eth.defaultBlock\](\#web3ethdefaultblock). 3.
\`Function\` - (optional) If you pass a callback the HTTP request is
made asynchronous. See \[this note\](\#using-callbacks) for details.

1.  1.  Returns

\`String\` - The data at given address \`addressHexString\`.

\`\`\`js var code =
web3.eth.getCode("0xd5677cf67b5aa051bb40496e68ad359eb97cfbf8");
console.log(code); //
"0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056"
\`\`\`

1.  1.  web3.eth.getBlock

`    web3.eth.getBlock(blockHashOrBlockNumber [, returnTransactionObjects] [, callback])`

Returns a block matching the block number or block hash.

1.  1.  Parameters

1\. \`String|Number\` - The block number or hash. Or the string
\`"earliest"\`, \`"latest"\` or \`"pending"\` as in the \[default block
parameter\](\#web3ethdefaultblock). 2. \`Boolean\` - (optional, default
\`false\`) If \`true\`, the returned block will contain all transactions
as objects, if \`false\` it will only contains the transaction hashes.
3. \`Function\` - (optional) If you pass a callback the HTTP request is
made asynchronous. See \[this note\](\#using-callbacks) for details.

1.  1.  Returns

\`Object\` - The block
object:

`` - `number`: `Number` - the block number. `null` when its pending block.``
`` - `hash`: `String`, 32 Bytes - hash of the block. `null` when its pending block.``
`` - `parentHash`: `String`, 32 Bytes - hash of the parent block.``
`` - `nonce`: `String`, 8 Bytes - hash of the generated proof-of-work. `null` when its pending block.``
`` - `sha3Uncles`: `String`, 32 Bytes - SHA3 of the uncles data in the block.``
`` - `logsBloom`: `String`, 256 Bytes - the bloom filter for the logs of the block. `null` when its pending block.``
`` - `transactionsRoot`: `String`, 32 Bytes - the root of the transaction trie of the block``
`` - `stateRoot`: `String`, 32 Bytes - the root of the final state trie of the block.``
`` - `miner`: `String`, 20 Bytes - the address of the beneficiary to whom the mining rewards were given.``
`` - `difficulty`: `BigNumber` - integer of the difficulty for this block.``
`` - `totalDifficulty`: `BigNumber` - integer of the total difficulty of the chain until this block.``
`` - `extraData`: `String` - the "extra data" field of this block.``
`` - `size`: `Number` - integer the size of this block in bytes.``
`` - `gasLimit`: `Number` - the maximum gas allowed in this block.``
`` - `gasUsed`: `Number` - the total used gas by all transactions in this block.``
`` - `timestamp`: `Number` - the unix timestamp for when the block was collated.``
`` - `transactions`: `Array` - Array of transaction objects, or 32 Bytes transaction hashes depending on the last given parameter.``
`` - `uncles`: `Array` - Array of uncle hashes.``

\`\`\`js var info = web3.eth.getBlock(3150); console.log(info); /\*
{

` "number": 3,`
` "hash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",`
` "parentHash": "0x2302e1c0b972d00932deb5dab9eb2982f570597d9d42504c05d9c2147eaf9c88",`
` "nonce": "0xfb6e1a62d119228b",`
` "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",`
` "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",`
` "transactionsRoot": "0x3a1b03875115b79539e5bd33fb00d8f7b7cd61929d5a3c574f507b8acf415bee",`
` "stateRoot": "0xf1133199d44695dfa8fd1bcfe424d82854b5cebef75bddd7e40ea94cda515bcb",`
` "miner": "0x8888f1f195afa192cfee860698584c030f4c9db1",`
` "difficulty": BigNumber,`
` "totalDifficulty": BigNumber,`
` "size": 616,`
` "extraData": "0x",`
` "gasLimit": 3141592,`
` "gasUsed": 21662,`
` "timestamp": 1429287689,`
` "transactions": [`
`   "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b"`
` ],`
` "uncles": []`

}

  - /

\`\`\`

1.  1.  web3.eth.getBlockTransactionCount

`   web3.eth.getBlockTransactionCount(hashStringOrBlockNumber [, callback])`

Returns the number of transaction in a given block.

1.  1.  Parameters

1\. \`String|Number\` - The block number or hash. Or the string
\`"earliest"\`, \`"latest"\` or \`"pending"\` as in the \[default block
parameter\](\#web3ethdefaultblock). 2. \`Function\` - (optional) If you
pass a callback the HTTP request is made asynchronous. See \[this
note\](\#using-callbacks) for details.

1.  1.  Returns

\`Number\` - The number of transactions in the given block.

\`\`\`js var number =
web3.eth.getBlockTransactionCount("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
console.log(number); // 1
\`\`\`

1.  1.  web3.eth.getUncle

`   web3.eth.getUncle(blockHashStringOrNumber, uncleNumber [, returnTransactionObjects] [, callback])`

Returns a blocks uncle by a given uncle index position.

1.  1.  Parameters

1\. \`String|Number\` - The block number or hash. Or the string
\`"earliest"\`, \`"latest"\` or \`"pending"\` as in the \[default block
parameter\](\#web3ethdefaultblock). 2. \`Number\` - The index position
of the uncle. 3. \`Boolean\` - (optional, default \`false\`) If
\`true\`, the returned block will contain all transactions as objects,
if \`false\` it will only contains the transaction hashes. 4.
\`Function\` - (optional) If you pass a callback the HTTP request is
made asynchronous. See \[this note\](\#using-callbacks) for details.

1.  1.  Returns

\`Object\` - the returned uncle. For a return value see
\[web3.eth.getBlock()\](\#web3ethgetblock).

  -   - Note\*\*: An uncle doesn't contain individual transactions.

\`\`\`js var uncle = web3.eth.getUncle(500, 0); console.log(uncle); //
see web3.eth.getBlock

\`\`\`

  -   -
<!-- end list -->

1.  1.  web3.eth.getTransaction

`   web3.eth.getTransaction(transactionHash [, callback])`

Returns a transaction matching the given transaction hash.

1.  1.  Parameters

1\. \`String\` - The transaction hash. 2. \`Function\` - (optional) If
you pass a callback the HTTP request is made asynchronous. See \[this
note\](\#using-callbacks) for details.

1.  1.  Returns

\`Object\` - A transaction object its hash
\`transactionHash\`:

`` - `hash`: `String`, 32 Bytes - hash of the transaction.``
`` - `nonce`: `Number` - the number of transactions made by the sender prior to this one.``
`` - `blockHash`: `String`, 32 Bytes - hash of the block where this transaction was in. `null` when its pending.``
`` - `blockNumber`: `Number` - block number where this transaction was in. `null` when its pending.``
`` - `transactionIndex`: `Number` - integer of the transactions index position in the block. `null` when its pending.``
`` - `from`: `String`, 20 Bytes - address of the sender.``
`` - `to`: `String`, 20 Bytes - address of the receiver. `null` when its a contract creation transaction.``
`` - `value`: `BigNumber` - value transferred in Wei.``
`` - `gasPrice`: `BigNumber` - gas price provided by the sender in Wei.``
`` - `gas`: `Number` - gas provided by the sender.``
`` - `input`: `String` - the data sent along with the transaction.``

\`\`\`js var blockNumber = 668; var indexOfTransaction = 0

var transaction = web3.eth.getTransaction(blockNumber,
indexOfTransaction); console.log(transaction); /\*
{

` "hash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b",`
` "nonce": 2,`
` "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",`
` "blockNumber": 3,`
` "transactionIndex": 0,`
` "from": "0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b",`
` "to": "0x6295ee1b4f6dd65047762f924ecd367c17eabf8f",`
` "value": BigNumber,`
` "gas": 314159,`
` "gasPrice": BigNumber,`
` "input": "0x57cb2fc4"`

}

  - /

\`\`\`

1.  1.  web3.eth.getTransactionFromBlock

`   getTransactionFromBlock(hashStringOrNumber, indexNumber [, callback])`

Returns a transaction based on a block hash or number and the
transactions index position.

1.  1.  Parameters

1\. \`String\` - A block number or hash. Or the string \`"earliest"\`,
\`"latest"\` or \`"pending"\` as in the \[default block
parameter\](\#web3ethdefaultblock). 2. \`Number\` - The transactions
index position. 3. \`Function\` - (optional) If you pass a callback the
HTTP request is made asynchronous. See \[this note\](\#using-callbacks)
for details.

1.  1.  Returns

\`Object\` - A transaction object, see
\[web3.eth.getTransaction\](\#web3ethgettransaction):

\`\`\`js var transaction =
web3.eth.getTransactionFromBlock('0x4534534534', 2);
console.log(transaction); // see web3.eth.getTransaction

\`\`\`

1.  1.  web3.eth.getTransactionReceipt

`   web3.eth.getTransactionReceipt(hashString [, callback])`

Returns the receipt of a transaction by transaction hash.

  -   - Note\*\* That the receipt is not available for pending
        transactions.

<!-- end list -->

1.  1.  Parameters

1\. \`String\` - The transaction hash. 2. \`Function\` - (optional) If
you pass a callback the HTTP request is made asynchronous. See \[this
note\](\#using-callbacks) for details.

1.  1.  Returns

\`Object\` - A transaction receipt object, or \`null\` when no receipt
was
found:

`` - `blockHash`: `String`, 32 Bytes - hash of the block where this transaction was in.``
`` - `blockNumber`: `Number` - block number where this transaction was in.``
`` - `transactionHash`: `String`, 32 Bytes - hash of the transaction.``
`` - `transactionIndex`: `Number` - integer of the transactions index position in the block.``
`` - `from`: `String`, 20 Bytes - address of the sender.``
`` - `to`: `String`, 20 Bytes - address of the receiver. `null` when its a contract creation transaction.``
`` - `cumulativeGasUsed `: `Number ` - The total amount of gas used when this transaction was executed in the block.``
`` - `gasUsed `: `Number ` -  The amount of gas used by this specific transaction alone.``
`` - `contractAddress `: `String` - 20 Bytes - The contract address created, if the transaction was a contract creation, otherwise `null`.``
`` - `logs `:  `Array` - Array of log objects, which this transaction generated.``

\`\`\`js var receipt =
web3.eth.getTransactionReceipt('0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b');
console.log(receipt);
{

` "transactionHash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b",`
` "transactionIndex": 0,`
` "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",`
` "blockNumber": 3,`
` "contractAddress": "0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b",`
` "cumulativeGasUsed": 314159,`
` "gasUsed": 30234,`
` "logs": [{`
`        // logs as returned by getFilterLogs, etc.`
`    }, ...]`

}
\`\`\`

1.  1.  web3.eth.getTransactionCount

`   web3.eth.getTransactionCount(addressHexString [, defaultBlock] [, callback])`

Get the numbers of transactions sent from this address.

1.  1.  Parameters

1\. \`String\` - The address to get the numbers of transactions from. 2.
\`Number|String\` - (optional) If you pass this parameter it will not
use the default block set with
\[web3.eth.defaultBlock\](\#web3ethdefaultblock). 3. \`Function\` -
(optional) If you pass a callback the HTTP request is made asynchronous.
See \[this note\](\#using-callbacks) for details.

1.  1.  Returns

\`Number\` - The number of transactions sent from the given address.

\`\`\`js var number =
web3.eth.getTransactionCount("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
console.log(number); // 1 \`\`\`

1.  1.  web3.eth.sendTransaction

`   web3.eth.sendTransaction(transactionObject [, callback])`

Sends a transaction to the network.

1.  1.  Parameters

1\. \`Object\` - The transaction object to
send:

`` - `from`: `String` - The address for the sending account. Uses the [web3.eth.defaultAccount](#web3ethdefaultaccount) property, if not specified.``
`` - `to`: `String` - (optional) The destination address of the message, left undefined for a contract-creation transaction.``
`` - `value`: `Number|String|BigNumber` - (optional) The value transferred for the transaction in Wei, also the endowment if it's a contract-creation transaction.``
`` - `gas`: `Number|String|BigNumber` - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).``
`` - `gasPrice`: `Number|String|BigNumber` - (optional, default: To-Be-Determined) The price of gas for this transaction in wei, defaults to the mean network gas price.``
`` - `data`: `String` - (optional) Either a [byte string](https://github.com/ethereum/wiki/wiki/Solidity,-Docs-and-ABI) containing the associated data of the message, or in the case of a contract-creation transaction, the initialisation code.``
`` - `nonce`: `Number`  - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.``

2\. \`Function\` - (optional) If you pass a callback the HTTP request is
made asynchronous. See \[this note\](\#using-callbacks) for details.

1.  1.  Returns

\`String\` - The 32 Bytes transaction hash as HEX string.

If the transaction was a contract creation use
\[web3.eth.getTransactionReceipt()\](\#web3gettransactionreceipt) to get
the contract address, after the transaction was mined.

\`\`\`js

// compiled solidity source code using
<https://chriseth.github.io/cpp-ethereum/> var code =
"603d80600c6000396000f3007c01000000000000000000000000000000000000000000000000000000006000350463c6888fa18114602d57005b6007600435028060005260206000f3";

web3.eth.sendTransaction({data: code}, function(err, address)
{

` if (!err)`
`   console.log(address); // "0x7f9fade1c0d57a7af66ab4ead7c2eb7b11a91385"`

}); \`\`\`

1.  1.  web3.eth.sendRawTransaction

`   web3.eth.sendRawTransaction(signedTransactionData [, callback])`

Sends an already signed transaction. For example can be signed using:
<https://github.com/SilentCicero/ethereumjs-accounts>

1.  1.  Parameters

1\. \`String\` - Signed transaction data in HEX format 2. \`Function\` -
(optional) If you pass a callback the HTTP request is made asynchronous.
See \[this note\](\#using-callbacks) for details.

1.  1.  Returns

\`String\` - The 32 Bytes transaction hash as HEX string.

If the transaction was a contract creation use
\[web3.eth.getTransactionReceipt()\](\#web3gettransactionreceipt) to get
the contract address, after the transaction was mined.

\`\`\`js var Tx = require('ethereumjs-tx'); var privateKey = new
Buffer('e331b6d69882b4cb4ea581d88e0b604039a3de5967688d3dcffdd2270c0fd109',
'hex')

var rawTx =
{

` nonce: '0x00',`
` gasPrice: '0x09184e72a000', `
` gasLimit: '0x2710',`
` to: '0x0000000000000000000000000000000000000000', `
` value: '0x00', `
` data: '0x7f7465737432000000000000000000000000000000000000000000000000000000600057'`

}

var tx = new Tx(rawTx); tx.sign(privateKey);

var serializedTx = tx.serialize();

//console.log(serializedTx.toString('hex'));
//0xf889808609184e72a00082271094000000000000000000000000000000000000000080a47f74657374320000000000000000000000000000000000000000000000000000006000571ca08a8bbf888cfa37bbf0bb965423625641fc956967b81d12e23709cead01446075a01ce999b56a8a88504be365442ea61239198e23d1fce7d00fcfc5cd3b44b7215f

web3.eth.sendRawTransaction(serializedTx.toString('hex'), function(err,
hash)
{

` if (!err)`
`   console.log(hash); // "0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385"`

}); \`\`\`

  -   -
<!-- end list -->

1.  1.  web3.eth.sign

`   web3.eth.sign(address, dataToSign, [, callback])`

Signs data from a specific account. This account needs to be unlocked.

1.  1.  Parameters

1\. \`String\` - Address to sign with. 2. \`String\` - Data to sign. 3.
\`Function\` - (optional) If you pass a callback the HTTP request is
made asynchronous. See \[this note\](\#using-callbacks) for details.

1.  1.  Returns

\`String\` - The signed data.

After the hex prefix, characters correspond to ECDSA values like this:
\`\`\` r = signature\[0:64\] s = signature\[64:128\] v =
signature\[128:130\] \`\`\`

Note that if you are using \`ecrecover\`, \`v\` will be either \`"00"\`
or \`"01"\`. As a result, in order to use this value, you will have to
parse it to an integer and then add \`27\`. This will result in either a
\`27\` or a \`28\`.

\`\`\`js var result =
web3.eth.sign("0x135a7de83802408321b74c322f8558db1679ac20",

`   "0x9dd2c369a187b4e6b9c402f030e50743e619301ea62aa4c0737d4ef7e10a3d49"); // second argument is web3.sha3("xyz")`

console.log(result); //
"0x30755ed65396facf86c53e6217c52b4daebe72aa4941d89635409de4c9c7f9466d4e9aaec7977f05e923889b33c0d0dd27d7226b6e6f56ce737465c5cfd04be400"
\`\`\`

1.  1.  web3.eth.call

`   web3.eth.call(callObject [, defaultBlock] [, callback])`

Executes a message call transaction, which is directly executed in the
VM of the node, but never mined into the blockchain.

1.  1.  Parameters

1\. \`Object\` - A transaction object see
\[web3.eth.sendTransaction\](\#web3ethsendtransaction), with the
difference that for calls the \`from\` property is optional as well. 2.
\`Number|String\` - (optional) If you pass this parameter it will not
use the default block set with
\[web3.eth.defaultBlock\](\#web3ethdefaultblock). 3. \`Function\` -
(optional) If you pass a callback the HTTP request is made asynchronous.
See \[this note\](\#using-callbacks) for details.

1.  1.  Returns

\`String\` - The returned data of the call, e.g. a codes functions
return value.

\`\`\`js var result =
web3.eth.call({

`   to: "0xc4abd0339eb8d57087278718986382264244252f", `
`   data: "0xc6888fa10000000000000000000000000000000000000000000000000000000000000003"`

}); console.log(result); //
"0x0000000000000000000000000000000000000000000000000000000000000015"
\`\`\`

1.  1.  web3.eth.estimateGas

`   web3.eth.estimateGas(callObject [, callback])`

Executes a message call or transaction, which is directly executed in
the VM of the node, but never mined into the blockchain and returns the
amount of the gas used.

1.  1.  Parameters

See \[web3.eth.sendTransaction\](\#web3ethsendtransaction), except that
all properties are optional.

1.  1.  Returns

\`Number\` - the used gas for the simulated call/transaction.

\`\`\`js var result =
web3.eth.estimateGas({

`   to: "0xc4abd0339eb8d57087278718986382264244252f", `
`   data: "0xc6888fa10000000000000000000000000000000000000000000000000000000000000003"`

}); console.log(result); //
"0x0000000000000000000000000000000000000000000000000000000000000015"
\`\`\`

1.  1.  web3.eth.filter

\`\`\`js // can be 'latest' or 'pending' var filter =
web3.eth.filter(filterString); // OR object are log filter options var
filter = web3.eth.filter(options);

// watch for changes filter.watch(function(error, result){

` if (!error)`
`   console.log(result);`

});

// Additionally you can start watching right away, by passing a
callback: web3.eth.filter(options, function(error, result){

` if (!error)`
`   console.log(result);`

}); \`\`\`

1.  1.  Parameters

1\. \`String|Object\` - The string \`"latest"\` or \`"pending"\` to
watch for changes in the latest block or pending transactions
respectively. Or a filter options object as
follows:

`` * `fromBlock`: `Number|String` - The number of the earliest block (`latest` may be given to mean the most recent and `pending` currently mining, block). By default `latest`.``
`` * `toBlock`: `Number|String` - The number of the latest block (`latest` may be given to mean the most recent and `pending` currently mining, block). By default `latest`.``
`` * `address`: `String` - An address or a list of addresses to only get logs from particular account(s).``
`` * `topics`: `Array of Strings` - An array of values which must each appear in the log entries. The order is important, if you want to leave topics out use `null`, e.g. `[null, '0x00...']`. You can also pass another array for each topic with options for that topic e.g. `[null, ['option1', 'option2']]` ``

1.  1.  Returns

\`Object\` - A filter object with the following
methods:

`` * `filter.get(callback)`: Returns all of the log entries that fit the filter.``
`` * `filter.watch(callback)`: Watches for state changes that fit the filter and calls the callback. See [this note](#using-callbacks) for details.``
`` * `filter.stopWatching()`: Stops the watch and uninstalls the filter in the node. Should always be called once it is done.``

1.  1.  Watch callback return value

\- \`String\` - When using the \`"latest"\` parameter, it returns the
block hash of the last incoming block. - \`String\` - When using the
\`"pending"\` parameter, it returns a transaction hash of the most
recent pending transaction. - \`Object\` - When using manual filter
options, it returns a log object as
follows:

``   - `logIndex`: `Number` - integer of the log index position in the block. `null` when its pending log.``
``   - `transactionIndex`: `Number` - integer of the transactions index position log was created from. `null` when its pending log.``
``   - `transactionHash`: `String`, 32 Bytes - hash of the transactions this log was created from. `null` when its pending log.``
``   - `blockHash`: `String`, 32 Bytes - hash of the block where this log was in. `null` when its pending. `null` when its pending log.``
``   - `blockNumber`: `Number` - the block number where this log was in. `null` when its pending. `null` when its pending log.``
``   - `address`: `String`, 32 Bytes - address from which this log originated.``
``   - `data`: `String` - contains one or more 32 Bytes non-indexed arguments of the log.``
``   - `topics`: `Array of Strings` - Array of 0 to 4 32 Bytes `DATA` of indexed log arguments. (In *solidity*: The first topic is the *hash* of the signature of the event (e.g. `Deposit(address,bytes32,uint256)`), except if you declared the event with the `anonymous` specifier.)``

  -   - Note\*\* For event filter return values see \[Contract
        Events\](\#contract-events)

\`\`\`js var filter = web3.eth.filter('pending');

filter.watch(function (error, log)
{

` console.log(log); //  {"address":"0x0000000000000000000000000000000000000000", "data":"0x0000000000000000000000000000000000000000000000000000000000000000", ...}`

});

// get all past logs again. var myResults = filter.get(function(error,
logs){ ... });

...

// stops and uninstalls the filter filter.stopWatching();

\`\`\`

1.  1.  web3.eth.contract

`   web3.eth.contract(abiArray)`

Creates a contract object for a solidity contract, which can be used to
initiate contracts on an address. You can read more about events
\[here\](https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI\#example-javascript-usage).

1.  1.  Parameters

1\. \`Array\` - ABI array with descriptions of functions and events of
the contract.

1.  1.  Returns

\`Object\` - A contract object, which can be initiated as follows:

\`\`\`js var MyContract = web3.eth.contract(abiArray);

// instantiate by address var contractInstance =
MyContract.at(\[address\]);

// deploy new contract var contractInstance =
MyContract.new(\[contructorParam1\] \[, contructorParam2\], {data:
'0x12345...', from: myAccount, gas: 1000000});

// Get the data to deploy the contract manually var contractData =
MyContract.new.getData(\[contructorParam1\] \[, contructorParam2\],
{data: '0x12345...'}); // contractData =
'0x12345643213456000000000023434234' \`\`\`

And then you can either initiate an existing contract on an address, or
deploy the contract using the compiled byte code:

\`\`\`js // Instantiate from an existing address: var myContractInstance
= MyContract.at(myContractAddress);

// Or deploy a new contract:

// Deploy the contract asyncronous from Solidity file: ... const fs =
require("fs"); const solc = require('solc')

let source = fs.readFileSync('nameContract.sol', 'utf8'); let
compiledContract = solc.compile(source, 1); let abi =
compiledContract.contracts\['nameContract'\].interface; let bytecode =
compiledContract.contracts\['nameContract'\].bytecode; let gasEstimate =
web3.eth.estimateGas({data: bytecode}); let MyContract =
web3.eth.contract(JSON.parse(abi));

var myContractReturned = MyContract.new(param1, param2,
{

`  from:mySenderAddress,`
`  `<data:bytecode>`,`
`  gas:gasEstimate}, function(err, myContract){`
`   if(!err) {`
`      // NOTE: The callback will fire twice!`
`      // Once the contract has the transactionHash property set and once its deployed on an address.`

`      // e.g. check tx hash on the first call (transaction send)`
`      if(!myContract.address) {`
`          console.log(myContract.transactionHash) // The hash of the transaction, which deploys the contract`
`      `
`      // check address on the second call (contract deployed)`
`      } else {`
`          console.log(myContract.address) // the contract address`
`      }`

`      // Note that the returned "myContractReturned" === "myContract",`
`      // so the returned "myContractReturned" object will also get the address set.`
`   }`
` });`

// Deploy contract syncronous: The address will be added as soon as the
contract is mined. // Additionally you can watch the transaction by
using the "transactionHash" property var myContractInstance =
MyContract.new(param1, param2, {data: myContractCode, gas: 300000, from:
mySenderAddress}); myContractInstance.transactionHash // The hash of the
transaction, which created the contract myContractInstance.address //
undefined at start, but will be auto-filled later \`\`\`

\`\`\`js // contract abi var abi = \[{

`    name: 'myConstantMethod',`
`    type: 'function',`
`    constant: true,`
`    inputs: [{ name: 'a', type: 'string' }],`
`    outputs: [{name: 'd', type: 'string' }]`

},
{

`    name: 'myStateChangingMethod',`
`    type: 'function',`
`    constant: false,`
`    inputs: [{ name: 'a', type: 'string' }, { name: 'b', type: 'int' }],`
`    outputs: []`

},
{

`    name: 'myEvent',`
`    type: 'event',`
`    inputs: [{name: 'a', type: 'int', indexed: true},{name: 'b', type: 'bool', indexed: false}]`

}\];

// creation of contract object var MyContract = web3.eth.contract(abi);

// initiate contract for an address var myContractInstance =
MyContract.at('0xc4abd0339eb8d57087278718986382264244252f');

// call constant function var result =
myContractInstance.myConstantMethod('myParam'); console.log(result) //
'0x25434534534'

// send a transaction to a function
myContractInstance.myStateChangingMethod('someParam1', 23, {value: 200,
gas: 2000});

// short hand style
web3.eth.contract(abi).at(address).myAwesomeMethod(...);

// create filter var filter = myContractInstance.myEvent({a: 5},
function (error, result)
{

` if (!error)`
`   console.log(result);`
`   /*`
`   {`
`       address: '0x8718986382264244252fc4abd0339eb8d5708727',`
`       topics: "0x12345678901234567890123456789012", "0x0000000000000000000000000000000000000000000000000000000000000005",`
`       data: "0x0000000000000000000000000000000000000000000000000000000000000001",`
`       ...`
`   }`
`   */`

}); \`\`\`

1.  1.  Contract Methods

\`\`\`js // Automatically determines the use of call or sendTransaction
based on the method type myContractInstance.myMethod(param1 \[, param2,
...\] \[, transactionObject\] \[, defaultBlock\] \[, callback\]);

// Explicitly calling this method
myContractInstance.myMethod.call(param1 \[, param2, ...\] \[,
transactionObject\] \[, defaultBlock\] \[, callback\]);

// Explicitly sending a transaction to this method
myContractInstance.myMethod.sendTransaction(param1 \[, param2, ...\] \[,
transactionObject\] \[, callback\]);

// Get the call data, so you can call the contract through some other
means var myCallData = myContractInstance.myMethod.getData(param1 \[,
param2, ...\]); // myCallData = '0x45ff3ff6000000000004545345345345..'
\`\`\`

The contract object exposes the contract's methods, which can be called
using parameters and a transaction object.

1.  1.  Parameters

\- \`String|Number\` - (optional) Zero or more parameters of the
function. If passing in a string, it must be formatted as a hex number,
e.g. "0xdeadbeef". - \`Object\` - (optional) The (previous) last
parameter can be a transaction object, see
\[web3.eth.sendTransaction\](\#web3ethsendtransaction) parameter 1 for
more. \*\*Note\*\*: \`data\` and \`to\` properties will not be taken
into account. - \`Number|String\` - (optional) If you pass this
parameter it will not use the default block set with
\[web3.eth.defaultBlock\](\#web3ethdefaultblock). - \`Function\` -
(optional) If you pass a callback as the last parameter the HTTP request
is made asynchronous. See \[this note\](\#using-callbacks) for details.

1.  1.  Returns

\`String\` - If its a call the result data, if its a send transaction a
created contract address, or the transaction hash, see
\[web3.eth.sendTransaction\](\#web3ethsendtransaction) for details.

\`\`\`js // creation of contract object var MyContract =
web3.eth.contract(abi);

// initiate contract for an address var myContractInstance =
MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

var result = myContractInstance.myConstantMethod('myParam');
console.log(result) // '0x25434534534'

myContractInstance.myStateChangingMethod('someParam1', 23, {value: 200,
gas: 2000}, function(err, result){ ... }); \`\`\`

  -   -
<!-- end list -->

1.  1.  Contract Events

\`\`\`js var event = myContractInstance.MyEvent({valueA: 23} \[,
additionalFilterObject\])

// watch for changes event.watch(function(error, result){

` if (!error)`
`   console.log(result);`

});

// Or pass a callback to start watching immediately var event =
myContractInstance.MyEvent(\[{valueA: 23}\] \[, additionalFilterObject\]
, function(error, result){

` if (!error)`
`   console.log(result);`

});

\`\`\`

You can use events like \[filters\](\#web3ethfilter) and they have the
same methods, but you pass different objects to create the event filter.

1.  1.  Parameters

1\. \`Object\` - Indexed return values you want to filter the logs by,
e.g. \`{'valueA': 1, 'valueB': \[myFirstAddress, mySecondAddress\]}\`.
By default all filter values are set to \`null\`. It means, that they
will match any event of given type sent from this contract. 2.
\`Object\` - Additional filter options, see \[filters\](\#web3ethfilter)
parameter 1 for more. By default filterObject has field 'address' set to
address of the contract. Also first topic is the signature of event. 3.
\`Function\` - (optional) If you pass a callback as the last parameter
it will immediately start watching and you don't need to call
\`myEvent.watch(function(){})\`. See \[this note\](\#using-callbacks)
for details.

1.  1.  Callback return

\`Object\` - An event object as follows:

\- \`address\`: \`String\`, 32 Bytes - address from which this log
originated. - \`args\`: \`Object\` - The arguments coming from the
event. - \`blockHash\`: \`String\`, 32 Bytes - hash of the block where
this log was in. \`null\` when its pending. - \`blockNumber\`:
\`Number\` - the block number where this log was in. \`null\` when its
pending. - \`logIndex\`: \`Number\` - integer of the log index position
in the block. - \`event\`: \`String\` - The event name. - \`removed\`:
\`bool\` - indicate if the transaction this event was created from was
removed from the blockchain (due to orphaned block) or never get to it
(due to rejected transaction). - \`transactionIndex\`: \`Number\` -
integer of the transactions index position log was created from. -
\`transactionHash\`: \`String\`, 32 Bytes - hash of the transactions
this log was created from.

\`\`\`js var MyContract = web3.eth.contract(abi); var myContractInstance
= MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

// watch for an event with {some: 'args'} var myEvent =
myContractInstance.MyEvent({some: 'args'}, {fromBlock: 0, toBlock:
'latest'}); myEvent.watch(function(error, result){

`  ...`

});

// would get all past logs again. var myResults =
myEvent.get(function(error, logs){ ... });

...

// would stop and uninstall the filter myEvent.stopWatching(); \`\`\`

1.  1.  Contract allEvents

\`\`\`js var events =
myContractInstance.allEvents(\[additionalFilterObject\]);

// watch for changes events.watch(function(error, event){

` if (!error)`
`   console.log(event);`

});

// Or pass a callback to start watching immediately var events =
myContractInstance.allEvents(\[additionalFilterObject,\] function(error,
log){

` if (!error)`
`   console.log(log);`

});

\`\`\`

Will call the callback for all events which are created by this
contract.

1.  1.  Parameters

1\. \`Object\` - Additional filter options, see
\[filters\](\#web3ethfilter) parameter 1 for more. By default
filterObject has field 'address' set to address of the contract. This
method sets the topic to the signature of event, and does not support
additional topics. 2. \`Function\` - (optional) If you pass a callback
as the last parameter it will immediately start watching and you don't
need to call \`myEvent.watch(function(){})\`. See \[this
note\](\#using-callbacks) for details.

1.  1.  Callback return

\`Object\` - See \[Contract Events\](\#contract-events) for more.

\`\`\`js var MyContract = web3.eth.contract(abi); var myContractInstance
= MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

// watch for an event with {some: 'args'} var events =
myContractInstance.allEvents({fromBlock: 0, toBlock: 'latest'});
events.watch(function(error, result){

`  ...`

});

// would get all past logs again. events.get(function(error, logs){ ...
});

...

// would stop and uninstall the filter myEvent.stopWatching(); \`\`\`

\*\#\# web3.eth.getCompilers

`   web3.eth.getCompilers([callback])`

Gets a list of available compilers.

1.  1.  Parameters

1\. \`Function\` - (optional) If you pass a callback the HTTP request is
made asynchronous. See \[this note\](\#using-callbacks) for details.

1.  1.  Returns

\`Array\` - An array of strings of available compilers.

\`\`\`js var number = web3.eth.getCompilers(); console.log(number); //
\["lll", "solidity", "serpent"\] \`\`\`

1.  1.  web3.eth.compile.solidity

`   web3.eth.compile.solidity(sourceString [, callback])`

Compiles solidity source code.

1.  1.  Parameters

1\. \`String\` - The solidity source code. 2. \`Function\` - (optional)
If you pass a callback the HTTP request is made asynchronous. See \[this
note\](\#using-callbacks) for details.

1.  1.  Returns

\`Object\` - Contract and compiler info.

\`\`\`js var source = "" +

`   "contract test {\n" +`
`   "   function multiply(uint a) returns(uint d) {\n" +`
`   "       return a * 7;\n" +`
`   "   }\n" +`
`   "}\n";`

var compiled = web3.eth.compile.solidity(source); console.log(compiled);
//
{

` "test": {`
`   "code": "0x605280600c6000396000f3006000357c010000000000000000000000000000000000000000000000000000000090048063c6888fa114602e57005b60376004356041565b8060005260206000f35b6000600782029050604d565b91905056",`
`   "info": {`
`     "source": "contract test {\n\tfunction multiply(uint a) returns(uint d) {\n\t\treturn a * 7;\n\t}\n}\n",`
`     "language": "Solidity",`
`     "languageVersion": "0",`
`     "compilerVersion": "0.8.2",`
`     "abiDefinition": [`
`       {`
`         "constant": false,`
`         "inputs": [`
`           {`
`             "name": "a",`
`             "type": "uint256"`
`           }`
`         ],`
`         "name": "multiply",`
`         "outputs": [`
`           {`
`             "name": "d",`
`             "type": "uint256"`
`           }`
`         ],`
`         "type": "function"`
`       }`
`     ],`
`     "userDoc": {`
`       "methods": {}`
`     },`
`     "developerDoc": {`
`       "methods": {}`
`     }`
`   }`
` }`

} \`\`\`

1.  1.  web3.eth.compile.lll

`   web3. eth.compile.lll(sourceString [, callback])`

Compiles LLL source code.

1.  1.  Parameters

1\. \`String\` - The LLL source code. 2. \`Function\` - (optional) If
you pass a callback the HTTP request is made asynchronous. See \[this
note\](\#using-callbacks) for details.

1.  1.  Returns

\`String\` - The compiled LLL code as HEX string.

\`\`\`js var source = "...";

var code = web3.eth.compile.lll(source); console.log(code); //
"0x603880600c6000396000f3006001600060e060020a600035048063c6888fa114601857005b6021600435602b565b8060005260206000f35b600081600702905091905056"
\`\`\`

1.  1.  web3.eth.compile.serpent

`   web3.eth.compile.serpent(sourceString [, callback])`

Compiles serpent source code.

1.  1.  Parameters

1\. \`String\` - The serpent source code. 2. \`Function\` - (optional)
If you pass a callback the HTTP request is made asynchronous. See \[this
note\](\#using-callbacks) for details.

1.  1.  Returns

\`String\` - The compiled serpent code as HEX string.

\`\`\`js var source = "...";

var code = web3.eth.compile.serpent(source); console.log(code); //
"0x603880600c6000396000f3006001600060e060020a600035048063c6888fa114601857005b6021600435602b565b8060005260206000f35b600081600702905091905056"
\`\`\`

1.  1.  web3.eth.namereg

`   web3.eth.namereg`

Returns GlobalRegistrar object.

1.  1.  Usage

see
\[namereg\](https://github.com/ethereum/web3.js/blob/master/example/namereg.html)
example.

  -   -
<!-- end list -->

1.  1.  web3.db

<!-- end list -->

1.  1.  web3.db.putString

`   web3.db.putString(db, key, value)`

This method should be called, when we want to store a string in the
local leveldb database.

1.  1.  Parameters

1\. \`String\` - The database to store to. 2. \`String\` - The name of
the store. 3. \`String\` - The string value to store.

1.  1.  Returns

\`Boolean\` - \`true\` if successfull, otherwise \`false\`.

`param is db name, second is the key, and third is the string value.`

\`\`\`js web3.db.putString('testDB', 'key', 'myString') // true \`\`\`

1.  1.  web3.db.getString

`   web3.db.getString(db, key)`

This method should be called, when we want to get string from the local
leveldb database.

1.  1.  Parameters

1\. \`String\` - The database string name to retrieve from. 2.
\`String\` - The name of the store.

1.  1.  Returns

\`String\` - The stored value.

`param is db name and second is the key of string value.`

\`\`\`js var value = web3.db.getString('testDB', 'key');
console.log(value); // "myString" \`\`\`

1.  1.  web3.db.putHex

`   web3.db.putHex(db, key, value)`

This method should be called, when we want to store binary data in HEX
form in the local leveldb database.

1.  1.  Parameters

1\. \`String\` - The database to store to. 2. \`String\` - The name of
the store. 3. \`String\` - The HEX string to store.

1.  1.  Returns

\`Boolean\` - \`true\` if successfull, otherwise \`false\`.

\`\`\`js web3.db.putHex('testDB', 'key', '0x4f554b443'); // true

\`\`\`

1.  1.  web3.db.getHex

`   web3.db.getHex(db, key)`

This method should be called, when we want to get a binary data in HEX
form from the local leveldb database.

1.  1.  Parameters

1\. \`String\` - The database to store to. 2. \`String\` - The name of
the store.

1.  1.  Returns

\`String\` - The stored HEX value.

`param is db name and second is the key of value.`

\`\`\`js var value = web3.db.getHex('testDB', 'key');
console.log(value); // "0x4f554b443" \`\`\`

  -   -
<!-- end list -->

1.  1.  web3.shh

\[Whisper
Overview\](https://github.com/ethereum/wiki/wiki/Whisper-Overview)

\`\`\`js var shh = web3.shh; \`\`\`

1.  1.  web3.shh.post

`  web3.shh.post(object [, callback])`

This method should be called, when we want to post whisper message to
the network.

1.  1.  Parameters

1\. \`Object\` - The post
object:

`` - `from`: `String`, 60 Bytes HEX - (optional) The identity of the sender.``
`` - `to`: `String`, 60 Bytes  HEX - (optional) The identity of the receiver. When present whisper will encrypt the message so that only the receiver can decrypt it.``
`` - `topics`: `Array of Strings` - Array of topics `Strings`, for the receiver to identify messages.``
`` - `payload`: `String|Number|Object` - The payload of the message. Will be autoconverted to a HEX string before.``
`` - `priority`: `Number` - The integer of the priority in a rang from ... (?).``
`` - `ttl`: `Number` - integer of the time to live in seconds.``

2\. \`Function\` - (optional) If you pass a callback the HTTP request is
made asynchronous. See \[this note\](\#using-callbacks) for details.

1.  1.  Returns

\`Boolean\` - returns \`true\` if the message was send, otherwise
\`false\`.

\`\`\`js var identity = web3.shh.newIdentity(); var topic = 'example';
var payload = 'hello whisper world\!';

var message = {

` from: identity,`
` topics: [topic],`
` payload: payload,`
` ttl: 100,`
` workToProve: 100 // or priority TODO`

};

web3.shh.post(message); \`\`\`

1.  1.  web3.shh.newIdentity

`   web3.shh.newIdentity([callback])`

Should be called to create new identity.

1.  1.  Parameters

1\. \`Function\` - (optional) If you pass a callback the HTTP request is
made asynchronous. See \[this note\](\#using-callbacks) for details.

1.  1.  Returns

\`String\` - A new identity HEX string.

\`\`\`js var identity = web3.shh.newIdentity(); console.log(identity);
//
"0xc931d93e97ab07fe42d923478ba2465f283f440fd6cabea4dd7a2c807108f651b7135d1d6ca9007d5b68aa497e4619ac10aa3b27726e1863c1fd9b570d99bbaf"
\`\`\`

1.  1.  web3.shh.hasIdentity

`   web3.shh.hasIdentity(identity, [callback])`

Should be called, if we want to check if user has given identity.

1.  1.  Parameters

1\. \`String\` - The identity to check. 2. \`Function\` - (optional) If
you pass a callback the HTTP request is made asynchronous. See \[this
note\](\#using-callbacks) for details.

1.  1.  Returns

\`Boolean\` - returns \`true\` if the identity exists, otherwise
\`false\`.

\`\`\`js var identity = web3.shh.newIdentity(); var result =
web3.shh.hasIdentity(identity); console.log(result); // true

var result2 = web3.shh.hasIdentity(identity + "0");
console.log(result2); // false \`\`\`

1.  1.  web3.shh.newGroup

\`\`\`js // TODO: not implemented yet \`\`\`

1.  1.  web3.shh.addToGroup

\`\`\`js // TODO: not implemented yet \`\`\`

1.  1.  web3.shh.filter

\`\`\`js var filter = web3.shh.filter(options)

// watch for changes filter.watch(function(error, result){

` if (!error)`
`   console.log(result);`

}); \`\`\`

Watch for incoming whisper messages.

1.  1.  Parameters

1\. \`Object\` - The filter
options:

`` * `topics`: `Array of Strings` - Filters messages by this topic(s). You can use the following combinations:``
``   - `['topic1', 'topic2'] == 'topic1' && 'topic2'` ``
``   - `['topic1', ['topic2', 'topic3']] == 'topic1' && ('topic2' || 'topic3')` ``
``   - `[null, 'topic1', 'topic2'] == ANYTHING && 'topic1' && 'topic2'` -> `null` works as a wildcard``
`` * `to`: Filter by identity of receiver of the message. If provided and the node has this identity, it will decrypt incoming encrypted messages.``

2\. \`Function\` - (optional) If you pass a callback the HTTP request is
made asynchronous. See \[this note\](\#using-callbacks) for details.

1.  1.  Callback return

\`Object\` - The incoming
message:

`` - `from`: `String`, 60 Bytes - The sender of the message, if a sender was specified.``
`` - `to`: `String`, 60 Bytes - The receiver of the message, if a receiver was specified.``
`` - `expiry`: `Number` - Integer of the time in seconds when this message should expire (?).``
`` - `ttl`: `Number` -  Integer of the time the message should float in the system in seconds (?).``
`` - `sent`: `Number` -  Integer of the unix timestamp when the message was sent.``
`` - `topics`: `Array of String` - Array of `String` topics the message contained.``
`` - `payload`: `String` - The payload of the message.``
`` - `workProved`: `Number` - Integer of the work this message required before it was send (?).``

1.  1.  web3.eth.sendIBANTransaction

\`\`\`js var txHash =
web3.eth.sendIBANTransaction('0x00c5496aee77c1ba1f0854206a26dda82a81d6d8',
'XE81ETHXREGGAVOFYORK', 0x100); \`\`\`

Sends IBAN transaction from user account to destination IBAN address.

1.  1.  Parameters

\- \`string\` - address from which we want to send transaction -
\`string\` - IBAN address to which we want to send transaction -
\`value\` - value that we want to send in IBAN transaction

1.  1.  web3.eth.iban

\`\`\`js var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK"); \`\`\`

1.  1.  web3.eth.iban.fromAddress

\`\`\`js var i =
web3.eth.iban.fromAddress('0x00c5496aee77c1ba1f0854206a26dda82a81d6d8');
console.log(i.toString()); // 'XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS \`\`\`

1.  1.  web3.eth.iban.fromBban

\`\`\`js var i = web3.eth.iban.fromBban('ETHXREGGAVOFYORK');
console.log(i.toString()); // "XE81ETHXREGGAVOFYORK" \`\`\`

1.  1.  web3.eth.iban.createIndirect

\`\`\`js var i = web3.eth.iban.createIndirect({

` institution: "XREG",`
` identifier: "GAVOFYORK"`

}); console.log(i.toString()); // "XE81ETHXREGGAVOFYORK" \`\`\`

1.  1.  web3.eth.iban.isValid

\`\`\`js var valid = web3.eth.iban.isValid("XE81ETHXREGGAVOFYORK");
console.log(valid); // true

var valid2 = web3.eth.iban.isValid("XE82ETHXREGGAVOFYORK");
console.log(valid2); // false, cause checksum is incorrect

var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK"); var valid3 =
i.isValid(); console.log(valid3); // true

\`\`\`

1.  1.  web3.eth.iban.isDirect

\`\`\`js var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK"); var direct =
i.isDirect(); console.log(direct); // false \`\`\`

1.  1.  web3.eth.iban.isIndirect

\`\`\`js var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK"); var indirect
= i.isIndirect(); console.log(indirect); // true \`\`\`

1.  1.  web3.eth.iban.checksum

\`\`\`js var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK"); var checksum
= i.checksum(); console.log(checksum); // "81" \`\`\`

1.  1.  web3.eth.iban.institution

\`\`\`js var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK"); var
institution = i.institution(); console.log(institution); // 'XREG'
\`\`\`

1.  1.  web3.eth.iban.client

\`\`\`js var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK"); var client =
i.client(); console.log(client); // 'GAVOFYORK' \`\`\`

1.  1.  web3.eth.iban.address

\`\`\`js var i = new
web3.eth.iban('XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS'); var address =
i.address(); console.log(address); //
'00c5496aee77c1ba1f0854206a26dda82a81d6d8' \`\`\`

1.  1.  web3.eth.iban.toString

\`\`\`js var i = new
web3.eth.iban('XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS');
console.log(i.toString()); // 'XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS'
\`\`\`