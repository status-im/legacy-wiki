# Write a chat command

One of the main use case for extensions is to add new chat commands.
This tutorial will go step by step through the creation and deployment
of a chat command allowing to send NFTs.

## Add meta data

First step is to create the skeleton extension with some relevant
metadata:

Metadata will be displayed to the end user before installing an
extension.

## Define hook entry point (chat command)

Hooks identify what part of status will be extended. Each hook has a
unique identifier and a set of key/value elements specific to this hook.

An extension can implement several hooks.

In this tutorial a chat command is created: it’s specific id
is `collectible` and the generic hook type for a chat command
is `commands`.

A `command` hook requires the following properties to be set:

  - scope
  - preview and short-preview
  - parameters

### Scope

Scope can be any combination of:

  - personal-chats
  - group-chats
  - public-chats

Here we will demonstrate `personal-chats`.

### Previews

`Previews` are used to display the result of a command execution in a
chat.

`Short previews` will be displayed as last message in the chat item of
the Home tab of Status.

Both previews must point to definition of UI in [Hiccup
syntax](https://github.com/weavejester/hiccup/wiki/Syntax) using a
combination of views, queries and events supported by status host.

More details can be
found [here](https://status-im.github.io/pluto/docs/concepts/Anatomy.html).

Previews receive data from status encapsulating the parameters provided
by the end user and some relevant contextual information. Those can be
accessed in a view using the `properties` reference.

Our short preview definition:

`properties` data is accessed
using [destructuring](https://status-im.github.io/pluto/docs/concepts/View.html#destructuring).

`text` and `view` are view elements available for all hosts.

`if` is a block providing conditional logic.

Our preview definition:

`status/nft-token` and `status/send-status` are view element specific to
status.

`[status/get-collectible-token {:symbol symbol :token token}]` is a
query giving access to details for a specific collectible.

### Parameters

The NFT chat command has 2 required parameters: the NFT type and the
specific NFT you want to exchange.

Both will use Status UI components to provide a nice visual selection
experience.

A parameter is identified by its `id` and must define a `type` and
a `placeholder` (any string).

In this tutorial `:text` and `:number` will be used.

`suggestions` can be optionally provided and must point to a `view`.

## Deploy

Extensions are identified by a URI and can be loaded in status via a
universal link.

Currently only GitHub gist is supported as provider.

A universal link pointing to an extension would then look
like: `https://get.status.im/extension/gist@janherich/92747e730b2e115bcbe145114d024e66`

## Use

The simplest option is to scan a QR code pointing to your extension. You
can also navigate to status user profile and open the `Extensions`item
in the `Advanced` section. This option is only available in developer
mode.

Once loaded, details about an extension are available. An extension can
then be installed. Once installed all hooks are `active`. Any extension
can then be `deactivated` or re-`activated`. Associated hooks will then
be removed/added from Status.

You can now use your new extension from within a 1-1 chat\!

## Full extension code