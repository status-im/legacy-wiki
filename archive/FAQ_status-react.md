# Is there a rationale for using simple optimizations only for prod builds?

difference in size is about 300KB, anyway RN packager makes some magic
with js after cljs compiler. The difference in error output is that with
\`:simple\` we have much more useful info if error happens, not just
\`a.b.adsfas is undefined\`.

The only reason why we added \`:advanced\` was that when resulting cjs
was small RN packager ran much faster. And we didn’t meet that
non-configurable 300s timeout for
packager

# Is that intentional? Alice has account Sneaky White Koala. Bob has account Cool Damp Bear

When Alice adds Bob's account, she will see him as e.g. Breezy Tall
Elephant Until Bob repays the honors.

the names are not currently deterministic so the best option at the
time, was to randomly generate one well, we intentionally did that
behaviour but it's not desirable in this
revision

# Is there no way to skip new account creation wizard after a fresh install? If e.g. I just installed Status on a new device, but only with intention to recover my existing account there?

we'll be redesigning the onboarding but currently, no, the reason was
back in the day 90% of people would be making a new
account

<https://github.com/status-im/status-react/issues/741>

# Saved username, image, etc are only stored in the device's local storage, right? Not supposed to be restored ?

thats correct, we have no servers to store that on (as we are a p2p
application), when we have Light Swarm Clients (goteam i'm looking at
you\!) we can have some kind of persistent state, photo/audio messaging,
encrypted app-db and uploaded to swarm for retrieval

<https://github.com/status-im/status-react/issues/718>