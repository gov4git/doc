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

XXX

## Infrastructure requirements

XXX

## Trust model

XXX

## Application development model

- no VM
- application mutates repo
- low barrier to entry, fast iterations

XXX
