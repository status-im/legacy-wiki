### Before reporting

1.  Make sure you can reproduce an issue at least 3 times
2.  Check that found issue is not know yet: search in GitHub by the
    keywords:
    1.  If there is an error message then search by its text or 3-4
        first words from the message.
    2.  If there is no error shown then search by some names of UI
        elements like "profile", "send", "QR" etc that are connected
        with issue you've found.
    3.  Search by the screen/area title where issue occurs, e.g.
        "Chats", "Console", "Popular statuses"
    4.  It's also useful just to read known issues (marked with label
        \`bug\`) and get used to the terminology and format of the bug
        report.
3.  If issue is not yet reported then create a new one in Github repo
    and follow the issue template. It's very important and if you really
    want to help then invest some time in creating a good title,
    description, steps, attaching video/pictures and logs.

### Good Bug Title

This is the first thing anyone reads about the found issue. This is the
line that explains how this bug is different from all other bugs in the
list. So, be specific and tell what’s wrong. No need to tell what’s not
happening and what you don’t see. Tell what happens and what you can
see. Literally, describe physical objects you see on the screen. Pretend
you are the mirror that shows some situation on the screen to the
reader.

For example, this title is not good: \`location does not work in chat\`
because it does not explain what exactly is wrong and it does not tell
what you really see when issue happens. There is definitely something
strange but the title does not tell what. There might be 20 issues with
similar title but the exact manifestation of the broken location would
be different for each of them. So, describe that thing you can see on
the app screen when issue happens. This specific info will differentiate
your report from other 19 bugs where location does not work.

Let’s look again at the location title and ask yourself: what exactly
happens that makes me think it’s a bug? What do I see on the screen?
What I would expect to see? For example, location command is used, it
shows your current location, you tap on the address but sent message
shows other location. Then tell this in the title: “Other location is
shown instead of the selected current location in the sent message”.

The pattern for a good title has 3 parts and only 1 is mandatory:

1.  Exact problem description: “Other location is shown”
2.  What was expected (optional): “instead of the selected current
    location”
3.  Place/condition/details (optional): “ in the sent message”

Do not tell in the title “Fix location” or “Strange behaviour in chat”.
You already reporting an issue that means it needs to be fixed.
Behaviour is already strange this why you are reporting it. The less
specific info you provide the harder is to search for the bug and to
understand it.

More
examples:

|                                  |                                                                                                                                             |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| Bad                              | Good                                                                                                                                        |
| location does not work in chat.  | Empty box instead of the map is shown in 1-1 chat                                                                                           |
| Incorrect functioning while type | Previously sent text is automatically inserted in the input field if start typing new text                                                  |
| Weird things with emoji          | Letter instead of flag emoji is shown in the emoji list and 1-1 chat                                                                        |
| Red error in chat                | "Recipient name must be specified" error is shown if tap on requesting message in group chatBattery icon is not visible on white status bar |

Bug title is the essence of the bug. It’s like a headline in the
newspaper. You want to bring news about the problem, so tell about the
exact problem in the beginning of the title and leave the details for
the end or for the description.

### Good description

So, we are done with the title and now we have some attention from the
reader, this is time to tell more details about the bug. There are
different types of details:

  - Must:
      - what’s the problem,
      - write screen/area title where issue occurs. By mentioning the
        name of the screen/area you make it super easy to find the bug
        for anyone who will see issues on the same screen. By making a
        typo or not mentioning it - you are decreasing this chance
      - describe conditions to get the issue.
  - If there is a visual bug then add a screenshot with a problem area
    selected by red rectangle. Picture is always better than long
    description.
  - If there is no picture but reader needs to see a flow then add a
    link to a video (for example on youtube/google drive). Sorry but we
    might not follow links to other resources.
  - If there is a popup with an error message, then type its text or at
    least first 4-5 words. It’s important because search will not find
    anything inside the screenshot of the message, so to make life
    easier for other testers, including you, error text is needed. If
    there are more than 4-5 words to type then attach screenshot of the
    full error text too.
  - Mention any special conditions that are needed to reproduce an
    issue. For example, bug happens only after some specific actions or
    only on the 1st app run.
  - Mention how many times you tried to reproduce an issue.  E.g. can
    reproduce 3 times/3 attempts. If it happens sporadically then
    mention how many times you’ve seen it / how many times you’ve tried,
    e.g crash happened 3 times / 7 total attempts.

### Good expected and actual results

Expected:

1.  what I expect to see, not what I do not expect. For example: "after
    sending location command there is new message in the chat with a
    map" instead of "no error shown"
2.  Even if it's very obvious please tell what you expect to happen.
    This will help developer to justify if the fix works and tester to
    verify the bug fix.

Actual:

1.  what exactly happens. Not: "and it's broken" or "I can't type" but
    "after I tap in the input field there is no keyboard shown, input
    field stays at the bottom of the screen as if I didn't type inside
    it". It might be too obvious but it's not. There are more reasons
    why you can't type: maybe keyboard is shown but tap on each key has
    no effect. Maybe you can't type because keyboard is shown ok, you
    can tap on the key but the typed letter is not shown in the input
    field, etc.
2.  Most likely this will be the second thing reader will check after
    the title. So even if you need to repeat some info from description
    - repeat it. This is a brief on actual manifestation of the bug.
    You'll read it first too when you'll start verifying the fix for the
    issue.

### Good Steps to Reproduce

1.  Start from the beginning and document each step to get to the
    problem. Do not jump from the middle of nowhere with a step:" and
    send 2 more messages to that contact in any chat."
2.  Even if you can reproduce the issue with any data in any chat,
    specify the exact data you've used. For example: "send any amount in
    any chat, in my case I've sent 0.004 ETH in the group chat."
3.  Guide a reader. Ideally, each step that need some waiting or
    transition to other area/screen should mention it. Imagine your
    issue will be investigated 10 days later when some new screen is
    introduced and older one is removed. It will be hard for the reader
    to follow why suddenly there are some UI elements mentioned that
    he/she doesn't see in the app. If you explicitly tell that screen X
    is opened as a result of some action then it will help.

For example:

\- Open 1-1 chat with Jarrad

\- Tap on send command

\- in the input field after /send command type "0.1"

\- tap on send button. As a result, Sign a transaction screen should be
shown

\- type your password in the popup ...

### Good Additional Info

Specify not only OS but also

\- real device model or emulator, e.g real Samsung Galaxy S6 ;
Genymotion emulator Galaxy S6

\- build type used (release or nightly build)

\- build version

\- link to apk and date if it's nightly build

\- Logs (logcat at least)

\- For nightly builds we record session info in the TestFairy (video and
logs) if you are on wifi, so you can specify exact time when issue
happened, your country and build number - this will help to locate your
session in TestFairy and tester from the core team can update your bug
report with a link