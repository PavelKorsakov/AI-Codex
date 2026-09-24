# AI-Codex

> An operational standard for collaborative software development by a human and multiple AI agents.

**Current version:** `v1.1.0`

AI-Codex does not describe *how a particular AI should think*. It is about **how to organize collaborative work** so that decisions, tests and reviews, implementation, and project history do not depend on the context memory of a single chat, a single AI model, or a single executor.

This guide defines a shared operating protocol for:

- you, as the creator of the product;
- AI systems acting as architectural and product assistants;
- AI systems acting as independent reviewers and testers;
- AI systems acting as executors and coding agents.

---

## What you need to run AI-Codex

- At least one AI model, preferably two or three.
- A coding agent from any model provider.
- A repository on GitHub or another Git platform.

---

## Why this is needed

Modern AI-assisted development can easily turn into a chain of disconnected conversations where you:

- discuss the product with one AI;
- write code with another;
- let a third review and test the result.

A week later the first chat has exhausted its useful context and the AI starts losing the foundation.

Neither you nor the AI can say with confidence why a particular decision was made.

You open a new chat with the same or another AI agent and, at best, reconstruct part of the context from old conversations.

Some correction made after review quietly becomes a different result that nobody has actually reviewed.

And so on.

AI-Codex is built around a different idea:

> **The source of truth is the repository and the recorded decisions, not chat memory.**

The goal is that a new AI participant can enter a project with zero context, read AI-Codex, read the project's `Canon` section in your repository, receive an assignment, and start working as if it had been present in the project from the beginning.

---

## Core principle

### Role != specific actor

For example:

```text
Product Owner       → Human
Compiler               → AI model A
Reviewer and Tester    → AI model B
Executor               → coding agent
Adjudicator            → independent AI or human
```

Tomorrow the AI models may swap places. Nothing fundamental changes.

AI-Codex does not require ChatGPT, Claude, Grok, Codex, or any other specific system. It defines role obligations, authority boundaries, and the rules for handing work from one role to another.

---

## Basic workflow

```text
          Idea
           │
           ▼
Product Owner + Compiler
(discuss and develop the idea)
           │
           ▼
 Candidate — proposed solution
           │
           ▼
 Independent Reviewer
 reads the Candidate
           │
           ▼
 Returns a judgment
    and decides:
     PASS or BLOCK
           │
           ▼
 Product Owner approves
           │
           ▼
 Decision is recorded
    and becomes
       "Canon"
           │
           ▼
   A task is created
        (TASK)
           │
           ▼
 Executor receives it
           │
           ▼
 Implementation Review
           │
           ▼
      Integration
```

Each transition is a separate state.

For example:

- a Reviewer's `PASS` does not automatically make a Candidate part of Canon;
- `PROMOTE` does not replace review;
- completing implementation does not mean it is ready for integration;
- green tests do not replace independent implementation review.

---

## Main roles

### Product Owner

The human who owns the product decision.

They decide **what is being built**, make product decisions, and separately authorize a reviewed Candidate to enter Canon.

The Product Owner may immediately:

- stop work;
- cancel it;
- pause it;
- reopen a previously accepted question.

But a direct Product Owner instruction must not silently rewrite the current Canon, substitute for a Reviewer `PASS`, or cancel a valid unresolved `BLOCK`.

### Compiler

The Compiler turns an idea or problem into a Candidate that can be reviewed.

Its job is to:

- investigate the question;
- separate facts from assumptions;
- discuss alternatives with the Product Owner;
- state honestly what is still unknown;
- assemble the Candidate;
- freeze an exact Candidate revision for review;
- correct the Candidate after review.

Before substantive work under an open-loop assignment, the Compiler performs **Assignment Preflight**: it checks the assignment against the adopted AI-Codex, project configuration, current Canon, its Role authority, and lifecycle boundaries. If a material conflict exists, the Compiler stops and returns it instead of working around it.

In a fresh context, the Compiler first resolves required bootstrap parameters. If reading the Codex and governing state succeeds, a short readiness receipt is enough: it does not recap the Codex or narrate its verification history unless asked.

A Compiler cannot act as the independent Reviewer of its own Candidate.

### Reviewer

The Reviewer is not supposed to approve the Compiler's work just because it looks reasonable.

Its job is to **try to falsify it**.

The Reviewer looks for:

- contradictions;
- mandatory rules that cannot be executed;
- hidden assumptions;
- ways to bypass required gates;
- violations of independence;
- ambiguities that could lead two good-faith participants to different mandatory actions.

Review is not voting.

One valid unresolved `BLOCKING` finding closes the gate regardless of how many other `PASS` signals exist.

Before substantive review work, the Reviewer performs **Assignment Preflight**. Until an exact Review Assignment identifies the frozen Candidate and authorized reading set, the Reviewer does not begin project analysis, form findings, or infer review scope. It may only acknowledge readiness or report an access/bootstrap problem.

### Executor

The Executor implements an already accepted decision.

It works from an explicit `TASK`. At TASK intake, the Executor first performs **Execution Contract Validation**: a valid contract is accepted and executed; a conflict, missing mandatory field, or load-bearing ambiguity causes STOP and return of the exact defect rather than product redesign by the Executor.

The `TASK` must define:

- the starting state;
- scope;
- allowed and forbidden changes;
- acceptance criteria;
- required checks;
- stop conditions;
- handback requirements.

The Executor must not repair a product or architectural hole on its own "along the way."

If the task conflicts with Canon, or implementation reveals a conceptual defect, the correct action is to **stop and return the question to the decision-making level**.

### Adjudicator

The Adjudicator is not another full Reviewer.

It is used only when the Compiler presents evidence against a specific `BLOCKING` finding, but the Compiler and Reviewer still cannot resolve the disagreement.

The Adjudicator resolves **only that specific disputed issue**.

It does not replace the Reviewer and does not issue `PASS` for the whole Candidate.

---

## Review: PASS, BLOCK, and corrections

Review always applies to an **exact frozen Candidate revision**.

If the Candidate changes after review, it is a new Candidate revision.

The previous `PASS` does not automatically carry over.

Findings are divided into two types:

- **BLOCKING** — the problem prevents the Candidate from moving forward;
- **NON-BLOCKING** — the issue exists, but the mandatory process remains correct, understandable, and executable.

The Compiler may disagree with a finding and present evidence.

The Reviewer may then:

- withdraw the finding;
- downgrade it to `NON-BLOCKING`;
- keep it `BLOCKING`.

If the disagreement remains unresolved, an Adjudicator may be asked to resolve that specific finding.

---

## Multiple Reviewers

AI-Codex supports one or several independent Reviewers.

With `N = 1`, the normal loop is:

```text
Candidate → Review → Correction → Review
```

With `N > 1`:

1. each Reviewer first checks the same frozen Candidate independently;
2. only after those independent reviews may they see each other's conclusions;
3. cross-examination begins;
4. Reviewers check not only the Candidate but also the material conclusions of their peers;
5. each Reviewer then forms a final verdict.

Majority voting is not used.

If even one valid `BLOCKING` finding remains unresolved, the Candidate cannot move forward.

---

## Dogma and Interpretation

AI-Codex separates the mandatory core of the methodology from freedom in implementation.

### Dogma

Any normative requirement marked with `MUST` is mandatory for a project that calls itself **AI-Codex compliant**.

These rules cannot be silently cancelled by a local agreement.

### Interpretation

A project may choose its own:

- Git workflow;
- structure of supporting files;
- number of Reviewers where it is not fixed;
- naming rules;
- specific AI models;
- tools;
- CI;
- reporting format;
- other local mechanisms.

The only condition is that the chosen implementation must not violate Dogma.

A project may intentionally depart from mandatory AI-Codex requirements.

But in that case it must identify itself honestly as:

- `AI-Codex-derived`;
- or `AI-Codex-modified`;

rather than fully AI-Codex compliant.

---

## Rule precedence

AI-Codex does not use the primitive rule:

> "Whoever is more senior is right."

Different layers have **different kinds of authority**.

| Layer | What it determines |
|---|---|
| **AI-Codex Dogma** | How the process itself must work |
| **Project configuration** | Which permitted process choices are selected for this project |
| **Project Canon** | What is currently accepted as project truth |
| **Product Owner** | May immediately control work and initiate a Canon change |
| **TASK** | The concrete execution assignment |

For example, a direct Product Owner instruction may stop work immediately.

But it does not automatically turn new text into Canon.

To change Canon, a new decision must pass through the normal cycle:

```text
Candidate → Review → PROMOTE → Canon
```

Likewise, a new `TASK` that conflicts with the current Canon does not win simply because it was written later.

The Executor must stop and return the conflict.

---

## Canon and the Working Repository

For public projects, AI-Codex requires two physically separate repositories or repository planes.

### Private Working Repository

This is where the following live:

- drafts;
- discussions;
- intermediate hypotheses;
- Compiler workspaces;
- Reviewer materials;
- internal service memos;
- correction loops;
- the full working provenance of decisions.

This is the project's kitchen.

Participants need it, but it does not need to become public.

### Public Canon Repository

This contains only:

- intentionally published Canon;
- public documentation;
- required review and promotion records;
- published releases.

The idea is simple:

> Do not publish the whole kitchen first and then try to scrub it out of Git history.

Public Canon should be assembled from material intended for publication from the start.

---

## Single entry point: `index.md`

Every Canon package must have one predictable entry point:

`index.md`

A new participant with zero context must be able to use that file to find:

- mandatory documents;
- scope and boundaries;
- the reasoning behind accepted decisions;
- material rejected alternatives;
- review results;
- current lifecycle state;
- handoff information and source of truth.

This is the minimum stable interface of Canon.

The internal structure of the remaining documents may evolve.

---

## Immutable package and Promotion Record

A reviewed Candidate must not be rewritten after review merely because its lifecycle state changed.

For example, if after `PROMOTE` a reviewed file changes from:

```text
CANDIDATE — NOT CANON
```

to:

```text
CANON
```

it is formally a different file that the Reviewer did not review.

AI-Codex therefore separates two things:

1. the **immutable reviewed package**;
2. an external **Promotion Record**.

The package itself remains exactly what passed review.

The separate Promotion Record records:

- Compiler check results;
- independent Reviewer `PASS` or `BLOCK`;
- readiness for promotion;
- the Product Owner's `PROMOTE` command;
- the published release identity.

This prevents a lifecycle-state change from turning a reviewed package into a new unreviewed revision.

---

## How to start using AI-Codex

A minimal path looks like this:

1. Read [`codex/index.md`](codex/index.md).
2. Select and pin an exact AI-Codex version.
3. Record the AI-Codex configuration for your project.
4. Assign concrete participants to the required roles.
5. Create the Project Canon.
6. For every separate workstream, specify:
   - the participant's role;
   - the exact Candidate or starting baseline;
   - what material the participant is allowed to read;
   - how many Reviewers are assigned;
   - where the result must be written;
   - what repository actions are permitted.
7. Before implementation, give the Executor an explicit `TASK`.
8. Do not integrate implementation without independent review.

After that, a new AI participant should need only an instruction in roughly this form:

> Before starting work, read the adopted AI-Codex and the Project Canon.  
> Your role in this workstream is: `<ROLE>`.

The specific assignment should provide the parameters of the current work, not retell the entire methodology.

---

## What AI-Codex does not do

AI-Codex:

- does not choose AI models for you;
- does not force different models to think in the same way;
- does not define the architecture of your product;
- does not replace the Product Owner;
- does not require bureaucracy for its own sake;
- does not guarantee the absence of errors;
- does not require one specific Git workflow;
- does not bind the project to one AI provider;
- does not treat chat history as reliable long-term storage for project truth.

It defines **process boundaries, verifiable states, and rules for transferring responsibility**.

---

## Repository structure

A public AI-Codex release is organized roughly like this:

```text
README.md
README-RU.md
CHANGELOG.md
LICENSE
CREDITS.md
RELEASE-MANIFEST.md

codex/
  index.md
  README.md
  00-CODEX-SCOPE-AND-TERMS.md
  10-DECISION-STORAGE-SYSTEM.md
  20-DECISION-METHODOLOGY.md
  30-IMPLEMENTATION-METHODOLOGY.md
  40-ROLES-AND-AUTHORITY.md
  50-REVIEW-AND-GATES.md
  60-PROJECT-ADOPTION-AND-VERSIONING.md
  RATIONALE.md
  ADOPTION.md
  MIGRATION-v1.0.0-to-v1.1.0.md
  FORMAT2.md
  OPEN-LOOP-PROTOCOL.md
  CONTENT-MANIFEST.md
  schemas/
  profiles/
  format2/

promotion/
review/
```

For mandatory rules, the source of truth is `codex/index.md` and the documents it references.

The README introduces the project to people but does not replace Canon.

---

## Versioning

A project using AI-Codex must pin a concrete version instead of referring to "the latest AI-Codex."

For example:

```text
AI-Codex version: v1.1.0
Commit: <public commit identifier>
```

Moving a project to a newer AI-Codex version is a separate conscious action.

A new AI-Codex version does not retroactively rewrite the rules under which earlier decisions were made.

---

## Project status

**AI-Codex v1.1.0**

v1.1.0 preserves the base model established in v1.0.0 and adds rules found through real dogfooding:

- the declared `Canon/` namespace is promoted-only;
- every versioned Canon component uses immutable `R001`, `R002`, ... Canonical Revisions;
- lifecycle evidence has one deterministic discovery interface;
- Markdown remains authored Canon while FORMAT-2 provides generated JSONL/NDJSON machine records;
- TASK / Review Assignment / Service Memo use closed, validatable `FIELD: value` load-bearing fields;
- bootstrap is minimal and does not narrate the Codex unless asked;
- Compiler/Reviewer use Assignment Preflight, while Executor uses Execution Contract Validation;
- optional `Researcher` and `Skeptic` Role Context Profiles are included;
- migration guidance, schemas, and a CHANGELOG are part of the release package.

---

## Languages

Russian `README-RU.md` is the source authorial text of the overview documentation for `v1.1.0`.

English `README.md` is a translation of the same public concept.

The normative AI-Codex corpus is published separately from the overview README and remains the source of mandatory rules.

---

## License

© 2026 Pavel Korsakov

Created by Pavel Korsakov & AI Group.

Unless a specific file or directory explicitly states otherwise, the textual materials in this repository are licensed under the **Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)** license.

Future software code MAY be released under a separate software license explicitly stated for the relevant file or directory.

See [`LICENSE`](LICENSE) for the full licensing notice. See [`CREDITS.md`](CREDITS.md) for the members and contributions of the AI Group.

---

**AI-Codex does not try to make AI infallible. It makes errors, decisions, reviews, and handoffs visible, reproducible, and manageable.**
