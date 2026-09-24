# Changelog

This file records released AI-Codex changes. Normative behavior is defined by `codex/index.md` and the documents it links.

## v1.1.0

Changes from **v1.0.0**.

### Canon storage and Canonical Revisions

- The declared Canon namespace is now promoted-only. Pre-promotion Candidate/review/correction/staging material lives outside Canon.
- Versioned Canon components use literal immutable Canonical Revisions `R001`, `R002`, `R003`, ... .
- `RNNN` is a Canon/configuration-item revision coordinate, not product SemVer, Git revision, or review-round numbering.
- A stable component `index.md` resolves the current revision and revision history.

### Lifecycle evidence

- Review/promotion evidence for each Canonical Revision must be discoverable through one deterministic interface from the component entry point.
- Working review artifacts remain outside Canon until promotion; canonical/public-safe lifecycle evidence may be materialized during promotion.

### FORMAT-2 machine representation

- Canon remains authored and authoritative in Markdown.
- A generated JSONL/NDJSON FORMAT-2 is now mandatory.
- FORMAT-2 records are self-contained semantic units with stable identity/source mapping; v1.1.0 documents its coarse H2-section granularity, synthetic anchors, and JSONL-local `§N` dependency resolution.
- Selective retrieval is intended to reduce LLM context cost.
- Markdown ↔ FORMAT-2 integrity is checked at lifecycle boundaries, not on every read.

### Open-loop operational contracts

- TASK, Review Assignment, and transported Service Memo use a strict versioned `FIELD: value` envelope for load-bearing data.
- Per-`TYPE` schemas define required/optional, scalar/repeatable, duplicate, lexical, and extension behavior. Repeating a scalar field is invalid; repeatable values are appended in encounter order.
- Complex nested data is referenced as an attachment instead of inventing nested envelope syntax.

### Service Memo

- Literal sender, recipient, and purpose fields are mandatory.
- Memos include a short operational summary plus an artifact map explaining what each referenced file is for.
- One Service Memo is one independently copyable transport unit.

### Bootstrap and assignment validation

- Successful onboarding is acknowledged with a minimal deterministic receipt rather than an unsolicited Codex/promotion-history summary.
- Compiler and Reviewer perform Assignment Preflight before substantive work under an open-loop assignment.
- Reviewer performs no substantive review before an exact Review Assignment.
- Executor performs Execution Contract Validation at TASK intake.

### Role Context Profiles

- Optional non-authoritative reference profiles added:
  - `Researcher` for Compiler;
  - `Skeptic` for Reviewer.
- Profiles are workstream-scoped and cannot modify authority, Dogma, gates, or Canon.

### Migration and compatibility

- Explicit migration guidance added for v1.0.0 projects.
- Historical v1.0.0 decisions keep their original provenance and compliance context.
- Existing promoted components may be normalized into `R001` as the Canonical Revision identity of the already-accepted state without pretending that this creates a historical product/SemVer release.

### Documentation and release packaging

- Added FORMAT-2 reference/schema material.
- Added open-loop protocol/schema registry.
- Added migration guide and Role Context Profiles.
- Added this `CHANGELOG.md`.
- AI-Codex release Candidates now undergo a complete-package version sweep so public documentation cannot silently remain on an earlier current-version claim.

### Intentionally preserved from v1.0.0

- Role != Actor.
- Candidate-scoped Reviewer independence.
- Exact Frozen Revision review.
- N=1 / N>1 review semantics and Stage 2 gate-bearing rule.
- No majority-vote override of a valid BLOCK.
- Product Owner `PROMOTE` remains separate from Reviewer PASS.
- Typed normative precedence.
- Mandatory independent implementation review.
- Public-project Working/Canon repository separation.

---

## v1.0.0

Initial public release establishing the core Decision Storage System, Decision Methodology, Implementation Methodology, Roles and Authority, Review and Gates, and Project Adoption/Versioning model.
