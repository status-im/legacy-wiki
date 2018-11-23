This document describes approach to labeling Github issues in
[Status.im](http://status.im/) repositories `status-react` and
`status-go`.

# **Labels**

## **Issue types**

`bug` - label for bug reports

`Epic` - epic issues that help to group the issues related to a
high-level feature or requirement

`test` - issues related to creating or updating the code tests

`story` - user stories describe particular features of flows

## **Priorities**

`high-priority`, `medium-priority`, `low-priority` - help to prioritize
issues and focus on the top priority first.

## **Complexity**

`beginner` - the issue that can be resolved by the new comer developers
and does not require total understanding of the existing codebase and
dependencies.

`advanced` - the issue requires expert level of understanding the
existing code and its dependencies, may involve some research, and the
teamwork.

`intermediate` - something in between the `beginner` and `advanced` :)

## **Platform**

`ios` - iOS specific issues`android` - Android specific issues

## **Process**

`wontfix` - issues that the team has considered and not going to fix in
the near future.

`design` - issues that require approved design before starting the
implementation.

The design label can be extended to `design-todo`, `design-in-progress`
(or `design-wip`?) and `design-approved` to help tracking the design
progress and plan the implementation and testing.

`bounty` - tbd

`blocked` - the issue is known as blocked by the other issue(s), not
necessarily explicitly linked, so it’s recommended to checkout the
comments that should spotlight the problem.

`blocker` - the issue is known as blocker for the other issue(s), not
necessarily explicitly linked, so it’s recommended to checkout the
comments that should spotlight the problem.

`workflow` - the issue is related to the process or workflow that is
used by the team internally and it’s neither a product feature nor bug
in the app, e.g. a release process improvement.

`devops` - the issue is related to the development operations that is
used by the team to maintain the product or process, e.g. a Jenkins job
creation.

## **Testing**

In addition to `bug` and `test` labels described above there are some
more related to testing:

`help to reproduce` - the issue has sporadic nature and there’s a need
to involve community for helping reproduce the bug.

`help needed` - needed community efforts to help understand or verify
the issue.

## **Combining labels**

Labels can be combined for better classification of the issues, e.g.:

`bug high-priority android` - would clearly classify a high priority bug
that is specific to Android platform

`Epic high-priority` - the epic issue for the high priority feature that
is critical to implement as soon as possible.

`advanced ios` - iOS specific feature that requires team efforts and
research for finding solution.