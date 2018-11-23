## Developer Collaboration Rules

Status is an open-source application and any code you write is reviewed
and merged by somebody else. We ask you to stick to some rules while
implementing your new cool features and trying out new ideas in order to
keep pull requests clean and to the point and help others review and
merge your code easily.

### The Crush Rule

Keep pull requests as small as possible. Thus, keep open issues as small
as possible.

Why? Small issues are done faster, reviewed faster, less error-prone and
merged more easily. This leads to faster delivery and feedback and more
happy users.

How? Ensure you absolutely understand what needs to be done after
starting a new issue and try to estimate how much code you need to add
or change to achieve it. If it is more than roughly 300 lines without
tests, discuss splitting the issue into several.

### The Main Rule

While working on feature A, only change code related to feature A.

Why? This simplifies review and merge. A reviewer will not have to
distract from the code goal on unrelated edits. Also, while you were
working on feature A, somebody had already merged feature B, C and D. If
all of you changed the same unrelated code, merge can become hard.

This is what you should not do for unrelated code:

1.  Do NOT add blank lines.
2.  Do NOT move code within a file.
3.  Do NOT move code between files.
4.  Do NOT move or rename files.
5.  Do NOT rename any variable, function or type.

However, we all strive to perfect code: efficient, well-structured and
readable and there are often reasons to make one of those 5 actions. If
you feel a reasonable desire to perform such refactoring, it is welcome
but in a separate refactoring task. Refactoring task should also be
small, fast and merged in within a single working day.

### The Reliability Rule

Always write tests for the code you change.

Why? This helps to review your code, ensure it works properly and make a
decision in case of merge conflicts. Preferrably, write tests for the
changed code in the same commit.