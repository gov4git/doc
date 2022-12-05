# Governance for git

Our journey into "governance for git" begins with a real-world use case: [The Plurality Book project](XXX). The book project was conceived in October 2022 with the objective of crowdsourcing the authoring of a new book on the subject of technologies for digital democracy. By design, this is an open source project aiming to harness the expertise of many loosely (or not at all) related contributors across multiple countries in various capacities, such as translation, fact-checking, original research, discovery of prior art, typesetting, and so on.

Due to the sheer scale of this project — anticipating a number of contributors in the hundreds or thousands, well-beyond the Dunbar limit — an ad-hoc approach to community organization is likely to lead to subpar results. This prompted us to pursue a systemic approach towards designing a flexible and secure solution for governance needs with practicality and rapid path-to-deployment being non-negotiable requirements.

### Problem statement

From an application point of view, the book project needs to support a variety of community organizational procedures, such as:

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

We determined that this initial list of functionalities will suffice for an MVP and will go a long way in facilitating the book project, as well as potentially other projects where community-sourced prioritization is essential, such as the [Filecoin specification proposals](XXX) project.

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



<hr>
- large scale open source community organizing

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