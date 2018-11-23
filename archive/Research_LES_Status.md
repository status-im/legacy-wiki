# Research: LES Status

Date: 2018-02-13

## Subprojects

### Implementing Observer Chain

  - \`Block\` with signifaction and containing a trie is almost done
  - \`Statement\` for storrage of key/value pairs in the trie of a block
    is done
  - \`Chain\` is missing the transaction methods to create and block a
    new block, manipulate the trie, and unlock it again

### Event Logging with Observer Chains

  - \`EventLogger\` for wrapping the observber chain for logging is
    designed and will be started now

### On-Chain Validation with Chain Filters