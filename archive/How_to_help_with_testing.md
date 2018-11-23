You can help with testing Status app in several ways:

1.  Test all features in the nightly develop build (to check new
    functionality and regressions in the old functionality). Report
    found issues
2.  Find and report issues in the stable released versions on iOS or
    Android
3.  Check for the issues with "Help to reproduce" label and if you can
    reproduce - add the extra info, like exact steps, logs, video etc.
4.  Test new features in PR builds that are not yet merged to develop
    build. Report found issues.
5.  Check the Python end-to-end test suite and create new tests.

### Test all features in the nightly develop build

The most useful would be to check nightly develop build on Android. We
expect that features merged in the develop build are stable enough and
this build is free of obvious bugs. This is of course just an assumption
and should be verified. There are end-to-end tests that cover some
common scenario. For the rest we test manually once in 2-3 days, so
anyone who can find and report not-known issue is welcome to the club.
There is an outdated version of the smoke test cases (for version
0.9.10) available
[here](https://docs.google.com/spreadsheets/d/1Nx9JLNhW9SwHBOMgWdT0LPxbB-r6EBoqGQ2AcwEpEIU/edit#gid=572215431)
that we will update soon. It can be used as a base to learn the features
of the app.

Each European night there is a new nightly build created and it’s very
important to take the latest because issues you will find in 2-days old
build might be already fixed in the recent build. [List of the
builds](http://artifacts.status.im:8081/artifactory/nightlies-local/) is
not sorted by the latest date, so to find the latest search by the date,
e.g. “06-dec”

When you report an issue it’s also important to mention that nightly
build X was used. Just add an info about the date and link to the apk
you’ve used in section “Additional Information” of the issue. When
reporting an issue, please follow the guidelines from How to report an
issue

### Find and report issues in the stable released versions on iOS or Android

If you don’t want to install nightly apk build then you can help by
finding and reporting issues in the released Android version available
on PlayStore or in iOS version from TestFlight. This is less useful as
we continue to fix known issues and modify existing areas, so it might
happen that found issue is already fixed or area is being re-designed in
the current develop build. When reporting an issue, please follow the
guidelines from How to report an issue

### Reproduce issues with "Help to reproduce" label and add info

Sometimes we see the issues or get the bug reports but can't find the
exact steps/model/condition to reproduce them. You can search in the
Github repo by label "Help to reproduce" to see the
[list](https://github.com/status-im/status-react/issues?q=is%3Aopen+is%3Aissue+label%3A%22help+to+reproduce%22)
and try to reproduce bugs. If you can then add as much info as possible:
exact steps, logs, video, info about the phone model/OS, anything else
that can help us to reproduce and then to fix a bug.

### Test new features in PR builds

Team is working on providing an access to the PR builds to the external
contributors, including testers. However, PR builds are expected to be
tested faster and thoroughly by someone who is very familiar with the
app. This is what core test team is doing daily: testing PR builds that
introduce changes/fixes/features. So, if you are just starting to help
it would not be the best option to choose.

### Check the Python end-to-end test suite and create new tests

[Setup](Appium_end-to-end_tests_for_Status_app "wikilink") and run
existing automated tests. Review the tests and create new ones in PR.
You might also like to create an issue in Github
[repo](https://github.com/status-im/status-react/issues) marked
\`Autotests\` in title to explain which test you want to automate, so
core test team can verify you have the correct assumptions about
expected results.