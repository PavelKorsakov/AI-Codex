# Rationale and Rejected Alternatives

Release target: **AI-Codex v1.1.0**

Authoritative lifecycle status is external to this immutable payload and is resolved through the applicable Promotion Record.

This document preserves the generalized rationale needed to understand the normative package without exposing private development history.

## Role obligations are independent of model identity

The standard defines Product Owner, Compiler, Reviewer, Executor, and Adjudicator independently of specific AI products.

**Rejected:** hard-coding one named AI as the permanent designer, reviewer, or executor.

## Review is adversarial, not a vote

A Reviewer attempts to falsify a frozen Candidate. An unresolved BLOCKING finding closes the relevant gate regardless of how many other participants issued PASS.

**Rejected:** majority voting as a fallback for unresolved blocking defects.

## Multiple Reviewers preserve first-pass independence

With more than one blocking Reviewer, independent first passes occur before cross-examination. Only the final post-cross-examination verdict is gate-bearing.

**Rejected:** sequential review where later Reviewers see earlier verdicts before forming their own first judgment.

## Reviewer independence is Candidate-scoped

An Actor that produced a Candidate cannot supply a blocking Reviewer PASS for that same Candidate. The same Actor may hold different roles on other Candidates or workstreams unless another rule forbids it.

**Rejected:** treating a new chat or session as sufficient independence from one's own work.

## Canon has a deterministic entry point

Every promoted Canon package exposes `index.md` so a zero-memory participant can locate normative content, rationale, review evidence/status, and handoff without repository archaeology.

**Rejected:** allowing every Canon package to invent an unrelated entry structure.

## Public Canon and private Working process are physically separated

For public projects, raw working process stays in a private Working Repository and only intentionally publishable Canon material enters the public Canon Repository.

**Rejected:** routinely committing raw AI working history publicly and later rewriting public Git history to remove it.

## Product authority is typed, not a single hierarchy

AI-Codex Dogma governs process, Project Canon is current project truth, the Project Adoption Declaration configures only permitted choices, the Product Owner has immediate control without silent Canon-rewrite authority, and TASK is a subordinate execution contract.

**Rejected:** both a single universal "highest authority wins" ladder and a "latest instruction wins" rule.

## BLOCKING findings have a defined escape path

A Compiler may disprove a finding with evidence. If disagreement remains, a narrow independent Adjudicator can resolve that finding without becoming a replacement full Reviewer or a Product Owner override.

**Rejected:** making one Reviewer's mistaken BLOCK permanently unresolvable.

## Immutable payload and external Promotion Record

The reviewed payload is not rewritten merely because its lifecycle status later changes. Current status and gate evidence live in an external Promotion Record linked by a stable locator.

**Rejected:** editing "NOT CANON" into "CANON" after review, which would create a new unreviewed payload.


## Canon namespace and Canonical Revisions are explicit

v1.0.0 allowed a project to preserve logical Working/Canon separation inside one private repository but did not explicitly reserve the declared `Canon/` namespace itself for promoted material. Dogfooding showed that good-faith actors could therefore stage Candidates and even review artifacts under `Canon/` before promotion.

v1.1.0 removes that ambiguity: the declared Canon namespace is promoted-only, and every versioned component advances through immutable `R001`, `R002`, ... Canonical Revisions.

**Rejected:** treating `RNNN` as product/SemVer numbering or consuming revision numbers for ordinary Candidate corrections.

## Lifecycle evidence has one deterministic interface

Dogfooding produced several individually traceable Canon components whose review/promotion evidence lived in different locations. Each worked locally, but a zero-memory participant had to learn a different archaeology pattern per component.

v1.1.0 requires deterministic per-Canonical-Revision lifecycle evidence discovery from the component entry point.

**Rejected:** leaving lifecycle evidence location entirely ad hoc after the content requirements themselves are standardized.

## Human-authored Canon and generated machine Canon are separate representations of one semantic state

Markdown remains the authored authoritative representation because Canon includes explanation, rationale, and connective reasoning that humans and Compilers naturally author well there.

FORMAT-2 is generated JSONL/NDJSON containing self-contained semantic records. It exists so machine consumers can selectively retrieve only relevant semantic units without rereading the whole Markdown corpus or comparing both forms on every pass.

**Rejected:** independently hand-authoring both Markdown and machine representation, and excerpt-only indexes that still force ordinary consumers back into Markdown for the actual rule text.

## Open-loop contracts use closed load-bearing fields

A live design conversation can repair ambiguity immediately. A TASK, Review Assignment, or transported Service Memo cannot assume that correction loop exists before downstream work starts.

v1.1.0 therefore uses a deliberately small line-oriented `FIELD: value` envelope with a per-`TYPE` schema for load-bearing data, while keeping optional Markdown for explanation.

**Rejected:** full JSON for every small routing contract, free prose as a substitute for mandatory fields, and allowing the envelope to grow into an improvised YAML-like language.

## Successful onboarding is acknowledged, not performed as a ceremony

Repeated fresh-chat dogfooding showed Actors narrating how they verified Codex history, promotion records, and status even when the Product Owner had only asked them to read the Codex. The verification may still be necessary internally; the unsolicited recital is not.

v1.1.0 requires a minimal deterministic receipt on successful onboarding and full reporting only when an unresolved actionable conflict blocks safe work or when the Product Owner asks for an explanation.

**Rejected:** proof-of-reading essays as a default bootstrap ritual.

## Role Context Profiles are optional and non-authoritative

Compiler and Reviewer benefit from different working postures, but posture must never become a second source of authority. v1.1.0 therefore ships optional reference profiles `Researcher` and `Skeptic`, selected per workstream after Role is known.

**Rejected:** mandatory model-personality policy and vendor-specific normative prompts.
