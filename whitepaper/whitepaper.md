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

- On the other hand, we aspire to provide a method of communication which is secure, verifiable, efficient and fast.

No solution is perfect, rather different trade-offs exist. For instance, in [Solid](https://solidproject.org/) Tim Berners-Lee posits that the user's instance of a decentralized application should be encapsulated in a container, respectively making end users responsible for managing container-level technology and migratory routines. This burden to the user is somewhat mitigated by the use of a specialized deployment provider, in this case [Inrupt](https://www.inrupt.com/). However, this comes at the cost of a significant reduction in access to this technology and diversity of providers. Citizens of most countries cannot purchase Inrupt services, and geographic diversity of Inrupt services is predicated on using one of the cloud giants.

An alternative architectural approach is federation, as used by services such as [Mastodon](XXX) and [Matrix](XXX). This approach relieves end users from managing infrastructure, but it requires them to associate with a federation server which comes with some downsides. Federated servers generally provide lower performance and lower reliability guarantees than analogous cloud services. It may be hard to migrate a user's identity or application history from one server to another, as URLs are often server-dependent. Finally, the [deployment requirements of federated systems such as Mastodon](https://docs.joinmastodon.org/user/run-your-own/) are so dependent on higher-level cloud services that in practice the only places they can be deployed are the cloud giants, which reduces the diversity of their availability as was the case with Inrupt's approach.

[BlueSky](https://atproto.com/docs) is adopting a variation on federation. Users' application instances are hosted on federated servers, similarly to Matrix or Mastodon. However, every user's identity is effectively embodied in a DNS name that they own, control and manage. Users' public DNS names are the gateway to their cryptographic identities as well as to the federation servers that they use. This approach promises to improve the user's agency to migrate, in contrast to other federated services. Nonetheless, the deployment of federation servers continues to be predicated on high-level cloud services (such as storage and VMs), which creates the same frictions to proliferation and diversity of availability applicable to Inrupt, Matrix and Mastodon.

In this work, we propose a new trade-off in decentralized application architecture which accomplishes extreme diversity, availability and durability of service at the cost of a tolerable degradation in the latency experienced by users.

We posit that a decentralized application can be built on top of git as the only required infrastructure. Every participant hosts the state of their application identity in a pair of git repositories: a public one, used for sharing information with other participants, and a private one, for personal data and keys. The public identity of each user is captured in a DNS name (owned, controlled and managed by the user), which points to the public git repository of that participant.

Similarly to BlueSky, the association between DNS and identity decouples the user's data and application logic from dependence on hosting providers (or federation providers in the case of BlueSky). Our approach of using git as infrastructure provides a few additional benefits. 

Out of the box the git protocol itself provides essentially "one-click" migration and replication functionalities. This means that application users are truly able to transition providers nearly instantaneously using well-understood and much practiced standard workflows, which can be performed with any git client and do not require specialized software. To the best of our knowledge, no existing dapp architecture supports this level of frictionless migration. It should be noted that while ease of migration may seem a tangential consideration, it is the key measure and driver of infrastructure independence for dapps. The only better alternative to frictionless migration is requiring end users to own their infrastructure down to the metal which is untenable.

Reliance on git as infrastructure also accounts for a remarkably low barrier to deployment of a personal application instance. Git hosting is ubiquitously accessible around the world. Hosting opportunities range from high-end commercial (such as GitHub, GitLab, BitBucket, or SourceForge) in the industrialized world, to mid- and small-size offerings based on open-source software (such as Gitea) in developing countries, to standard ad-hoc solutions such as running the git client in server mode through an SSH server on UNIX systems.

The lack of dependence on high-end cloud services (such as sophisticated storage solutions, used by most dapp architectures) affords social networks based on git to be truly independent of large international networking backbones (such as those of Google, Amazon, or Microsoft). In particular, such networks can be deployed rapidly in war-torn countries and other disaster areas with only a standard Linux distribution at hand.

Beyond improving access to infrastructure, using git provides a number of benefits to application development. The mature ecosystem of developer tooling (e.g. IDE integrations) for git facilitates common tasks such as inspecting (local and remote) application state when developing, debugging or auditing. In contrast, inspecting the state of a live smart contract without (or even with) access to the original application schema is non-trivial and time-consuming.

Using git for application storage also affords us to build a simple, reactive application development framework which relieves developers from data management considerations such as caching, compression and networking. In part these benefits come "for free" from git's built-in capabilities, such as bandwidth-optimal synchronization and blob compression. Still, cloning large repositories can be slow and can affect user experience. We have mitigated this issue to a reasonable degree by developing a local git cache at the client, which saves bandwidth and latency transparently to the application.

There are two caveats to using git as an application backend. Users will experience a latency in UI interactions stemming from the underlying multi-round cloning protocol used by git. This latency is perceptibly longer (e.g. 1-2 seconds) compared to Web2 applications (e.g. 200ms), but also perceptibly shorter than blockchain-based applications (e.g. 1 min). Standard git hosting solutions prohibit unsolicited communication. This makes it hard to implement applications such as email over git, for instance. On the other hand, most modern social applications — such as social networking or community governance — do not entail unsolicited communication.

Our application framework uses a channel-like abstraction to model directed communication from Alice to Bob. Conceptually, Alice and Bob share a git branch that Alice can write to and Bob can read from. When Alice appends a new git commit to the branch, she is sending a message to Bob. Under the hood, Alice maintains a _dropbox_ branch, associated with the channel, inside her public repository. Bob maintains a branch within his repository that tracks Alice's dropbox and synchronizes with it occasionally, processing previously unseen messages. Communication is signed (and verified) generically at the commit level. Communication can also be encrypted by applying encryption to individual files inside the repository.

### Governance-specific blockchain design

A _blockchain_ is an implementation of a state machine. State changes are driven by asynchronous inputs (such as a "call" to a smart contract method) originating at the miners.

In traditional blockchains, like Ethereum, inputs arising from different miners may result in conflicting changes to the global state. To resolve this conflict, traditional blockchains entail a consensus algorithm which selects and commits the changes proposed by a single miner. The changes proposed by all other miners are discarded and to be attempted at a later time. These conflicts are unavoidable because, by design, different independent applications may try to access the same state and there is no application-level logic that can resolve cross-application conflicts.

In contrast, when a blockchain is dedicated to a single application the potential conflicts arising from inputs at different miners can be resolved at the application level. This observation affords us to design a governance-specific blockchain which sidesteps expensive conflict resolution (and leader election) primitives, such as proof-of-work, proof-of-stake, and so on.

We describe a governance-specific blockchain wherein the miners are the application stakeholders themselves — the community members, in our case. We adopt the [partially synchronous](https://timroughgarden.org/f21/l6.pdf) communication model whereby participant have access to a shared clock (and some reasonable assumptions can be made about network latencies). Note that a shared clock is definitionally required by a governance application, in part because any ballot requiring human input is open for voting during an interval specified relative to the standard time (e.g. GMT).



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