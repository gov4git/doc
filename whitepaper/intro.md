# Governance for git

Our journey into "governance for git" begins with a real-world use case: [The Plurality Book project](plurality.net). The book project was conceived in October 2022 with the objective of crowdsourcing the authoring of a new book on the subject of technologies for digital democracy. By design, this is an open source project aiming to harness the expertise of many loosely (or not at all) related contributors across multiple countries in various capacities, such as translation, fact-checking, original research, discovery of prior art, typesetting, and so on.

Due to the sheer scale of this project — anticipating a number of contributors in the hundreds or thousands, well-beyond the Dunbar limit — an ad-hoc approach to community organization is likely to lead to subpar results. This prompted us to pursue a systemic approach towards designing a flexible and secure solution for governance needs with practicality, low cost, rapid iteration and ease of deployment being non-negotiable requirements.

### Problem statement

On its inception, the book project had a variety of specific community organizing needs — common to many open source projects — along with the expectation that these needs will evolve organically over time and will entail experimentation as the norm. 

The minimal viable set of features largely focused on the ability to manage a community of contributors with various roles; and conduct free, fair and transparent polls for prioritizing work (such as issues or pull requests). Furthermore, we were interested in using novel polling and tallying algorithms (such as the many flavors of Quadratic Voting) which in turn necessitate contextual mechanisms such as community-scoped member accounts (for keeping track of voting credits, badges, and other logical tokens).

Above all, it was clear that governance needs are to be viewed as a moving target: both because the science of DAO mechanisms is still developing, and because a long-lived community will invariably evolve its rules.

This informed us to approach governance as an application, supported by (and decoupled from) an underlying framework which addresses the common responsibilities: decentralized identity, decentralized communication, security, transparency, state replication and consensus.

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

Finally, our application framework provides a mechanism for changing the logic (i.e. the code) of the application itself. 

XXX
