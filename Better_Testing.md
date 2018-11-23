How we should structure tests to improve velocity and reliability.

# Pages

[Roadmap](Roadmap "wikilink") \[CI Pipeline (Travis CI)\]\]

# Terminology

**Unit tests** verify if the smallest structure of a program, like
functions or methods, work properly. They should test all branches in a
given function and aim for 100% coverage.

**Integration tests** verify if separate components can work together.
For instance, verify if `jail` can work with the `rpc.Client` using a
`net/http/httptest.Server` mock. It’s important to keep integration
tests fast, reliable and predictable.

**E2e tests** verify if a program works properly with external systems.
It should resemble the production setup as close as possible. In our
case, e2e tests would use any public testnet.

# Type of tests

![03-Piramides-03-03.png](03-Piramides-03-03.png
"03-Piramides-03-03.png")

## Unit tests

**Features**

  - the first line of defense against bugs
  - lightning fast
  - lots of small functions each testing a single case
  - cover only code within its own package
  - the obligatory part of CI on every stage

**When write**

  - when you introduce a new function
  - add a functionality to the existing function
  - modify an existing function

**When run**

  - after any change of code
  - in each CI job

## Components/Integration/API Tests

**Features**

  - verify that components can work with each other
  - use mocks heavily
  - lightning fast
  - do not require external systems
  - the obligatory part of CI on every stage

**When write**

  - a component got new exported functions
  - an existing exported function/struct/interface changed its shape or
    logic

**When run**

  - before commit
  - in each CI job

## e2e tests (GUI in the image)

**Features**

  - verify if compliant with other systems
  - take significantly more time

**When write**

  - a new system feature was added (e.g. new protocol support)

**When run**

  - before preparing a release
  - periodically in CI job
  - manually if needed

# Private blockchain and CI pipelines

-----

A private blockchain can be used in e2e tests and could be a part of a
pull request CI pipeline because the private network should provide much
higher reliability than public testnets.

The public testnets could be a part of a release CI pipeline and also
run periodically every 1 hour or so. The idea is that if the public
testnet is unavailable, it does not affect the development but it can
affect release.

[Continous Delivery](Continous_Delivery "wikilink")