# Governance for git

Our journey into "governance for git" begins with a real-world use case: [The Plurality Book project](plurality.net). The book project was conceived in October 2022 with the objective of crowdsourcing the authoring of a new book on the subject of technologies for digital democracy. By design, this is an open source project aiming to harness the expertise of many loosely (or not at all) related contributors across multiple countries in various capacities, such as translation, fact-checking, original research, discovery of prior art, typesetting, and so on.

Due to the sheer scale of this project — anticipating a number of contributors in the hundreds or thousands, well-beyond the Dunbar limit — an ad-hoc approach to community organization is likely to lead to subpar results. This prompted us to pursue a systemic approach towards designing a flexible and secure solution for governance needs with practicality, low cost, rapid iteration and ease of deployment being non-negotiable requirements.

### Problem statement

On its inception, the book project had a variety of specific community organizing needs — common to many open source projects — along with the expectation that these needs will evolve organically over time and will entail experimentation as the norm. 

The minimal viable set of features largely focused on the ability to manage a community of contributors with various roles; and conduct free, fair and transparent polls for prioritizing work (such as issues or pull requests). Furthermore, we were interested in using novel polling and tallying algorithms (such as the many flavors of Quadratic Voting) which in turn necessitate contextual mechanisms such as community-scoped member accounts (for keeping track of voting credits, badges, and other logical tokens).

Above all, it was clear that governance needs are to be viewed as a moving target: both because the science of DAO mechanisms is still developing, and because a long-lived community will invariably evolve its rules.

This informed us to approach governance as an application, supported by (and decoupled from) an underlying framework which addresses the common responsibilities: decentralized identity, decentralized communication, security, transparency, state replication, consensus and a principled mechanism for changing the program of governance itself during the normal life of the application.

### Practical constraints

We take a somewhat uncommon (and ambitious) aim at serving the tail end of "disadvantaged" users who may live in regions of the world that are disconnected from the global Internet and where commodity hardware may be the only available.

Anecdotally, we would like to enable a group of people — living in a war-torn or disaster zone with local connectivity — to form an open source community and crowd-source vital information securely.

In reality, our constraints address a much larger set of use cases. For instance, any decentralized governance solution based on public blockchain technology (which requires connection to the global Internet) can be incapacitated by authoritarian states: Public blockchains have well-known addresses which can be fire-walled surgically.

In further consideration of the heavy tail of users, we insist that our solutions are accessible to "non-technical" users. Every decentralized application entails some technical responsibility of its users. We assume that a "non-technical" user is able to provision and administer a git repository. Whereas a user able to provision and operate a container virtual machine — a fairly involved process, in comparison — is considered "technical".
Our goal is to design governance software which targets non-technical users, both in the roles of community organizers and community members.

Our next consideration pertains to security and trust. Communities, especially if successful, grow large. Large groups of people are more likely to be comprised of strangers. It is therefore the software's responsibility to ensure members' rights are respected in that governance is conducted as advertised. In practice, governance is codified as a computer program which evolves its state in response to messages from the community members and clock events. Fair governance equates to faithful and correct execution of said program.

We adopt the current gold standard for faithful state machine execution: Byzantine fault tolerance. In the context of security, we assume that a community utilizing our software will comprise a set of one or more designated "trusted" users. These users will take on a differentiated responsibility of running the (minimal) infrastructure required for the sustenance of the replicated state of the governance application. These users might be appropriately called "miners", reflecting the fact that their role is analogous to the role of miners in a public blockchain. In particular, we require that governance execution is provably correct and lively unless more than 33% of miners fail in the Byzantine sense (i.e. hardware failure or adversarial behavior).

### Application model

We view governance as a single-threaded deterministic program (aka a state machine), whose execution is driven by exogenous events of two types: communication from community members or wall clock events.

In our design, the application logic is invoked at regular wall clock intervals, such as once per hour, which are called rounds. During each invocation, the application is provided with a list of all events (i.e. communication from community participants) that have occurred since the last round. The application is then free to process these events as a batch and update its state.

Note that this application execution model is _different_ from the execution model of smart contracts on blockchains like Ethereum. A smart contract processes events (which represent smart contract method calls) one at a time. In contrast, our governance application can "see" the entire batch of events arising during a round, which enables it to handle conflicts at the application level.

Due to the community focus of governance applications, our framework maintains an explicit list of all community members. Only members can communicate with the governance application. The membership list is shared between the application and the underlying (security and communication) framework, and the application _can_ change this list at its discretion. For instance, the application can add a new member after they have been approved with a majority vote from the current community members. Similarly, the list of governance miners is shared between the application and the framework, and can be modified by the application.

Finally, our application framework provides a principled mechanism for changing the logic (i.e. the code) of the application itself. We believe that the fundamental function of any long-lived governance system is the ability to mediate its own change. In our software architecture, the rule of governance is embodied in the application layer and specifically its program. Whereas the underlying software framework is responsible for the immutable technical concerns of governance — execution, identity, communication, Byzantine fault tolerance, and others.

As a part of its state, our underlying framework maintains an explicit reference to the application's current logic, for instance, using content-addressable links to its source code and binary distributions. This reference is a shared state between the application and the framework, in the sense that the application can view it and mutate it as part of its normal execution. This enables applications to implement workflows like this one:
- community members change the source of their governance application and submit a pull-request to the community governance blockchain
- in response, a referendum is held to approve the changes
- if approved, the running governance application is replaced with the new one for the next round of the blockchain execution

In summary, a governance system can be broadly characterized in two ways:
- Governance is a decentralized social application in that it entails asynchronous interactions with the decentralized identities of its participants, and
- Governance is a Byzantine fault-tolerant replicated state machine

In the remainder of this paper we describe the technical architecture underpinning our software framework for governance. Specifically, we describe our infrastructure requirements and how our framework implements (a) decentralized social communication and (b) Byzantine fault-tolerant state replication.

### Framework: Decentralized social applications over git

By definition governance is a social application. It entails multiple stakeholders — such as contributors, organizers, even the organization itself — asynchronously communicating on matters of governance. For instance, a contributor may send their vote on a ballot administered by the organization; the organization may send notifications to contributors about a pending referendum on a pull request approval; or one contributor may simply want to engage in a private chat session with another.

Any application involving communication requires an identity system and a mechanism for secure and verifiable communication. The tension in designing a practical architecture for a decentralized application lies between two opposing objectives:

- On the one hand, we aspire to impose minimal infrastructural requirements on the end users, who are ultimately responsible for owning and managing their instances (of execution and/or data) in the application.

- On the other hand, we aspire to provide a method of communication which is secure, verifiable, efficient and fast.

No solution is perfect, rather different trade-offs exist. For instance, in [Solid](https://solidproject.org/) Tim Berners-Lee posits that the user's instance of a decentralized application should be encapsulated in a container, respectively making end users responsible for managing container-level technology and migratory routines. This burden to the user is somewhat mitigated by the use of a specialized deployment provider, in this case [Inrupt](https://www.inrupt.com/). However, this comes at the cost of a significant reduction in access to this technology and diversity of providers. Citizens of most countries cannot purchase Inrupt services, and geographic diversity of Inrupt services is predicated on using one of the cloud giants.

An alternative architectural approach is federation, as used by services such as [Mastodon](https://joinmastodon.org/) and [Matrix](https://matrix.org/). This approach relieves end users from managing infrastructure, but it requires them to associate with a federation server which comes with some downsides. Federated servers generally provide lower performance and lower reliability guarantees than analogous cloud services. It may be hard to migrate a user's identity or application history from one server to another, as URLs are often server-dependent. Finally, the [deployment requirements of federated systems such as Mastodon](https://docs.joinmastodon.org/user/run-your-own/) are so dependent on higher-level cloud services that in practice the only places they can be deployed are the cloud giants, which reduces the diversity of their availability as was the case with Inrupt's approach.

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

### Framework: Governance-specific blockchain over git

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
