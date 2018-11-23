## Intro

As part of <https://github.com/status-im/ideas/issues/64> we want to
document how we do releases and spread knowledge for this.

## Release cutting/mangement process

\- \[ \] Branch of develop 1-2w before release - \[ \] Create PR
something like this:
<https://github.com/status-im/status-react/pull/2919> - \[ \]
(Continuous) Cherry pick bug fixes and only critical bug fixes _from_
develop to release branch - \[ \] Use \`script/tag-release.sh 0.9.13\`
script to tag each cherry-pick for accurate build info - \[ \]
Communicate with QA and devs to ensure all critical bugs are caught and
fixed - \[ \] Make sure TF is off in release build for final RC - \[ \]
Upload binaries for both Android/iOS - \[ \] Fill out release notes from
comms team (short version) in description

## Testing process

TBD. Anna?

## Release note / comms process

TBD. Chris?

## Access

As a release cutter, make sure you have access to: - \[ \] iTunesConnect
- \[ \] Google Play Store - \[ \] Build system with proper certificates
for both platforms (Jenkins, local, Fastlane, ~TBD)

## Knowledge

\- How to answer iTunesConnect questions and submit build. TODO:
Document here (yes to encryption and exemption, IIRC - easiest option
that is true). - How to submit build to Play Store