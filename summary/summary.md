# Gov4git: Executive summary

## Mission and motivation

Our mission is to design governance software and protocols which enable people to bootstrap and grow pluralistic and transparent open source communities.

Two of the most useful functionalities for open source community governance are polls and referendums. Polls are commonly used for prioritizing work, such as issues or pull-requests. Whereas referendums are used for approving changes to the source of the community, its membership or its governing rules.

Every community is unique, which is why a successful governance software must accommodate different types of balloting mechanisms, ranging from traditional one-person-one-vote to modern approaches such as Quadratic Voting and all of its flavors. More generally, governance should be programmable.

Modern community mechanisms also utilize a range of community-scoped tokens (such as fungible/non-fungible, transferable/non-transferable) as a way of bookkeeping member contributions and mediating voting power. Therefore, no governance solution is complete without support for community-scoped member accounts and services for community-specific tokens.

## Constraints and requirements

First, we aim to support the entire life of a community. Communities grow from small to large, especially if successful. Large communities comprise individuals who are mutually distrusting. Therefore, a successful governance solution must ensure that member rights are enforced as advertised, e.g. polls and referendums cannot be "rigged" by higher-ranking members and can always be audited.

Second, it is our utmost priority to serve the tail case of users who are disadvantaged, reside in the developing world, or in disaster, war-torn or otherwise firewall-ed zones. They should be able to form local governed communities as well. To accommodate these cases, we require that our solution does not depend on connectivity to the global Internet and can operate on commodity hardware alone.

## Software abstraction

From a software perspective, a governance application can be described succinctly by two standard abstractions. Governance is a:

- _Decentralized social application_: Community governance entails the interactions of its members. It requires an identity system and a method for asynchronous communication.

- _Shared state machine_: All community members must agree on a unified view of community affairs, such as current governing rules, past decisions, referendum outcomes, and so on.

## Related work

The current Web3 ecosystem provides "textbook" solutions for both of the software abstractions required by governance. [Inrupt](XXX), [Matrix](XXX), [Mastodon](XXX) and [BlueSky](XXX), for instance, offer alternative software architectures for building decentralized software applications. Whereas blockchains like [Ethereum](XXX), [Solana](XXX) or [Tendermint](XXX), for example, can be used to implement an application-specific shared state machine.

Unfortunately, none of these solutions meet our requirements for operation in disconnected regions of the world with only commodity infrastructure at hand.

## Technical solution

As our key technical contribution we provide two software frameworks:

- One, for building decentralized social applications, using only git as a backend infrastructure while deferring all computation to the client (such as a web app). This framework provides an identity system and a method for communication to application developers.

- Second, for building governance-specific replicated state machines (i.e. blockchains), using git as a storage backend and commodity hardware (such as a Raspberry Pi) for execution. This framework provides execution and state replication to application developers.

It is noteworthy how and why we are able to build a blockchain with low infrastructure requirements. The source of high infrastructural demands in traditional blockchains stems from expensive application-agnostic mechanisms (e.g. proof-of-work, proof-of-stake, proof-of-storage) for conflict resolution between competing applications. In contrast, community governance runs as a single application on a dedicated blockchain. Conflicts in a governance application can arise only between user requests. These are application-specific conflicts which are definitionally resolved at the application level, using human-centric protocols such as referendums, quorums, approvals, and so on. 

## Software architecture

![Architecture](arch.svg)

## Deployment infrastructure

XXX

## Trust model

XXX

## Application development model

Traditional blockchains use a VM-based architecture to execute application code. VMs provide isolation and portability. Unfortunately, VMs also necessitate the use of bleeding edge development environments, such as languages like [Solidity](https://soliditylang.org/), [AssemblyScript](https://www.assemblyscript.org/) or [WASM](https://webassembly.org/).



- no VM
- application mutates repo
- low barrier to entry, fast iterations

XXX
