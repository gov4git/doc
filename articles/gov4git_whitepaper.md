# Governance for git

Our journey into "governance for git" begins with a real-world use case: [The Plurality Book project](XXX). The book project was conceived in October 2022 with the objective of crowdsourcing the authoring of a new book on the subject of technologies for digital democracy. By design, this is an open source project aiming to harness the expertise of many loosely (or not at all) related contributors across multiple countries in various capacities, such as translation, fact-checking, original research, discovery of prior art, typesetting, and so on.

Due to the sheer scale of this project — anticipating a number of contributors in the hundreds or thousands, well-beyond the Dunbar limit — an ad-hoc approach to community organization is likely to lead to subpar results. This prompted us to pursue a systemic approach towards designing a flexible and secure solution for governance needs with practicality and rapid path-to-deployment being non-negotiable requirements.

### Problem statement

On its inception, the book project had a variety of specific community organizing needs along with the expectation that these needs will evolve organically over time and will entail experimentation as the norm.

The minimal viable set of features largely focused on the ability to manage a community of contributors with various roles, and conduct free, fair and transparent polls for prioritizing work (such as issues or pull requests). Furthermore, we were interested in using novel polling and voting algorithms (such as the many flavors of quadratic voting) which in turn necessitate mechanisms like community-scoped currencies (for keeping track of voting rights, for instance).

XXX
- Maintain a registry of the identities of its contributors
- Maintain a fine-grained differentiation of roles and responsibilities of individual contributors, such as "organizer", "translator for Korean", "typesetter", "editor for chapter 4" and so on.
- Reward contributors with virtual certificates some of which may have community scope, such as badges or voting credits, while others may have exogenous scope e.g. blockchain-scoped SBTs or hypercerts.
- Perform fair community polls to resolve questions of prioritization, such as "Which issues or pull request should be addressed first?"
- Define and enforce fine-grain policies governing the arbitration and approval processes for changes in various parts of the project. Some examples of policies are:
  - "Changes to Chapter 1 can be approved by the organizer"
  - "Changes to Chapter 2 require a majority vote across contributors who are translators, based on a given quadratic voting rule"
  - "Changes to the governing policies requires a one-person-one-vote referendum across all contributors, or an approval by the organizer."
- Perform services requested by the community members. For instance, a community member may request:
  - "Transfer 20 of my voting credits to Alice."

We determined that such a set of functionalities would suffice for an MVP and will go a long way in facilitating the book project, as well as potentially other projects where community-sourced prioritization is essential, such as the [Filecoin specification proposals](XXX) project.

Nevertheless, we also understood that any application in governance will have to endure numerous changes throughout its lifetime to be successful. By its very nature governance is always-evolving and entails continuous, live experimentation with new policies and functionalities. For instance, a key objective of the book project is to experiment with a variety of research-stage quadratic voting schemes that mediate participants' voting power in ways that ensure representation of diverse points of view.

As a result, we set our sight on creating a framework for building governance applications which would enable rapid and safe experimentation by its users and in production.



- Transparency
- Security
- Economics
- Evolvability

<hr>


  - requirements
    - security
      - transparent, accountable and non-malleable record-keeping that
      - protects the interests of all participants (regardless of role)
    - economics and practicality
      - access
      - cost
    - functionality
      - rapid iteration with new logics and rules


### Decentralized social applications over git

By definition governance is a social application. It entails multiple stakeholders — such as contributors, organizers, even the organization itself — asynchronously communicating on matters of governance. For instance, a contributor may send their vote on a ballot administered by the organization; the organization may send notifications to contributors about a pending referendum on a pull request approval; or one contributor may simply want to engage in a private chat session with another.

Any application involving communication requires an identity system and a mechanism for secure and verifiable communication. The tension in designing a practical architecture for a decentralized application lies between two opposing objectives:

- On the one hand, we aspire to impose minimal infrastructural requirements on the end users, who are ultimately responsible for owning and managing their instances (of execution and/or data) in the application.

- On the other hand, we aspire to provide a method of communication which is secure, verifiable, efficient, timely and fast.

No solution is perfect, rather different trade-offs exist. For instance, in [Solid](https://solidproject.org/) Tim Berners-Lee posits that the user's instance of a decentralized application should be encapsulated in a container, respectively making end users responsible for managing container-level technology and migratory routines. This burden to the user is somewhat mitigated by the use of a specialized deployment provider, in this case [Inrupt](https://www.inrupt.com/). However, this comes at the cost of a significant reduction in access to this technology and diversity of providers. Citizens of most countries cannot purchase Inrupt services, and geographic diversity of Inrupt services is predicated on using one of the cloud giants.

An alternative architectural approach is federation, as used by services such as [Mastodon](XXX) and [Matrix](XXX). This approach relieves end users from managing infrastructure, but it requires them to associate with a federation server which comes with some downsides. Federated servers generally provide lower performance and lower reliability guarantees than analogous cloud services. It may be hard to migrate a user's identity or application history from one server to another, as URLs are often server-dependent. Finally, the [deployment requirements of federated systems such as Mastodon](https://docs.joinmastodon.org/user/run-your-own/) are so dependent on higher-level cloud services that in practice the only places they can be deployed are the cloud giants, which reduces the diversity of their availability as was the case with Inrupt's approach.

[BlueSky](https://atproto.com/docs) is adopting a variation on federation. Users' application instances are hosted on federated servers, similarly to Matrix or Mastodon. However, every user's identity is effectively embodied in a DNS name that they own, control and manage. Users' public DNS names are the gateway to their cryptographic identities as well as to the federation servers that they use. This approach promises to improve the user's agency to migrate, in contrast to other federated services. Nonetheless, the deployment of federation servers continues to be predicated on high-level cloud services (such as storage and VMs), which creates the same frictions to proliferation and diversity of availability applicable to Inrupt, Matrix and Mastodon.

In this work, we propose a new trade-off in decentralized application architecture which accomplishes extreme diversity, availability and durability of service at the cost of a tolerable degradation in the latency experienced by users.

We posit that a decentralized application can be built on top of git as the only required infrastructure. Every participant hosts the state of their application identity is a pair of git repositories: a public one, used for sharing information with other participants, and a private one, for personal data and keys. The public identity of each user is captured in a DNS name (owned, controlled and managed by the user), which points to the public git repository of that participant.

XXX
- dns: independence from hosting providers
- one-click migration
- optimal bandwidith utilization at the cost of sync latency

### Governance as a state machine

<hr>

## Cover
- what is governance
  - protocol between members + format and location of record keeping
- application model
- trust model

- comparisons
  - governance
    - smart-contract DAOs
  - dapp architecture vs 
    - bluesky
    - scuttlebug (https://scuttlebutt.nz/)
    - 