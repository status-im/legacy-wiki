As Status is growing, both in the number of teams and people, we've
started making use of "squads". What is a squad and what purpose does it
serve / what problems does it solve?

A squad is a small group of people across different teams who are all
working towards a specific goal / vision / epic. The purpose is to
clarify and focus communication between these individuals in light of
the goal they are all trying to achieve. It's also useful to
decentralize the decision making so who should work on what in a
specific area isn't necessarily a team decision but a squad decision.

Examples of useful squads: Jenkins squad. We have cross-team concerns
(Clojure, Go and QA) with the general goal of making Jenkins work better
for all of us. We found that there were conversations in multiple
threads and lots of people who were, or should be involved, didn't get
all relevant information.

Another example: Wallet squad, which currently consists of people from
Clojure and Design team. This is part of the wallet epic. They ended up
creating their own branch \`epic/wallet\` to make it easier to have work
in progress checked in and build on each others' works, and I've
personally seen a lot of back and forth with the design team on specific
issues. If this conversation happens in \#internal-design it is likely a
lot of people in \#internal-clojure who should be aware of it miss it
(this has happened a few times as far as I can tell).

Three main reasons:

1\) Decentralized decision making is the biggest one. The fewer people
are involved in a decision means we can take action quicker, and the
better it is. As long as everyone who is relevant to that thing are
involved.

2\) Communication localisation.

3\) Avoiding team silos, that's the other 3rd big thing I mean each team
working in isolation without talking much to people in other teams who
are impacted by work.