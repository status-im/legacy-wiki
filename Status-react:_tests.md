This document outlines options to improve our testing strategy.

Reason to test: To provide feedback on the quality state of the Status
builds as quickly as possible in a reproducible manner. (important:
feedback, quickly, reproducible)

Quality state (attributes/dimensions) Functionality: Does it work? Does
it work on specific devices/OS? If it works then other quality
attributes: e.g. Performance; Usability, etc.

Ways to check and what can be covered Unit tests (what is already
working will not be broken by changes) Manual tests, (check new
functionality in PRs, regression and integration/system tests of
development build with limited device coverage, review new UI designs
before implementation) Automated UI tests to cover basic functionality
(quickly get smoke coverage, measure performance, battery/memory/network
consumption, run tests on real devices in the cloud platforms) Community
manual tests (extra device/os/language coverage, testing development
builds, feedback on perf/memory/network usage, review new ui?) Prevent
bugs: review new stories/functionality, ui design and describe tests -\>
before implementing developer knows what will be checked.

## Unit tests

Short and fast tests that validate pure functions. Can be run often.
Based on `cljs.test`. When appropriate `test.check` is a great tool to
find non obvious issues.

Some higher level tests specific to `re-frame` can be written using
`re-frame-test`.

Every PR should have tests for new/updated functions. High coverage is
not the target but basic functionality and limit cases.

## Integration tests

At the other end of the spectrum the shippable application is tested
with more complex user scenario. Those tests will be written in
ClojureScript too to leverage the existing codebase. `Appium` provides a
`nodejs` layer that we can leverage.

## Non-regression tests

It might be useful to introduce performance and upgrade tests to make
sure new releases do not break customer expectations.

## Worth investigating

  - <https://github.com/pixielabs/cavy>
  - <https://github.com/wix/detox>

## Further discussion

TODO: Also import relevant commentary from gist here
<https://gist.github.com/jeluard/4564c04f118b1cfa828ffe741811b0aa>