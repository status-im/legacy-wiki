SECP256k1\] public key. Since the Ethereum Foundation’s Ethereum Name
Service is specifically designed to map string to DApps and addresses,
it is a natural fit for Whisper-based identities. We are working with
Nick Johnson, co-creator of the Ehereum Name Service, to create a
username- to-SECP256k1 public key resolver on ENS that allows
stakeholders to register usernames on a first-in, first-out basis, with
a social recovery mechanism. This gives stakeholders a human- readable
and recoverable access point should phone or private keys be lost.

Rather than delegating other addresses to recover, we instead abstract
the recovery process by allowing the stakeholder to create 5 signed
messages, that can then be shared out-of- band with friends or stored in
a safe location. A default of 3 of 5 signed messages will be required to
change the username owner, but can be optionally changed by the
stakeholder.

An account must be holding The Status Network Token in order to reserve
a username. This username is used to identify themselves within the
greater Status community, and signal participation in the network.
Should a stakeholder wish to leave the network they can withdraw their
tokens and the username will be unreserved and claimable by any other
stakeholder.

This system will then depend on uPort for KYC/AML requirements where
required, as a completely optional step, for users who wish to interact
with regulated financial tools and DApps within the application.

**SNT Utility**

  - SNT is required to register a username on the Status Network.

**Example use-cases**

  - Stakeholder A, a casual user, wishes to register the username @david
    inside of Status so his friends can easily find him, and more easily
    recover his account should he lose his mobile device.
  - Stakeholder B, a DApp developer, mandates only registered Status
    users to post on his discussion board DApp, in order to mitigate
    sockpuppet accounts using his decentralized application and increase
    the quality of content.