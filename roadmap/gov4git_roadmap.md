
# Roadmap

## Phase 1: Blank slate to MVP launch on the Plurality Book project (ongoing, started Sep 2022)

Phase 1 captures the development of the first MVP release of gov4git, starting from scratch and ending with the launch of gov4git on the Plurality Book project.

In general, applications based on git comprise:
- a protocol specification, which specifies how to interpret the contents of a git repository, along with
- a client implementation, which provides a user-facing functional API to the backend protocol.

Our reference gov4git client is implemented in Go, and it can be used as a command-line tool or as a Go library.

In this phase we build the first prototype of gov4git, including:
- A software library for writing multi-user/decentralized applications on top of git
- A community governance framework and application, deployable using a single trusted party, including an initial set of governance application features:
  - Polling and conducting referendums
  - Programmable balloting, initially with support for Quadratic Voting
  - Token (e.g. voting credits, badges, etc.) account services (e.g. balance transfers) for members
- A user interface in the form of a web extension, integrated with GitHub, allowing voting operations on issues and pull requests


### Milestone 1.1: Quadratic Voting for prioritization polling (finished, Oct 2022)

This milestone completed at end of Oct 2022.

#### Develop
- Decentralized apps over git framework:
  - [x] Identity over git and DNS
  - [x] Signed mail over git
- Community management:
  - [x] User and group management
  - [x] User balances
- Governance:
  - [x] Verifiable ballots: voting, tallying
  - [x] QV-based tallying

#### Validation
- [x] Demonstration to RadicalXChange (Glen/Leon/Alex)
- Feedback:
  - Aligned on voting model. 
    - Glen's use case:
      - Community organizer distributes voting credits
      - Users can up/down vote open PRs and issues
      - PR/issue closure credits/debits user voting accounts
  - Web UI required for live deployment with Plurality Book users

### Milestone 1.2: Framework rewrite + community features (finished, Nov 2022)

#### Develop
- Decentralized apps over git framework:
     - [x] Rewrite to enable easy extensibility and rapid feature development
       - [x] Identity
       - [x] User and group management
       - [x] Signed mail
       - [x] RPC over signed git mail
     - [x] Generic data structures over git (key-value, etc)
     - [x] Local on-disk repo cache at the client
     - [x] Support for embedding and tracking repos within other repos
- Community services:
  - [x] Bureau: governance operational proposals by users (an analog of smart contract method calls)
    - [x] User-initiated balance transfers
- Governance:
  - [x] User balances
  - [x] Balance holds during ballots
  - [x] Balance deductions on ballot closure/clearance

#### Validation
- [x] Dogfood deployment on gov4git repo
  - [x] Dogfooder docs
  - [x] Invite dogfooders (PL/RadX/GitHub)

### Milestone 1.3: Document and socialize core design (finished, March 2023)

Short-form:
- [x] Slides on gov4git
- [x] Slides on decentralized apps over git
- [x] Slides on trusted computation over git

Long-form:
- [x] Tutorial video for command-line interface
- [x] Whitepaper

### Milestone 1.4: Web app and browser extension for voting (ongoing, targeting July 2023)

The gov4git command-line client supports all features of the gov4git protocol and can be used by all participants in an open-source community regardless of their role (e.g. organizer or collaborator). Nevertheless, the command-line interface is not appropriate for communities whose contributing members are non-technical users, such as the case for the Plurality Book project.

To facilitate participation of non-technical contributors, we develop a web-based client and UI supporting a subset of the gov4git protocol relevant to contributors.

#### Develop
- [ ] Stand-alone TypeScript client library:
  - [ ] View open ballots and current tallies
  - [ ] View user balances
  - [ ] Vote
  - [ ] Transfer balances
- [ ] Web app
  - [ ] Dashboards for PR/issue prioritization based on current tallies
- [ ] Chrome extension for GitHub

#### Validation
- [ ] Dogfood on gov4git repo
- [ ] Dogfood on Plurality Book project


## Phase 2: Support Plurality Book deployment and development of new mechanisms (expected to start Sep 2023)

Following the launch of gov4git on the Plurality Book project, we expect a variety of support, maintenance and usability improvement tasks:
- Support and troubleshooting for the live deployment
- Development of admin integrations:
  - Export/import voting data to Excel spreadsheets
- Development of new governance mechanisms, such as:
  -  Soulbound tokens (SBTs) and other ZK primitives
  -  Different flavors of quadratic voting, e.g. voting with discounting for diversity (based on SBTs)

### Milestone 2.1: Support, troubleshooting, due diligence

This milestone captures the workload associated with supporting the live deployment at the Plurality Book, troubleshooting, fixing bugs, and making incremental improvements to our underlying framework to reduce future technical debt and operational costs.

Over time, this milestone will capture a growing list of improvements to the application framework, such as:
  - [ ] Sign commits at the framework level (rather than the application level)

### Milestone 2.2: ZK primitives

ZK-SNARK/STARK primitives are the building blocks of "smart documents" such as, for instance, certificates that are viewable and verifiable by designated parties. New types of smart documents, such as Soulbound Tokens, are the building blocks of various DeSoc (decentralized society) community mechanisms.

This milestone aims to adopt a ZK development technology — of which there are a few in the open-source Web3 community — and build the first prototypes of Soulbound Tokens for gov4git, as well as to establish a methodology for interoperating with existing ZK primitives on public blockchains.

### Milestone 2.3: Mechanism laboratory

We expect that the Plurality Book project may be interested in experimenting with different voting systems or entirely new mechanisms at the application level. For instance, one might be interested in normalizing voting power to meet dynamic constraints such as, for instance, ensuring that one group of members as a whole has equal voting weight as another. This milestone is a placeholder for any work in this category.

## Phase 3: Transition from benevolent dictator to fully plural governance

The goal of this phase is to transition away from relying on a benevolent dictator and hand off governance to the community.

Up until this phase, a benevolent dictator is relied upon both at the infra and the app level:
- at the infra level, to own and operate the git repo holding the state (and history) of governance as well as to execute the governance program regularly and faithfully
- at the app level, to perform governance duties (approve/revoke memberships, approval pull requests, initiate and tally ballots, etc.)

This phase upgrades the infra and the app layers independently, resulting in a system that can be governed entirely by the community.
- at the infra level, the state of governance is replicated on multiple git repos owned and operated by multiple trusted parties, bound by a Byzantine state machine execution protocol
- at the app level, all governance operations are associated with a policy specifying the requirements for performing the operation; a variety of policies are supported, such as majority via quadratic vote, quorum within a group of members, approval from anyone from a group of members, and so on.

### Milestone 3.1: Byzantine replicated state machine execution

This milestone implements a synchronous replicated state machine for a single application, using basic synchronous Byzantine broadcast to guarantee soundness and liveness. This blockchain (for short) is deployed by a set of trusted community parties, called validators.

State replicas are held in public git repos operated by the validators, whereas state transitions are performed by hourly cronjobs executed on the client devices of the validators.

The blockchain invokes the application on every hour, providing it with its current state and all user (community member) requests since the previous hour. The application is responsible for mutating the state and communicating it back to the blockchain.

The application is abstracted as a standard OS executable that takes two input arguments: a directory holding the current state, and a directory holding incoming user requests. The application mutates the state directory and completes execution.

### Milestone 3.2: Governance policies

At the application level, a governance application constitutes an ever-growing set of performable operations, also known as the governance API. For example:
- approving or revoking community members
- initiating and tallying polls or referendums
- assigning tokens to community members
- setting roles and access permissions to community members, and
- many more

Every governance operation must be associated with a policy, which dictates under what conditions the operation can be performed. Policies can be composed out of an adequate set of building blocks, for example:
- majority using a quadratic vote from a group of members
- quorum of a set size from a group of members
- approval from any one of a group of members

### Milestone 3.3: Source code policies

Operations over the community's source code, such as merging changes (pull requests), are also to be governed by policies. However, source code operations require a fine-grain policy control — at the directory level, rather than the commit level.

We adopt a design inspired by the ownership system of Google's internal monorepo source code system, known as Google3. Every directory contains an optional `POLICY` file which specifies the approval process required to make code changes within its subtree.

### Milestone 3.4: Auditing

The by-product of deploying a governance application — alongside the community's open source project — is a governance repo, containing a verifiable history of the deliberation and decision-making that resulted in the evolution and the current state of the source repository. In other words, the governance repo is a certificate that the source repo was developed while respecting the community's self-elected rules.

To make practical use of this "certificate", we introduce a tool which audits the source repo in conjunction with the governance repo and reports on all changes (in the source or government repo) that were made in an extra-judicial manner (i.e. without following governance policy).

From a technical standpoint, auditing comprises two steps:
- First, the execution history of the governance application (a state machine) is replayed from the genesis commit (the initial state) to verify that it matches the contents of the governance repo.
- Second, for every change (i.e. git commit) in the source code repository it is verified that there is a matching record in the governance repo, proving that policy was followed.

## Phase 4: Sound governance mechanisms (research direction)

Governance mechanisms are essentially multi-party protocols aiming to attain desired outcomes at the community-level. 

As the complexity of governance protocols rises, much like in the case of networking protocols, it becomes difficult for developers to assertain that their mechanism implementations are correct and function as intended towards community level goals. 

To address this problem, we adapt techniques from software verification to governance design.

Our governance framework already abstracts a governance application as a standard binary which mutates a file system. This enables mechanism developers to use any programming technology, but in particular verification languages such as the Lean Theorem Prover language developed by Microsoft Research. 

This seamless integration lays out the possibility of developing governance mechanisms whose logical correctness is formally provable using the same tools that have verified the correctness of some of the deepest results in Mathematics. 

