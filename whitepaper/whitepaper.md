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

In general, a _blockchain_ is an implementation of a state machine that entails secure replication by miners. State changes, recorded in the blocks of a blockchain, are ultimately driven by asynchronous requests (such as a "call" to a smart contract method) by users, which are processed by miners and packaged into proposals, known as transactions.

Traditional multi-application blockchains, like Ethereum, first compute the effect of each individual user request to the blockchain's state, and then they propose the resulting state change (in the form of a transaction) to all participating miners. Since user requests concur at different miners, they can result in conflicting changes of state (i.e. transactions). To remedy this issue, blockchains entail mechanisms (such as proof-of-work or proof-of-stake, for example) for picking a winner, which are oblivious to the underlying applications or semantics of user requests. These oblivious mechanisms are the sole reason for the high cost of running a general-purpose multi-application blockchain, either infrastructure-wise or financially.

In contrast, in the case of sovereign community governance, the blockchain miners are the community stakeholders (or a subset thereof) themselves. In this case, the blockchain is serving a single application and furthermore, by definition, it is the governance application's responsibility to arbitrate conflicting requests by its members — not the blockchain's.

This crucial difference enables us to design a blockchain architecture for governance which sidesteps expensive conflict resolution operations, while preserving all security guarantees enjoyed by traditional blockchains. As a result, our governance blockchain can be deployed on cheap commodity hardware (such as Raspberry Pi) or a pre-existing git hosting provider, and it does not require connectivity to the global Internet.

The key architectural difference between our governance blockchain and a traditional blockchain is in the order of operations involved in processing user requests. When user requests arrive at the miners, the miners share all user requests unprocessed using a Byzantine fault-tolerant broadcast protocol. Once all miners reliably agree on the totality of all user requests in an epoch, each miner independently computes the change in state by invoking the governance application and providing it with all user requests. 
Since the governance application is deterministic, all miners compute the same change. This protocol effectively pushes the responsibility of resolving conflicts between user requests to the governance application.

### Technical outline

A governance blockchain consists of a set of miners. Each miner maintains a replica of the blockchain, which holds the state of the governance application. The state of the application specifies the current set of participating miners, and this set can change in the normal course of the blockchain execution. Miners would typically be stakeholders in the community, but they could also be third parties that are incentivized exogenously.

We adopt the [partially synchronous](https://timroughgarden.org/f21/l6.pdf) communication model whereby participants have access to a shared clock (and some reasonable assumptions can be made about network latencies). Note that a shared clock is definitionally required by a governance application, in part because any ballot requiring human input is open for voting during an interval specified relative to the standard time (e.g. GMT).

Time is logically divided into epochs, corresponding to regular intervals of wall clock time, such as one hour, for instance. During each epoch, miners independently collect requests from community users. At the end of the epoch, miners share all user requests using [reliable Byzantine broadcast](https://groups.csail.mit.edu/tds/papers/Lynch/jacm88.pdf) for the partially synchronous model. Once all miners agree on all user requests, each miner privately computes the change to the blockchain state by invoking the governance application and supplying it with all user requests that occurred during the epoch. All miners are guaranteed to arrive at the same state, as the governance application is required to be deterministic with respect to its inputs.

### Infrastructure requirements

In every blockchain miner nodes require storage infrastructure to persist replicas of the blockchain, and compute infrastructure to participate in the protocol and make changes to the blockchain. 

Traditional general purpose blockchains have demanding infrastructure requirements. A typical miner deployment requires multiple high-end GPU machines as well as a sophisticated reliable storage solution, such as a cloud file system or key value store. These requirements may be prohibitively expensive for small and mid-size non-technical communities. Furthermore, such infrastructure may be entirely unavailable in the developing world, or it may be prohibitively complex for non-technical users to operate or administer.

Our design of a governance blockchain alleviates both of these pain points. A governance miner node requires a single commodity machine (such as a home Raspberry Pi or a free-tier GitHub action runner) for compute, and access to a networked git repository for storage (which could be provided by a local business, such as GitHub, or deployed from home using any Linux distribution). 

Our blockchain can execute on modest hardware because it does not entail general-purpose conflict resolution algorithms, such as proof-of-work or proof-of-storage.

Our choice of git as a storage backend is informed by a few considerations. The git protocol is possibly the most ubiquitous general-purpose storage API. Free-tier git hosting is available in most countries and in lieu of this, git can be hosted "at home" from a standard Linux distribution. In contrast, higher-end storage APIs such as networked file systems or databases are provider-specific, and significantly harder to deploy, operate and migrate from.

Governance blockchains require orders of magnitude less space than general-purpose ones, because they record bookkeeping information that directly describes human actions such as casting a vote or requesting a balance transfer, for example. As a result, the storage capacity of a typical single-machine git deployment is more than adequate.

Finally, by using git as a chain storage and JSON as a data encoding we ensure that the blockchain's state history can be audited (both manually and programmatically) with standard tooling.

### Trust and security model


## TODO
- application model
