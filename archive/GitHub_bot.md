### Motivation

Managing the Pull Request workflow in GitHub has become more and more of
a burden for the team, and it is easy for things to slip through the
cracks. Think moving a PR to testing state when: at least 2 reviewers
approve it, all automated checks pass, and there are no pending request
for changes. This is a type of task is much more suited for a bot. That
is why we started using [probot](https://probot.github.io/), and will be
expanding its functionalities as needs arise.

### Implementation

Probot is implemented as a [Node.js](https://nodejs.org/) package, and
each Probot app can be deployed as a standalone [GitHub
App](https://developer.github.com/apps/), or as a plugin in another
Probot app. Our Probot app implements some status.im-specific
functionality in the
[scripts](https://github.com/status-im/status-github-bot/tree/master/scripts)
folder, and imports some community plugins in
[package.json](https://github.com/status-im/status-github-bot/blob/master/package.json#L35),
under the **probot/plugins** section.

Where it makes sense, we may just deploy a community probot app instead
of importing it into our own bot. In that case, those bots are hosted by
their owners.

The repository is located
[here](https://github.com/status-im/status-github-bot), and the app is
currently hosted on Heroku.

### Implementing a new script

#### Prerequisites

You'll need [Node.js](https://nodejs.org/en/download/) (version 7.6 or
higher).

1.  Clone the repo and follow instructions in the [README.md
    file](https://github.com/status-im/status-github-bot#creating-the-bot-github-app)
    to how to create a test bot on your account. You can also check the
    [probot
    documentation](https://probot.github.io/docs/development/#configure-a-github-app)
    for more information.
2.  Add a file for your script under the **scripts** project folder.
3.  Add a **require()** call for your newly-created script in
    [index.js](https://github.com/status-im/status-github-bot/blob/master/index.js).

#### Guidelines

1.  Make it easy to test your script by checking the **DRY_RUN**
    environment variable, and only executing write actions if the
    variable is not defined.
2.  You are welcome to add a check for an environment variable that is
    specific to your script (e.g. **DRY_RUN_MY_SCRIPT_NAME**), if it
    does non-trivial actions. That'll make it easier to test just before
    deploying to production.
3.  Add a header to the script file mentioning a description, npm
    dependencies, environment variables, and author name (refer to
    existing scripts for examples)

### Importing a community script

1.  Install the bot package using npm (e.g. **npm install --save
    probot-gpg**)
2.  Reference the package under the **probot/plugins** section of
    **package.json**.
3.  Check the plugin documentation regarding any setting files that need
    to be added to the target repositories, environment variables, etc.