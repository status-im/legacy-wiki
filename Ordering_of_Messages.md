As in any distributed system ordering events (messages) is non trivial.
We describe a few of the solutions considered and then we describe the
current implementation.

### Causal relation between chat messages

To say that a message <i>a</i> is causally related to <i>b</i> (<i>a -\>
b</i>) means that the sender of <i>b</i> has seen/received <i>a</i>
before sending <i>b</i>.

For example:

    1) A sends message a1
    2) A sends message a2

<i>a1</i> and <i>a2</i> are causally related.

    1) A sends a1
    2) B receives a1
    3) B sends b1

<i>a1</i> and <i>b1</i> are causally related.

Note that:

    1) A sends a1
    2) B sends b1
    3) B receives a1

Then <i>a1</i> and <i>b1</i> are **not** causally related.

The assumption is that ***preserving causality is the most important
property in ordering messages***.

## Solutions considered

### Order by time received

Ordering by the time a message has been received would work only if the
network is FIFO (no out of order messages).

If FIFO is not guaranteed, then the following scenario might happen:

    1) A sends message a1
    2) A sends message a2
    3) B receives message a2
    4) B receives message a1

As network ordering cannot be guaranteed, this solution has not been
taken into consideration further.

### Order by time sent

Ordering by the time a message has been sent would work only if the
clocks are synchronized.

An example of where it would fail ( clock(B) is equal to clock(A) - 4)

    1) A sends message a1 with timestamp 0
    2) A sends message a2 with timestamp 1
    3) B receives a1 and a2
    3) B sends message b1 with timestamp 2 - 4
    4) B sends message a1 with timestamp 3 - 4

The display order on both clients would be:

  - b1
  - b2
  - a1
  - a2

This is not desirable so client timestamp only cannot be used for
ordering.

### Lamport timestamps

Lamport timestamps algorithm
<https://en.wikipedia.org/wiki/Lamport_timestamps> provides a way to
order causally related events.

The base algorithm is very simple.

When receiving a message:

    own_clock = own_clock + 1;
    time_stamp = own_clock;
    send(message, time_stamp);

When receiving:

    (message, time_stamp) = receive();
    own_clock = max(time_stamp, own_clock) + 1;

This ensure that causally related events are always correctly ordered.

Because of this property we decided to move forward with this algorithm.

## Current implementation

The current implementation uses Lamport timestamps, but modifies the
base algorithm to overcome 2 issues.

### Consistent ordering across clients

Because Lamport timestamps are not unique, consistent ordering across
clients cannot be guaranteed.

For example:

    1) A sends message a1 with T 1
    2) B sends message b1 with T 1
    3) A receives b1, B receives a1

Because the timestamp of <i>a1</i> and <i>b1</i> are identical, ordering
might be different across devices.

To overcome this we just break ties by `message-id`, which means that
now we have a consistent order.

### Propagation of timestamp

We should strive to show new messages last in the chat.

Unfortunately this is not always guaranteed with the base algorithm.

Here's an extreme case of when that might happen (there are many
variations to this scenario):

    1) A public chat is created
    2) Many messages are exchanged, bringing the chat-timestamp to 1000
    3) B joins the chat, but has not received any message
    4) B posts a message b1 with timestamp 0

As you can see <i>b1</i> would be displayed at the beginning of the
chat.

To overcome this we try to maximize the chances of <i>b1</i> to come on
top, leveraging the fact that clocks tends to be more or less
synchronized.

In the current whisper protocol (v6) any client that has drifted for
more than 20 seconds will not be able to receive or send any message
from untrusted peers.

So we modified Lamport algorithm for sending to be:

```
  own_clock = max(own_clock, now-in-ms() * 100) + 1;
  time_stamp = own_clock;
  send(message, time_stamp);
```

(The multiplication by 100 is just to ensure timestamp and post-id are
in separated digits in most cases)

Taking the maximum between `own_clock` and the local time ensures that
causality is preserved.

If for example you received message <i>a1</i> with timestamp <i>t1</i>,
but your clock is at <i>t1 -1</i>, when sending a reply <i>b1</i> the
timestamp is:

    max(t1, t1 -1) + 1 = t1 + 1

ensuring that `T(a1) < T(b1)`

If your clock is instead at <i>t1 + 1</i>

    max(t1, t1 + 1) + 1 = t1 + 2

which still ensures that `T(a1) < T(b1)`