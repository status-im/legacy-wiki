# Goals of this document

Describe the new protocol code in a clear and easy to understand way
("bird’s eye view" with just each distinct message type and what they
represent in term of actions explained) Describe each message type in
detail, including all the information encapsulated in it (update
whenever new features requiring protocol version inc are added) Describe
security mechanism of the new protocol, where the sym/asym keys are
used, how they are updated/refreshed, how do we choose whisper topics,
etc.

# TODOS

  - Make benchmarks
  - Write tests
  - Implement acks, nacks
  - Discuss transport layer as external library
  - Remove crypto libraries from status-react side
  - Remove nodeify hack
  - Clean up realm (pending-messages, useless fields…)