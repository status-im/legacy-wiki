# <span class="c2">Proposal: use trunk-based development</span>

<span class="c0">Written by Oskar Thorén on September 16, 2017. Updated
September 23.</span>

<span class="c0"></span>

<span class="c0">We should practice trunk-based development in
status-react and status-go repos.</span>

<span class="c0"></span>

<span class="c0">Trunk-based development (TBD) is a source-control
branching model, where developers collaborate on code in a single branch
called ‘trunk’ (‘develop’ for us), resist any pressure to create other
long-lived development branches by employing documented techniques,
avoid merge hell, do not break the build, and live happily ever
after.</span>

## <span class="c11 c12">Why?</span>

<span class="c0">1) We have a lot of issues related to breaking builds,
long-lived branches that other branches depend on, wasting precious time
on coordinating and doing manual builds from ad hoc branches to patch
things up.</span>

<span class="c0"></span>

<span class="c0">A breaking build on 'develop' (i.e. change that causes
the app to stop working in a major way) should be flaming emergency and
you stop everything to fix it, even if it means reverting
commits.</span>

<span class="c0"></span>

<span class="c0">2) We already have a great QA team, a PR based
workflow, a (semi-)-functioning CI, and feature flags. All of this is a
perfect fit for TBD.</span>

<span class="c0"></span>

<span class="c4">Objection: We merged a few bugs into develop with
changes related to wallet. If we use TBD such problems will increase. If
we merge want this not to happen we have to not merge stuff into develop
before merging, but this makes fast reviews (tens of minutes) not
possible.</span>

<span class="c4"></span>

<span class="c4">Reply: It is OK to push a few bugs under WIP if nothing
breaking like crazy. If it's a big/UX related change, QA can note known
regressions/limitations of things hidden behind flag. Once flag is about
to be turned on for release, you do more extensive checking. You don't
need auto testing for this.</span>

<span class="c0"></span>

<span class="c0">3) TBD encourages shipping code, breaking things into
smaller parts and getting things into trunk/develop as soon as
possible.</span>

<span class="c0"></span>

<span class="c4">Objection: Commit every 24h isn't a good policy because
it doesn't guarantee meaningful smaller parts.</span>

<span class="c4"></span>

<span class="c10">Reply: You don't need "commit every 24h", just if you
open a PR and it hasn't been merged in two days it is most likely (a)
not ready yet and/or (b) too big. Minimal utility can be for other
developers as
well.</span>

## <span class="c11 c12">I thought we kind of did this already; examples?</span>

<span class="c0">- \`status-react\` has never depended on \`status-go\`
develop branch as far as I can tell. This, in addition to lack of CI for
status-go, leads to cascading  effects for status-react.</span>

<span class="c0"></span>

<span class="c4">Objection: I don't see a big problem in this, most
cascading effects come from go-ethereum, which we don't control.</span>

<span class="c4"></span>

<span class="c4">Reply: It (status-go HEAD breaking for a month) can be
solved by only actually doing the upgrade when all breaking changes have
been incorporated. This too can be a flag and one commit. We have wasted
a lot of time on this status-react if you sum it all up I think, and
devs can't use bugfixes like crashing in debugger.</span>

<span class="c4"></span>

<span class="c4">Counter-reply: This will require a lot of coordination
on status-react and status-go side.</span>

<span class="c4"></span>

<span class="c4">What if we have this situation like we have with
\`upgrade-status-go\`, when we can’t merge it because app will be fucked
(now eth.sendTransaction calls are blocking), but you need some changes
from this branch (“like crashing in debugger”, etc). TBD definitely will
not work if we will use it on our side here, i doubt that it will be
possible to choose dependency by flag (and even if so, fix for crushing
debugger will not be available), so then flags should be</span>

<span class="c4">used on status-go side in order to achieve this. So we
still will have a loop between status-react and status-go: they have to
add flag (if possible), only then we can proceed.</span>

<span class="c4"></span>

<span class="c4">And then when it comes to using flags for bugs which
appears after they rebase on new version of go-ethereum and something is
not working, i hardly imagine how this is doable. I mean flag which will
choose between pieces of code before and after rebasing, it can be
impossible to separate these pieces at all.</span>

<span class="c4"></span>

<span class="c4">Counter-counter-reply: We can use branch by abstraction
as well <https://trunkbaseddevelopment.com/branch-by-abstraction/> and
possibly have the two in parallel. I guess it's a matter of extensive QA
on the upgrade-go-ethereum short lived branch and push bugfixes _to
trunk_. Google does TBD and has a single repo, and I'm guessing they
have worse dependency issues than we do, so it's not impossible. Then
it's a question whether that is _worth it_ and maybe we should have a
basic post mortem on what went wrong and how we can avoid this in
future. But it's certainly doable if we think it is worth it. That
doesn't mean it is definitely worth it in every possible circumstance
for our org/code base etc, but we should make trade-off explicit.</span>

<span class="c0"></span>

<span class="c0">- Long-lived PRs: 5+ open PRs that are older than a
week in both repos.</span>

<span class="c0"></span>

<span class="c4">Objection: On the other hand we keep not finished, not
completely meaningful piece of code outside codebase. Splitting things
in meaningful parts is not always straightforward.</span>

<span class="c4"></span>

<span class="c4">Reply: It's OK to keep WIP work in code base, it makes
it easier for someone to take over too. Of course it should always have
some utility to someone, not just random stuff. Utility can be for other
developers (yourself in future or other team members) too.</span>

<span class="c0"></span>

<span class="c0">- Long-lived branches: We use ‘epic/wallet’ when QA
flow and flag are already in place.</span>

<span class="c0"></span>

<span class="c4">Objection: We don't want WIP commits to go into develop
before QA, so we use epic branch to decrease number of testing
iterations.</span>

<span class="c4"></span>

<span class="c4">Reply: We can decrease number of testing iterations
just using flags. This is the exact same thing for QA especially if
everyone is running trunk and notice fatal errors immediately. The exact
involvement of QA for each WIP commit is up to QA and each PR.</span>

<span class="c0"></span>

<span class="c0">This is not any individual’s fault; it is a systemic
issue stemming from a lack of coherent vision and application of how we
do branching. Will there be issues? Sure, but nothing that can’t be
solved with disciplined application of known practices.</span>

## <span class="c11 c12">What would this mean in practice for us?</span>

<span class="c0">- Ensure that \`develop\` is always working (tools we
have: CI, flags, incremental changes).</span>

<span class="c0"></span>

<span class="c4">Objection: Develop is fine, we also will need to be
able to build different versions of develop afaiu (based on
flags).</span>

<span class="c4"></span>

<span class="c4">Reply: Possibly. To start with all flags should be on
by default for nightlies, then for release builds almost all should be
off (depends on scope of release).</span>

<span class="c0"></span>

<span class="c0">- Be aggressive about not allowing long-lived branches
or PRs or weird branch dependencies.</span>

<span class="c0"></span>

<span class="c4">Comment on weird dependencies: we have several PRs
waiting for status-go upgrade, the PN one being biggest offender since
it is against a PR that was first opened a month ago and it requires a
lot of coordination despite not being directly tied to it, if breaking
things were hidden behind flag in status-go then this commit could've
happened anytime and PN branch would have debugger fix and PN public API
available to consume.</span>

<span class="c0"></span>

<span class="c0">- For releases, don’t do code freeze, instead
short-lived release branches + cherry pick from develop.</span>

<span class="c0"></span>

<span class="c4">Comment on releases: This should only be the concern of
the people actively working on creating release artifact, testing and
fixing regression. It has little impact on other devs WIP on develop.
</span>

<span class="c0"></span>

<span class="c15">For more details on TBD, read here:
</span><span class="c13 c15">[<https://trunkbaseddevelopment.com/>](https://www.google.com/url?q=https://trunkbaseddevelopment.com/&sa=D&ust=1507366105390000&usg=AFQjCNEm3ZdxQRxb4P3FHLexY1Corp8oPw)</span>

## <span class="c11 c12"></span>

<span class="c0"></span>

-----

## <span class="c11 c12"></span>

## <span class="c11 c12">General comments</span>

<span class="c4">Comment: I see serious advantages in this approach,
because actually we have suffered from epic branches. Though still some
drawbacks \[captured in objections\]</span>

<span class="c4"></span>

<span class="c11 c7">Objection: Potential flag-based mess in
code.</span>

<span class="c4"></span>

<span class="c4">Reply: Yes we should be aggressive in deleting this,
can put 'best before' dates on branches - this requires some discipline
for sure. Also should get rid of flags as soon as they are in
release.</span>

<span class="c4"></span>

<span class="c11 c7">Objection: Resolving conflicts, this thing can’t
disappear.</span>

<span class="c4"></span>

<span class="c4">Reply: Why? with short lived PRs everyone will be more
up to date to trunk-state of the world, the distance between developers
will be shorter and deltas smaller. See for example that huge
refactoring PR, and a few things that depend on it and have either been
merged beforehand or after, both requiring extra conflict resolution
because of lack of shared work.</span>

<span class="c4"></span>

<span class="c7 c11">Objection: Coordination between status-go and
status-react.</span>

<span class="c4"></span>

<span class="c4">Reply: maybe but I'm not sure this is true if we do it
right (perhaps it is too expensive to do it right). In Rich Hickey's
words, "I don't want to know" - status-react should be able to depend on
status-go develop/HEAD (possibly last 'good commit' if bug is
introduced) and then new _capabilities_ are introduced in status-go.
Things being breaking changes can exists concurrently and then be
switched off, like deprecation notice kind of. I really like Rich
Hickey's philosophy of 'change' not being a useful word, either you
accrete, add to something and _provide_ more, or you break something
by introducing a bug or _requiring_ more. I think this lesson can be
applied outside of a Clojure ecosystem, even with crazy stuff like
go-ethereum vendoring, but it requires discipline</span>

<span class="c4"></span>

<span class="c4">There is a general point though that you hint at: a lot
of complexity status-go side are things I'm blissfully unaware of, so
the trade-off discussion there is better had there. We can still do
things on status-react side, though.</span>

<span class="c4"></span>

<span class="c7">Objection: Also we will need to redefine manual testing
process,</span><span class="c4"> so imagine task like wallet. If we add
commits with not finished features to trunk, we will add bugs for sure
(especially if new changes touch older code), so we still need to test
wallet related stuff periodically, we can’t wait for 100%
implementation. So then QA folks should be able to build develop with
flags they choose. It should be relatively easily doable with jenkins.
But then when it comes to flags existing in both status-react and
status-go, or just in status-go, it means that they have to build
status-go first with these flags and then link it with status-react,
only then build app. So we need to provide such tools. Also the list of
flags should be defined somewhere and well explained.</span>

<span class="c4"></span>

<span class="c4">Reply: QA can already test WIP wallet stuff with
Jenkins PR, no? It is true we might want something like develop with WIP
and without WIP, at least as we get closer to release. I don't know
about flags in status-go or why this would directly impact status-react,
but maybe I'm missing something. Agreed regarding list of being well
defined.</span>

<span class="c4"></span>

<span class="c4">Concrete example from stuff I'm working on now (PR:
<https://github.com/status-im/status-react/pull/1872>) in terms of
splitting work: I can submit PR that targets develop, doesn't have iOS
bindings/ mock call \`Notify\` and prints fcm-token and payload. Then
tester instruction is similar but deliverable is: add contact and see
their fcm-token in Testfairy logs, download logs and do \`grep
XXX\`.</span>

<span class="c4"></span>

<span class="c4">Then next PR is dependent on status-go public API
changes, ideally this would be available for consumption straightaway
when API is updated in trunk but if not, wait until this is provided by
status-go and then submit iOS/Java bindings. Possibly short-lived
minimal branch for QA to test flow that binds to specific unsupported
status-go branch, but this is where you maybe get some cascading
effects.</span>

<span class="c4"></span>

<span class="c4">Counter-reply:  We either have to always have already
compiled all invariants of flags for status-go (if don’t use only one
WIP flag), or to compile status-go as a step of status-react’s
build.</span>

<span class="c4"></span>

<span class="c4">The first case: There is some new feature in
status-react, like in that \`upgrade-status-go\` branch. Branch contains
changes related to new Whisper v5 api, and we definitely can abstract
this behind some protocol in cljs. So, we do this, test it with
status-go and find out that there is a breaking bug on status-go side.
We would commit this code to status-react’s trunk (it makes sense in
TBD, feature is not finished but we hide it anyway and don’t wait too
long for committing changes), but we can’t hide status-go related stuff
using the same flag, we need to add another flag in status-go first
which will hide new Whisper v5 api and provide v2 instead.</span>

<span class="c4"></span>

<span class="c4">The second case is similar, but related to that
go-ethereum rebasing thing: what if breaking change can’t be hidden by
flag at all. So we have short living branch in status-react, and we can
hide that feature but not status-go’s version on which it
depends.</span>

<span class="c4"></span>

<span class="c7">Objection: Seems obvious, the only reason we have
long-lived branches is due to being a new
team.</span><span class="c4"> I even wrote something like that before:
<https://hackmd.io/s/rkgqzRWIb>. I'm currently talking about status-go
as I've never had a chance to work to status-react to some
representative degree. I mean all our developers are middle to strong
but the team is weak just because we've just started working together on
the project nobody knows. This makes us do weird things and keep
long-lived branches.</span>

<span class="c4"></span>

<span class="c4">Reply: It might seem obvious yes, but we are not doing
it in practice. First step is getting everyone on the same page, I
think.</span>

<span class="c4"></span>

<span class="c4">Counter-reply: I thought everybody's already been on
the same page as I've already shared that document. The only thing
missing now is more aggressive approach to this requirement.</span>

<span class="c4"></span>

<span class="c7">Objection: Changes to stabilise everything shouldn't be
made at once,</span><span class="c4"> it's more flowing and gradual
process. Currently, everybody is occupied with the release and sees
broken and unreliable tests. Also, refactoring has come into play
recently. Once we have room to think about something else, we'll start
doing it. For example, everybody sees a lot of long-lived pull requests
and everybody understands that it shouldn't be so but we already have a
lot of more actual problems to tackle. From the status-go side we
already have more important things to tackle right now.</span>

<span class="c4"></span>

<span class="c4">Reply: As soon as release is out, I don't see why
status-react can't just depend on status-go develop straight away (even
if just a fixed commit). What downside would that have?</span>

<span class="c4"></span>

<span class="c4">Counter-reply: It can. status-react should just say: we
now depend on \`develop\`. Please, guys, keep it stable. This
requirement for us is already a good start.</span>

<span class="c4"></span>

<span class="c7">Objection: Let's start from status-react. Because these
problems are not so critical now for
status-go.</span><span class="c4"> They'll get more priority once we
have proper tests in place and the requirement from status-react to keep
status-go \`develop\` stable. Can open issue.</span>

<span class="c4"></span>

<span class="c10">Reply: OK.
</span><span class="c10 c13">[<https://github.com/status-im/status-go/issues/356>](https://www.google.com/url?q=https://github.com/status-im/status-go/issues/356&sa=D&ust=1507366105396000&usg=AFQjCNGtT8SN6Cm3tvdfddr3L6cYDSv3eA)</span><span class="c10"> 
Then I guess the other thing is about long lived PRs, but that's more
team-specific.</span>