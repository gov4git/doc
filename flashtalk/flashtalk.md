_Abstract_

We demonstrate a practical way to build decentralized community governance applications
using only very basic, widely-available infrastructure (DNS, git) while still offering most of the security guarantees of expensive and complex solutions based on public blockchain technology.

# gov4git: Governance of decentralized open-source communities

## 1. A real-world problem

broad goal
     conceive and govern a large open-source community
     to author a book together
     in a timely fashion

focus
     design tools to govern in a transparent and pluralistic fashion

day-to-day responsibilities of governance
     - directory of member identities
     - groups and roles
     - community-scoped member accounts: badges, voting credits, sbts
       - account services like transfers
       - import/export to public blockchains
     - polls for prioritizing work (issues and pull requests)
     - referendums for approving pull-requests
     - quadratic voting of many flavors

situational constraints
     - real-world governance is an evolution
       - experimentation and iteration with governing logic is the norm
       - governance must be equipped to govern changes to its own logic

     - large communities (> dunbar) need mechanisms for protecting member rights
       - users must be able to audit and verify governance history

     - cheap and easy deployment in potentially disconnected, war-torn or fire-walled regions

## 2. Software perspective: an application-agnostic abstraction for governance

Governance characterized by three observations:

Governance is a social application (i.e. a decentralized protocol):
     Multiple independent entities (contributors, organizer, community) communicate asynchronously.
     - Organizer notifies members of a new poll
     - Contributor casts a vote on a referendum
     - Contributor proposes a code merge
     - Contributor transfers voting credits to another

Governance is a state machine:
     Governance provides a unified state of what the community members have agreed on in the past.
     - Directory of community members, roles, and account balances
     - The official HEAD commit for the open-source project
     - Polls and referendums, votes and tallies

State changes require proofs-of-compliance (to current policies):
     - Changing the HEAD commit may require a majority vote by the group of editors

## 3. A radical approach

superficially, a textbook application for a smart-contract, but:
  - expensive, slow, development still complex
  - not available in fire-walled regions; access to public blockchains can be censored easily

minimal infrastructure?
- an open source community already uses git

technical contribution
- a decentralized governance-specific blockchain on git and DNS (as only infrastructure)
- blockchain-like trust model and security at no cost

result
- governance can be bootstrapped even in a war-torn zone, using a standard Linux distro and a regional connection

## 4. Decentralized applications over git and DNS

decentralized application
     storage: public git repo
     execution: at the client
     identity: user-operated public git repo with a public domain name
     communication: drop/fetch messages to and from public repos

alternative to bluesky, solid, mastodon, matrix
     (-) higher-latency
     (+) easier and cheaper to deploy

## 5. Governance-specific blockchain over git

- governance history is recorded in a blockchain (data structure)
  - a block represents a change to the state of governance
  - a valid new block requires proof-of-compliance (to policies)
    - e.g. proof that "a majority of editors voted to approve a change to chapter two"
    - proof-of-compliance is socially-sourced; cheap and easy compared to proof-of-work
    - commodity hardware more than sufficient

- the blockchain is embedded in one or more git repos (owned by community members)
  - miners are community stakeholders (organizer, contributors)

- benefits
  - standard tooling for application development and audits (Go, Rust, TypeScript, VS Code, git, JSON)













———

Identity system:
- Participant identifiers are public URLs (e.g. gov4git.maymounkov.org, DNS, ENS, etc.)
- Participant maintains a public git repo with rich/mutable identity information (signing keys, SBTs, ETH address, etc.)
- DID-compatible approach, also used by BlueSky

Mechanism for communication:
- Sender drops (signed/encrypted) messages in their repo
- Receiver fetches messages from sender's repo

Execution happens on the client (browser or terminal)
- Clone, read/verify, write/sign/encrypt, push
