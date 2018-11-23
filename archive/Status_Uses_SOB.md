## Our Internal Process for Maximising SOB's Potential

### Who We Target

**Contributors**: get paid for merged Pull Requests in any open source
GitHub repo. Payments are in ETH or ERC20 tokens like SNT.

**Organisations**: attract more talented developers who will fix bugs or
implement new features faster by paying in ETH or ERC20 tokens for the
merged Pull Requests in your GitHub repositories.

### How We Create Great Issues

It's important to set up a repeatable and transparent process for
everyone in your organization to understand what kinds of issues are
best to put up for bounties and how to create issues that communicate
succinctly and exactly what the fix or feature required is, how to scope
it correctly and assign an appropriate bounty, and how to manage testing
and review of any Pull Requests from external contributors so that they
are rewarded timeously for doing the work and encourage to keep on
contributing to your project.

  - Developer creates an issue she/he would like to see fixed using the
    same basic template we use for all our issues.
  - We use ZenHub to make boards that our developers can follow tasks on
    easily. We want to make sure that any of the core developers can
    easily suggest an issue they'd like to see fixed soon for bounty, so
    have created a 'Ready For Bounty' column in both status-react and
    status-go so that all devs can see which issues are bountyable and
    can move their own issues into that column so others can see it and
    agree if it is suited to a bounty.

![Bounty-check.png](Bounty-check.png "Bounty-check.png")

  - After the issue is created and placed in ZenHub, we require the
    developer who files it to specify what they think the scope is, and
    what - therefore - the reward should be. They can indicate this
    easily by attaching the bounty-S, bounty-M, bounty-L, or bounty-XL
    label to the issue.

![Bounty-labels.png](Bounty-labels.png "Bounty-labels.png")

  - Once they are happy the issue is well-described and scoped, they can
    add the bounty-awaiting-approval label, which will alert the
    Community and QA teams, who watch the repositories for this.
    Generally, at this point, it is also best for the developers
    responsible to ping the Swarm that runs bounties in our closed Slack
    in the channel \#10-ob-bounties.
  - We then have designated developers in both teams (status-react and
    status-go) to help our Community and QA team to ensure that the
    issue description is accurate, properly scoped and that we can
    review and test any potential fix that comes through easily and
    quickly.
  - The team lead (Anna) then adds a bounty label to the issue(s). As a
    result:

<!-- end list -->

1.  a new comment is added to the issue with an Ethereum contract
    address and **0 ETH** and **no tokens** on it.
2.  issue is listed with **0 ETH** and **no tokens** on Open Bounty
    website.

<!-- end list -->

  - Organisation treasurer sends SNT to the contract address. As a
    result, both GH comment on the issue and info on the Open Bounty
    website are updated with a new price, e.g. 2000 SNT. We can then
    direct people to Open Bounty to look for the latest and greatest
    issues to fix.

### **Status-React Issue Requirements**

Issue well-suited for bounty should satisfy these conditions:

1.  We are ok to wait undefined time for the PR (We will be implementing
    a time-constraint bounty option in the near future)
2.  It's described in a good enough way, so external contributor can
    understand without needing to much prior knowledge of our specific
    codebase.
3.  If it's a bug, then there need to be clear steps to reproduce (both
    actual and expected results). In case of status-react project -
    there is a link to TestFairy session with logs/video
4.  We have clear acceptance criteria: how do we check that it's done?
5.  We can estimate its complexity in XS, S, M, L or XL style (XS - tiny
    issue to fix, XL - lots of research and work to be done). Most of
    the time we will use S, M and L labels.
6.  At least 2 more team members agree we need it fixed or implemented.
    Comment or +1 in the issue from extra 2 team members are OK.
7.  As soon as it's done you can just wait or ping in the Slack channel
    \#10-ob-bounties with issue number. As a result, Anna or someone
    else from the bounty curation swarm (see
    [here](https://github.com/status-im/ideas/issues/10)) will add a
    label and send SNT tokens to it.
8.  Please, do not add label "bounty" on your own - it will result in 0
    bounty shown to the contributors. The bounty curation swarm will
    make sure that issue is listed and gets SNT almost in the same time.

### How We Handle and Test Submissions

Just as important as setting up a repeatable and transparent process for
everyone in your organization to understand what kinds of issues are
best suited to bounties, it's important to have a repeatable and
transparent process by which you handle any Pull Requests that are
submitted, test them quickly, suggest further fixes to contributors and
generally be responsive in order to keep them coming back for more. Our
aim is to incentivise open source contributions through both financial
rewards, as well as through being responsive, helpful, kind and
generally welcoming.

  - First, a basic review of the PR is done. In addition to what the
    issue description expects, make sure that PR description or title
    contains "Fixes \#NN" - this is how Open Bounty detects the claims
    for the bounty. Sometimes, this can be automated, as with our
    [translation tool](https://github.com/yenda/translation-checker),
    but mostly this is fairly manual still. If the PR does not contain
    the expected "Fixes \#NN" comment it will not be considered as a
    claim, so the developer will not receive payment if you merge as it
    is. *FYI: All detected claims are shown on the Open Bounty website
    in the Activity feed.*
  - If it's status-react project and it's a bug fix or functional
    feature: testers should get the build and verify the bug fix or new
    functionality on both Android/iOS before PR is merged.
  - *Still awaiting details on status-go testing/QA requirements.*
  - If the PR is good, then merge it. As a result these changes will
    happen:

<!-- end list -->

1.  For the repository owner on the Open Bounty website -\> Manage
    Payouts. You will see that the button "Confirm" is active to confirm
    the payment
2.  Open Bounty website shows that developer A won X USD for issue Y (on
    Activity feed) and status is "Pending maintainer confirmation"
3.  Issue is not listed anymore on Bounties feed
4.  GH issue comment also shows that developer A is a winner
5.  Repository owner confirms the payment on Open Bounty website -\>
    Manage Payouts by clicking on Confirm button and confirming 0 ETH
    transaction to contract address. As a result, ETH or tokens are sent
    to the winner's Ethereum address from the contract.