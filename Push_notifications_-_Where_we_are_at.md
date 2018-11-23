# <span class="c16">Push notifications: where we are at</span>

<span class="c1">Written September 22th by Oskar Thorén. Updated with
addendum October 2 and October 3.</span>

## <span class="c5">Goal of this document</span>

<span class="c1">Make sure people are on the same page when it comes to
current state of push notifications, what limitations are, who is
working on it, and most importantly to decide on priority to work on for
the next week-ish. Which in practice approximately means what we’ll
deliver for devcon3 \[update: see addendums at end of document\].</span>

## <span class="c5">Context</span>

<span class="c1">Going to quote what I wrote in the initial push
notification proposal document \[1\] as it is just as relevant now as
then:</span>

<span class="c1"></span>

<span class="c13">Work under the name of “push notifications” has been a
work in progress for six months as far as I can tell. There are many
reasons why it hasn’t been implemented yet, but one of the reasons is
that it hasn’t been scoped properly. By the power law of projects
(</span><span class="c9 c13">[<https://www.johndcook.com/blog/2015/12/21/power-law-projects/>](https://www.google.com/url?q=https://www.johndcook.com/blog/2015/12/21/power-law-projects/&sa=D&ust=1507288643409000&usg=AFQjCNHy_U6PlJ5ZYEcpyRTv131bj92GMA)</span><span class="c13">),
it is expected to take another six months at current pace.
</span><span class="c13">The only way to stop that from happening is to
aggressively cut
scope</span><sup>[\[a](#cmnt1 "wikilink")\]</sup><span class="c13 c15">,
break things down into their essence and then deliver on those.</span>

<span class="c1"></span>

<span class="c1">Since then you can add another 50ish days to the
above.</span>

## <span class="c5">Current state</span>

<span>On our nightly builds a MVP of push notifications has been
implemented \[2\]. </span><span>This feature is hidden behind
flags</span><sup>[\[b](#cmnt2 "wikilink")\]</sup><span>, and
</span><span>it has not been included in 0.9.11 release
candidates</span><sup>[\[c](#cmnt3 "wikilink")\][\[d](#cmnt4 "wikilink")\]</sup><span class="c1">.</span>

<span class="c1"></span>

<span>The MVP solves this specific user story:
</span><span class="c13 c17">As a user, I want to receive a push
notification alert when a person in my contacts messages me and I have
the app in background, so that I don't have to have the app in the
foreground all the
time.</span>

### <span>Limitations of the current design and implementation</span><sup>[\[e](#cmnt5 "wikilink")\]</sup>

  - <span class="c1">L1 UX: It only works in 1on1 chat</span>

<!-- end list -->

  - <span class="c1">L2 UX: You have to have the person added as a
    contact</span>
  - <span class="c1">L3 UX: It only works for text messages, not
    commands (like /send, /location, etc) \[3\]</span>
  - <span>L4 UX: Notification doesn’t include sender or message, so
    </span><span>message might be
    lost</span><sup>[\[f](#cmnt6 "wikilink")\][\[g](#cmnt7 "wikilink")\]</sup>
  - <span class="c1">L5 Technical/3rd-party risk: It depends on Firebase
    and is tightly coupled with it</span>
  - <span class="c1">L6 Technical/General security: Firebase SERVER_KEY
    is included in the binary \[4\]</span>
  - <span class="c1">L7 Technical/Spam: User A can trigger unlimited
    notifications at B</span>
  - <span>L8 Technical/Dark routing: User A has FCMToken of B, and
    </span><span>Firebase knows A and
    B</span><sup>[\[h](#cmnt8 "wikilink")\][\[i](#cmnt9 "wikilink")\]</sup>
  - <span>L9 Technical/Tracking: </span><span>Firebase
    Analytics</span><sup>[\[j](#cmnt10 "wikilink")\][\[k](#cmnt11 "wikilink")\]</sup><span class="c1"> is
    included in in react-native-fcm \[5\]</span>
  - <span class="c1">L10 UX\&Technical: Notifications are sent to a
    device, not account</span>

## <span class="c5">Resources</span>

<span>As Ricardo is leaving Status, currently Oskar is the only one
working on it. As far as I see it, there’s room for </span><span>one
more person on the Clojure side and one more person on the Go
side</span><sup>[\[l](#cmnt12 "wikilink")\][\[m](#cmnt13 "wikilink")\][\[n](#cmnt14 "wikilink")\][\[o](#cmnt15 "wikilink")\][\[p](#cmnt16 "wikilink")\]</sup><span class="c1">,
in terms of work there is to be done. Depending on priority, I could
keep working on it alone (Clojure & Go) but then it’ll obviously take
longer / fewer things can be done in parallel.</span>

## <span class="c5">Options</span>

<span class="c1">Here are the main strands of work as I see it. These
can obviously done in parallel if we have multiple people working on
this.</span>

### <span class="c14">A) Include payload hidden in message (L4)</span>

<span class="c1">This requires:</span>

  - <span>Androi</span><span class="c1">d: get data payload when
    resuming app from bg (status-go changes) \[6, 7\]</span>
  - <span class="c1">Extend status-go API to take notification and data
    payloads</span>
  - <span class="c1">Figure out routing from open app to adding payload
    to relevant chat db</span>
  - <span class="c1">When sending notification, include necessary info
    add payload to relevant chat db</span>
  - <span class="c1">Encrypting notification with key receiver (contact)
    can decrypt upon opening app</span>

<span class="c1"></span>

<span class="c1">Main drawback:</span>

  - <span class="c1">If you send 10 messages, each notification alert
    has to be clicked on to see it in app</span>

<span class="c1"></span>

<span class="c1">Main alternative:</span>

  - <span class="c1">Leave it as it is and use offline inboxing to
    retrieve messages when user opens app</span>

### <span class="c14">B) Increase basic UX capabilities (L1-L3)</span>

<span class="c1">Address UX weaknesses. Combating L1 requires figuring
out how to exchange FCMToken among many participant, extending status-go
public API, and it also increases L8
risk.</span>

### <span class="c14">C) Use something like a notification service (L7 and L8, L4? L10?)</span>

<span>Continue work done by Victor and Ricardo. This is the part I’m
least familiar with. The work has unfortunately mostly been done in the
past, and, recently, in isolation from the current status-react app and
real devices. As far as I can tell it addresses limitations L7, L8 and
possibly L4 (but w/o E2E encryption for actual Firebase notification?)
and L10. What limitations in the current implementation this addresses
should be made explicit, as well as what problems it introduces. For
example, it currently introduces another failure mode & increase
centralization by requiring a Whisper node to be run in a cluster that
we control. If we use this it should be added in small pieces in a way
th</span><span class="c1">at augments the current design and
implementation and can be done and used in a timely manner.</span>

<span class="c1"></span>

<span class="c1">See \[8-13\] below.</span>

<span class="c1"></span>

## <span class="c5">Addendum October 2</span>

<span class="c1">Initially a comment in Slack by Oskar.</span>

<span class="c1"></span>

<span class="c1">In retrospect I could've written the state of PN
document a bit differently. It is assuming that the works that \*can be
done\* with PN are necessarily highest prio. I think if we could get a
basic proof of concept for offline inboxing, which really has nothing to
do with PN, then this would have big consequences for PN. PN work A
would disappear and work C would probably be changed, at least
somewhat.</span>

<span class="c1"></span>

<span class="c1">To me, as an end user, offline inboxing is a much
bigger problem for real use right now. It'd be pretty cool if we could
get a very basic version of this working before devcon.</span>

<span class="c1"></span>

<span class="c1">Additionally, I think the general notifications problem
can best be seen as one instance of a "signal" of sorts in an attention
market. I think this is what @jarradhope hinted at in the whitepaper,
but generalized and ties into reputation systems for normal messages etc
too. I'm still formulating my thinking around this problem. I think this
can be one of the main differentiators between Status and other
IMs/browsers/app-stores.</span>

<span class="c1"></span>

<span class="c1">The use of the word "decentralized" should also be
qualified and we have to be more precise about language and what
problems are we solving. I tried to outline the main limitations of the
current approach in the above google doc, and the only thing that makes
it not decentralized is Firebase. So unless a proposal actually get rids
of that, it doesn't make any difference in that respect. in fact,
introducing a specific role for a Whisper node in our cluster
\*increases\* centralization and failure modes / robustness.</span>

<span class="c1"></span>

<span class="c1">This is also why I'm a bit against these static squads.
Now we are talking in a squads-notifications channel. The language we
use shape our thoughts, and the way I personally conceptualize many of
the immediate problems we want to solve have very little to do with push
notifications per se (offline inboxing, attention market). I think this
easily leads to clouded thinking. The things that are related to push
notifications per se (getting rid of Firebase, design for actual push
notification and deep linking into app, etc) are not thing we are
talking much about at all, and they don't necessarily seem like highest
prio right now either.</span>

<span class="c1"></span>

## <span class="c5">Addendum October 3</span>

<span>It was decided at devcon3 release meeting yesterday (Monday) that
basic user story together with removing FB Analytics
(</span><span class="c9">[<https://github.com/status-im/status-react/issues/1877>](https://www.google.com/url?q=https://github.com/status-im/status-react/issues/1877&sa=D&ust=1507288643417000&usg=AFQjCNFU5k7rSwSSEyAO9wkBzxdllDyf1A)</span><span class="c1">,
pending QA) is good enough of a deliverable for devcon. Next step in
this general sphere of problems is probably offline inboxing; this will
be elaborated on in a separate document.</span>

## <span class="c5">Links</span>

<span>1</span><span>:
</span><span class="c9">[<http://bit.ly/2ff3Cw5>](https://www.google.com/url?q=http://bit.ly/2ff3Cw5&sa=D&ust=1507288643417000&usg=AFQjCNHbeohO-2uWYn4bIbbwlpZWMGP_jg)</span>

<span>2:
</span><span class="c9">[<https://github.com/status-im/status-react/issues/1866>](https://www.google.com/url?q=https://github.com/status-im/status-react/issues/1866&sa=D&ust=1507288643418000&usg=AFQjCNH3EHZdg_diL7xL5vMJgn2x-jyxsw)</span>

<span>3:
</span><span class="c9">[<https://github.com/status-im/status-react/issues/1876>](https://www.google.com/url?q=https://github.com/status-im/status-react/issues/1876&sa=D&ust=1507288643418000&usg=AFQjCNEbAnfoSLFNOe6st1OFXJeCnRklzg)</span>

<span>4:
</span><span class="c9">[<https://github.com/status-im/status-go/issues/343>](https://www.google.com/url?q=https://github.com/status-im/status-go/issues/343&sa=D&ust=1507288643419000&usg=AFQjCNGgYoY1jGK06VzRDr1B63vsg4Rbwg)</span>

<span>5:
</span><span class="c9">[<https://github.com/status-im/status-react/issues/1877>](https://www.google.com/url?q=https://github.com/status-im/status-react/issues/1877&sa=D&ust=1507288643419000&usg=AFQjCNEG8txD71OyCit7InHhTaujZZPgUw)</span>

<span>6:
</span><span class="c9">[<https://github.com/status-im/status-react/issues/1907>](https://www.google.com/url?q=https://github.com/status-im/status-react/issues/1907&sa=D&ust=1507288643420000&usg=AFQjCNHvtac1b4KokQvv0Ousaqmh3Bs42Q)</span>

<span>7:
</span><span class="c9">[<https://github.com/status-im/status-react/issues/1883>](https://www.google.com/url?q=https://github.com/status-im/status-react/issues/1883&sa=D&ust=1507288643420000&usg=AFQjCNELVMI1GxvOJ1U-U0xoFF7UTcp7mg)</span>

<span>8:
</span><span class="c9">[<https://github.com/status-im/status-react/issues/1821>](https://www.google.com/url?q=https://github.com/status-im/status-react/issues/1821&sa=D&ust=1507288643421000&usg=AFQjCNHiAySMMJh92vSSHyCLsQ-DLMEpfA)</span>

<span>9:
</span><span class="c9">[<https://github.com/status-im/status-go/issues/307>](https://www.google.com/url?q=https://github.com/status-im/status-go/issues/307&sa=D&ust=1507288643421000&usg=AFQjCNF2WOhQCQBD5rnZOOZOHPpA_HEYVA)</span>

<span>10:
</span><span class="c9">[<https://github.com/status-im/status-go/pull/266>](https://www.google.com/url?q=https://github.com/status-im/status-go/pull/266&sa=D&ust=1507288643422000&usg=AFQjCNHfcnOz-PTJk_fVsEsErrzWL1VIvw)</span>

<span>11
</span><span class="c9">[<https://github.com/status-im/status-go/issues/222>](https://www.google.com/url?q=https://github.com/status-im/status-go/issues/222&sa=D&ust=1507288643423000&usg=AFQjCNHpZ1BNSziNV03Ddh1TnluKGeGeyg)</span>

<span>12:
</span><span class="c9">[<https://github.com/status-im/status-go/wiki/Whisper-Push-Notifications>](https://www.google.com/url?q=https://github.com/status-im/status-go/wiki/Whisper-Push-Notifications&sa=D&ust=1507288643423000&usg=AFQjCNFdrYBnBPUIKWlM0cccD-UZemY7RQ)</span>

<span>13:
</span><span class="c9">[<http://bit.ly/2xzjSlS>](https://www.google.com/url?q=http://bit.ly/2xzjSlS&sa=D&ust=1507288643424000&usg=AFQjCNE9C8ZpPb8jHyPVIG_T42gEI9Jzvg)</span>

<div class="c0">

[\[a](#cmnt_ref1 "wikilink")\]<span class="c1">agreed, we need to do
this with many aspects of our development, get to MVP as fast as
possible</span>

</div>

<div class="c0">

[\[b](#cmnt_ref2 "wikilink")\]<span class="c1">Really like this, again I
would like to see this happen across all features, and as suggested, we
should have a discussion about this vs eliminating epic/ branches</span>

</div>

<div class="c0">

[\[c](#cmnt_ref3 "wikilink")\]<span class="c1">Can we have a reference
link to the recent build with the feature enabled? I was trying to test
it but realized that I didn't have the right build on one of the two
devices.</span>

</div>

<div class="c0">

[\[d](#cmnt_ref4 "wikilink")\]<span class="c1">Latest nightly build
should work fine</span>

</div>

<div class="c0">

[\[e](#cmnt_ref5 "wikilink")\]<span class="c1">All these limitations are
fine.</span>

</div>

<div class="c0">

[\[f](#cmnt_ref6 "wikilink")\]<span class="c1">Can we clarify this? It
means that if a notification is seen, the message it refers to might be
already not available because of the nature of the protocol?</span>

</div>

<div class="c0">

[\[g](#cmnt_ref7 "wikilink")\]<span class="c1">Since there currently is
nothing in the notification payload, if you click no notification and
more than X minutes have passed the message will have disappeared (since
we don't have offline inboxing yet)</span>

</div>

<div class="c0">

[\[h](#cmnt_ref8 "wikilink")\]<span class="c1">What does Firebase know
about A and B?</span>

</div>

<div class="c0">

[\[i](#cmnt_ref9 "wikilink")\]<span class="c1">Since request to Firebase
comes from A, it at least knows A's IP.</span>

<span class="c1"></span>

<span class="c1">It also knows about B's device in the sense that a
FCMToken is tied to a device.</span>

<span class="c1"></span>

<span class="c1">It's a fairly advanced threat model though. There might
also be other similar unknown unknowns too of course.</span>

</div>

<div class="c0">

[\[j](#cmnt_ref10 "wikilink")\]<span class="c1">I recognize the need for
this, but it cannot make it into a production release.</span>

</div>

<div class="c0">

[\[k](#cmnt_ref11 "wikilink")\]<span class="c1">I agree. I'll see how
hard it is to remove this and if it requires upgrading React Native or
not over the next few days.</span>

</div>

<div class="c0">

[\[l](#cmnt_ref12 "wikilink")\]<span class="c1">I think features should
have team sizes of minimum 3 moving forward. Literature I've read 5
seems to be sweet spot, so we should aim for 5 and reduce amount of
things we are working on.</span>

</div>

<div class="c0">

[\[m](#cmnt_ref13 "wikilink")\]<span class="c1">Focusing on fewer things
and getting them done faster would indeed be desirable. The main concern
here is to make sure the work can be parallelized, so we aren't stepping
on each other's toes / people in a squad are blocked a lot.</span>

</div>

<div class="c0">

[\[n](#cmnt_ref14 "wikilink")\]<span class="c1">FYI I am happy to join
this project from status-go side. It looks pretty interesting to
me.</span>

</div>

<div class="c0">

[\[o](#cmnt_ref15 "wikilink")\]<span class="c1">Cool\!</span>

</div>

<div class="c0">

[\[p](#cmnt_ref16 "wikilink")\]<span class="c1">ok, we have the squad 3
for PN now</span>

</div>