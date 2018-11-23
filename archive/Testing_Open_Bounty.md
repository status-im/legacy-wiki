1.  Testing Open Bounty

We have a continuously deployed version tracking the \`develop\` branch
live at <https://openbounty.status.im:444>. It uses the
\[Ropsten\](https://ropsten.io/) Ethereum testnet. Any one is welcome to
use it and your help with testing Open Bounty is greatly appreciated\!

1.  1.  General

For testing you will need:

  - a web browser (Chrome is known to work, testing with others
    appreciated)
  - an Ethereum account on the Ropsten testnet
  - a Github account with administrative access to one or more
    repositories
  - for approving bounty payouts you will additionally need access to an
    Ethereum wallet. So far, Mist and \[MetaMask\](https://metamask.io/)
    have been used, but anything that provides the web3 javascript
    interface should work.

The developers can be reached on the \`\#openbounty\` channel in the
\[Status slack\](http://slack.status.im/).

1.  1.  Signing up

<!-- end list -->

  - point your browser to <https://openbounty.status.im:444> and click
    \`Login\`
  - Authorise status-open-bounty to have read access to your public
    GitHub profile.

You should now see \`Bounties\`, \`Activity\`, \`Repositories\` and
\`Manage Payouts\` tabs. In the upper right hand corner, there should be
a dropdown with your GitHub username and options \`My Payment Details\`
and \`Sign Out\`.

1.  1.  Connecting your wallet

(instructions for Metamask)

  - install Metamask and configure your account
  - select \`My Payment Details\` from the top-right dropdown, select
    the account you want to use from the selection list and click
    \`Update\`

<!-- end list -->

1.  1.  Creating bounty issues

Before you can create bounties, you need to add Open Bounty GitHub App
to your account or repos. Go to
<https://github.com/apps/status-open-bounty-app-test> (or link to
another GitHub App you've created for testing, as described in the
\[README\](README.md) and click Install. Specify whether access to all
org repos or specific repos is granted. This will install webhooks for
SOB in your repos.

  - Request for your account to be whitelisted. Contact
    \[Riot\](https://chat.status.im) for more information
  - now, add the \`bounty\` label to a new or an existing issue. This
    should cause Status Open Bounty to post a new comment for the issue
    containing an image with text \`Deploying contract, please wait\`
  - once the contract has been mined, the comment will be updated to
    contain the bounty contract's address and a QR code

<!-- end list -->

1.  1.  Funding bounties

The Github comment has a QR code as an image containing the bounty
contract address. The address is also on the comment as text. Use any
ethereum wallet to send ETH and/or supported ERC20 tokens to this
address. After a small delay (max 5 minutes), the activity feed should
show that the related bounty issue's balance increased and comment
should be updated.

1.  1.  Submitting claims

To get bounties you need to provide an Ethereum address in you Payment
details on the <https://openbounty.status.im:444> that will be used to
send bounties to.

Open a pull request against the target repository with \`Fixes: \#NN\`
in the comment where \`NN\` is the issue number of the bountied Github
issue. After the PR has been opened, the activity feed should show an
item indicating that your username has opened a claim for the related
bounty issue. The repository admin should also see the claim under
\`Open claims\` in the \`Manage payouts\` view.

1.  1.  Managing payouts

Repository admins see a listing of all open claims and bounties that
have already been paid out on the \`Manage Payouts\` tab. The \`open
claims\` listing includes unmerged claim pull requests and merged pull
requests. Once a claim pull request has been merged, it is selected as
the winning claim. The repository admin will still need to sign off the
payout with his connected Ethereum wallet. This is done with the
\`Confirm\` button. Once the payout transaction has been mined, the
\`Activity\` feed view will show that the claimer received the bounty
funds. All tokens and ETH will be transferred to the claimer's Ethereum
address.

1.  1.  Removing bounties

To remove issue from the Bounties list you can close it in GitHub.

1.  1.  Reporting bugs

All bugs should be reported as issues in the \[OpenBounty Github
repository\](https://github.com/status-im/open-bounty/issues).

Please first check that there is not already a duplicate issue. Issues
should contain exact and minimal step-by-step instructions for
reproducing the problem.

1.  1.  Status Open Bounty end-to-end tests

Framework for testing located in: \`open-bounty/test/end-to-end\`

Full installation and configuring manual: \[Status Open Bounty
end-to-end
tests\](https://wiki.status.im/Status_Open_Bounty_end-to-end_tests)

Currently supports local and Jenkins environment running (you can find
example of JenkinsFile in \`open-bounty/test\` )