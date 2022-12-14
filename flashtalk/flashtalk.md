_Title_
gov4git: Practical secure and transparent governance using git and DNS

_Synopsis_
We demonstrate a practical way to build decentralized community governance applications
using only very basic, widely-available infrastructure (DNS, git) while still offering most of the security guarantees of expensive and complex solutions based on public blockchain technology.

## 1. A real-world problem

problem
     conceive and organize a large-scale open-source project in timely fashion

functional needs
     - directory of member identities
     - groups and roles
     - community-scoped accounts: badges, voting credits, sbts
       - account services like transfers
     - polls for prioritizing work (issues and pull requests)
     - referendums for approving merges
     - quadratic voting of many flavors

situational constraints
     - real-world governance is an evolution
       - experimentation and iteration with governing logic is the norm
     - large communities (> dunbar) need mechanisms for protecting member rights
       - users can audit and verify governance history
     - high accessibility, minimal infrastructure

## 2. Software perspective

Governance is a decentralized social application, which maintains a common state (and history) for its participants.

Governance is a social application:
Multiple independent identities (contributors, organizer, community) communicate asynchronously.
     - Organizer notifies members of a new poll
     - Contributor casts a vote on a referendum
     - Contributor proposes a code merge
     - Contributor transfers voting credits to another

Governance is a state machine:
     - Directory of community members, roles, and account balances
     - The official HEAD commit for the open-source project
     - Polls and referendums, votes and tallies

Governance state transitions require proofs of compliance:
     - Changing the HEAD commit may require a majority vote or an approval from an organizer

## 3. A radical approach

Textbook application for a smart-contract, but not practical:
- Slow, expensive
- Complex development, using bleeding edge tools
- Unnecessary dependence on public blockchains

A sovereign community blockchain can be built on git and DNS as the only required infrastructure.
- Governance can be bootstrapped in a war-torn zone using a standard Linux distribution and a local network connection alone.

## 4. Decentralized applications over git and DNS

Identity system:
- Participant identifiers are public URLs (e.g. gov4git.maymounkov.org, DNS, ENS, etc.)
- Participant maintains a public git repo with rich/mutable identity information (signing keys, SBTs, ETH address, etc.)
- DID-compatible approach, also used by BlueSky

Mechanism for communication:
- Sender drops (signed/encrypted) messages in their repo
- Receiver fetches messages from sender's repo

Execution happens on the client (browser or terminal)
- Clone, read/verify, write/sign/encrypt, push

## 5. Application-specific blockchain over git

A blockchain holding governance state is embedded in a git repo:
- Chain blocks are git commits
- Block data is clear text JSON

* Standard tooling for application development and audits (Go, Rust, TypeScript, VS Code, git, JSON)

Operation:
- "miners" are community stakeholders (organizer, contributors)
- "blocks" represent changes to the state of governance
- "proof-of-work/stake/resource" is replaced by a proof-of-compliance (to governance policies)
  - e.g. proof that a majority vote was reached
  - e.g. proof of approval by organizer

* Proof-of-compliance is cheap and easy, unlike proof-of-resource
