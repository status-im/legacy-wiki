## How to bump status-go from status-react side?

For Core Contributors.

1\. Find the version of status-go you want to use. For
\`69276386d205293709856919ea2e80f2bc17755c\` do \`git describe
69276386d205293709856919ea2e80f2bc17755c\` which gives you back
\`0.9.7-175-g69276386\`. The version format here is
\`<branch>-<g7SHA>\`. So in this case the version is
\`develop-g6927638\`.

2\. Grep for \`status-go\` in \`status-react\` code base and notice
there are two places this version number has to be changed. Do this and
prepare a PR like:
<https://github.com/status-im/status-react/pull/2514/files> - the PR
should have minimal changes in it to ensure proper testing scope.

3\. To check for breaking changes you can generate an ad hoc CHANGELOG
(because we do rolling releases) like this: \`git log --pretty=oneline |
grep ad9a8777a89562f56027b7c859faa4407a9097c6 -B 100\` (optionally \`|
pbcopy\` at end). This gives you a list of changes from status-react
POV. Hopefully there aren't any breaking changes that haven't already
been communicated properly from status-go side, but sometimes things
fall between cracks.

4\. Optionally, look in Artifactory to see if artifact is there
<http://139.162.11.12:8081/artifactory/webapp/#/artifacts/browse/tree/General/libs-release-local/status-im/status-go>.
You can see how status-go deploys here:
<https://www.notion.so/status/Deployment-8264c0dcb58045afa7001823d647b15b>

5\. Ping TestTeam and Status-go to check for hotspots in terms of what
areas of behaviors are impacted. Give Testteam time to test this PR as
surface area is generally quite big and regressions can come up that
takes a lot of time to fix.