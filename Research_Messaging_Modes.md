# Research: Messaging Modes

The Status Messenger will operate in different modes. These modes
influence details of the implementation. Here we collect requirements of
the modes and so also requirements to the underlying technology.

## Mode 1:1

  - Messaging between two users
  - Receiving status is known by sender
  - Reading status is known by sender

## Mode 1:N

  - Messaging between multiple users
  - All users know all other users
  - Users don't have to be in each others contacts
  - Receiving status is known by sender
  - Reading status is known by sender

## Mode Public

  - Subscription to a topic
  - Messages to a topic can be read by all subscribers
  - List of receivers is not known
  - So receiving status or reading status is not known too