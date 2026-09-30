# 10 — Decision Storage System

Release target: **AI-Codex v1.1.0**

Authoritative lifecycle status is evidenced by the applicable Promotion Record. At `PROMOTE`, any embedded lifecycle self-reference MUST be made consistent through the narrowly scoped Promotion Lifecycle Normalization defined in `10-DECISION-STORAGE-SYSTEM.md` §8.

This document answers where and how a decision physically lives, not whether it is intellectually good. Intellectual honesty is governed by `20-DECISION-METHODOLOGY.md`; sufficiency of challenge is governed by `50-REVIEW-AND-GATES.md`.

## 1. Repository architecture

For any project with a public Canon, physical repository separation is mandatory Dogma:

- a **private Working Repository** — Compiler and Reviewer workspaces, raw process history, cross-model drafts, internal evidence, intermediate synthesis, review working materials, service-memo source material;
- a **public Canon Repository** — only promoted Canon, required canonical decision history, public templates, public handoff material, and other material deliberately intended for publication.

Raw working process MUST NOT enter the public Canon Repository's history. Promotion is a controlled, one-way publication step from the Working Repository into the Canon Repository, performed only after all required review gates are satisfied and the Product Owner has separately authorized it (`40-ROLES-AND-AUTHORITY.md` §3). Because raw process material never enters the Canon Repository's history under this architecture, no routine rewriting of public Git history is required as part of normal operation.

For a fully private project, physical separation into two repositories is not mandatory: the working area and Canon MAY coexist in one private repository, provided the logical distinction between Workspace material and Canon (§2) remains explicit.

### 1.1. Canon namespace is promoted-only

A project's declared Canon namespace MUST contain promoted Canonical material only. Pre-promotion Working drafts, Candidates, review artifacts, correction artifacts, staging manifests, pre-promotion lifecycle records, and other Working material MUST live outside that namespace even when they are expected to be promoted later. Private repository topology does not weaken this logical boundary.

## 2. Logical classes of information

Independent of repository topology, three logical classes exist:

1. **Working** — raw process material in a Compiler's or Reviewer's own workspace: drafts, intermediate hypotheses, ping-pong, abandoned branches. Mutable, may contain noise, not authoritative.
2. **Canonical decision history** — the distilled history necessary to understand an accepted decision: scope/context, rationale, material rejected alternatives, review artifacts, correction closure, boundaries, handoff. Promoted together with the decision.
3. **Current Canon** — the normative decision content itself, as it currently stands.

Classes 2 and 3 are promoted together as one package and both become project-owned on promotion. Class 1 is never promoted and does not travel with the package.

## 3. Candidate lifecycle

```text
Workspace (Working, outside Canon)
    ↓ Compiler prepares authored Markdown and freezes a revision
Frozen Candidate
    ↓ generate/validate FORMAT-2 from authored Markdown; validate manifest/source mapping
    ↓ Compiler-owned public-disclosure check (§6) — disclosure evidence for the exact
      candidate/publication payload MUST exist at this point, before Reviewers verify it
    ↓ review (see 50-REVIEW-AND-GATES.md — for N > 1, Stage 1 then mandatory Stage 2;
      Reviewers verify the disclosure evidence as part of this review)
Correction, if required → new Frozen Revision → disclosure check repeated → review again
    ↓ Compiler PASS + final (Stage 2, where applicable) PASS from every assigned Reviewer,
      on the same Frozen Revision
Promotion-Ready
    ↓ Product Owner PROMOTE (separate act, not a review vote)
    ↓ atomic Promotion Lifecycle Normalization (§8): normalize only allowlisted
      Candidate-era lifecycle/self-reference; regenerate only deterministic derived
      FORMAT-2/manifest identities caused by that normalization
    ↓ public commit/publication — separately blocked if the verified disclosure evidence
      is absent or no longer matches the payload actually being published
Project Canon (lifecycle-consistent Canonical Materialization + promotion evidence)
```

In a **Same-Problem Multi-Compiler** workstream (`20-DECISION-METHODOLOGY.md` §6), "Compiler PASS" in the diagram above requires **PASS from every participating Compiler on the same frozen synthesis revision**, not from a single designated Synthesis Compiler alone (the Same-Problem Multi-Compiler unanimity rule). Compiler-to-Compiler cross-checking of a synthesis candidate is not itself independent Reviewer approval and does not substitute for the Reviewer stage that follows; see `40-ROLES-AND-AUTHORITY.md` §3 and `50-REVIEW-AND-GATES.md` §2.

A later revision does not automatically inherit an earlier revision's `PASS`. A Promotion-Ready candidate whose promotion is postponed does not expire from calendar age alone; before a delayed `PROMOTE`, the Compiler MUST perform and record a freshness check against current baseline, Canon, requirements, and material dependencies, stating what was compared and against what evidence. If no material drift is found, the existing `PASS` states remain valid. If drift is found, the candidate re-enters the appropriate review/correction cycle before promotion. This delayed-promotion materiality trigger is a distinct, narrower rule from the unconditional any-change-requires-re-review rule that governs an actually edited candidate (`50-REVIEW-AND-GATES.md` §2); it applies only to an *unedited* candidate whose promotion was simply postponed.

## 4. Mandatory Canon package: deterministic component entry point and Canonical Revisions

Every versioned Canon component MUST expose one deterministic, human-readable stable entry point:

**`index.md`**

Canonical Revisions MUST use literal identifiers `R001`, `R002`, `R003`, ... . `RNNN` identifies the sequential accepted Canon state of that component/configuration item and is independent of product versions, SemVer, implementation releases, Git revisions, Frozen Candidate identities, and review-round numbers.

Reference layout:

```text
Canon/
  <component>/
    index.md
    R001/
    R002/
    ...
```

The first promoted revision is `R001`; the next is `R002`, and so on. Intermediate Candidate edits, correction rounds, review rounds, Git commits, and failed promotion attempts do not consume Canonical Revision numbers. A newly promoted `RNNN` becomes immutable after its atomic Promotion Lifecycle Normalization completes. Historical `RNNN` directories are otherwise immutable, except for an explicit legacy lifecycle-self-reference normalization migration permitted by `60-PROJECT-ADOPTION-AND-VERSIONING.md` §3. The component-level `index.md` MAY change as part of promotion of a new Canonical Revision or that narrowly defined migration.

A zero-memory contractor, Reviewer, Compiler, or Executor MUST be able to start from that file without repository archaeology. This is Dogma and is now the stable Canon interface the stable-interface rule required; it is no longer an open question whether a stable entry point exists — only its deeper filename/field layout remains open (below).

The package's `index.md` MUST either contain directly, or provide explicit links to, the package's required Canon information categories:

- current normative content;
- scope and boundaries;
- rationale and material rejected alternatives;
- required review/lifecycle evidence for the promoted revision through a deterministic per-revision locator;
- current Canonical Revision and current status;
- handoff / source-of-truth information.

**Proposed, not yet Dogma (reference layout for the material behind `index.md`'s links):** one observed reference instantiation — separate context document, canon body, topical decision register, per-reviewer review artifacts, and a closing handoff document — is a workable Interpretation-level layout a Compiler MAY adopt. No part of this specific layout beyond `index.md` itself is Dogma until a Product Owner decision makes it so.

**OPEN/TBD:** the exact filenames, directory layout, and field-level structure of the documents `index.md` links to remain Interpretation unless separately standardized. A global root Canon index MAY additionally exist across multiple packages, but it does not replace any individual package's own `index.md`. A Compiler MAY preserve additional rationale, diagrams, migrations, threat models, or other material beyond the Dogma minimum where the component warrants it; this additional material is Interpretation.

### 4.1. Deterministic lifecycle-evidence locator

For every Canonical Revision, the component entry point MUST make the corresponding promotion/lifecycle evidence deterministically discoverable without repository archaeology. The evidence MUST identify the Canonical Revision, exact reviewed payload identity, required Compiler gate state(s), blocking Reviewer state(s), Product Owner `PROMOTE`, release/promotion identity where applicable, and post-promotion validation where required.

The literal directory name is a reference mechanism rather than Dogma. Recommended layout:

```text
Canon/<component>/lifecycle/RNNN/
```

Gate-bearing review artifacts are produced outside Canon before promotion. After `PROMOTE`, canonical/public-safe lifecycle evidence MAY be materialized into the deterministic Canon locator while private Working provenance remains preserved separately.

## 5. Canon immutability and superseding

A canonical artifact MUST NOT be silently edited in place as if it were still a draft. A material change creates a new candidate/revision and re-enters the required review lifecycle (§3). A new canonical decision may supersede an earlier one; the earlier decision's history MUST remain traceable rather than being erased.

## 6. Public disclosure control is operational

The general rule for any project with a public Canon Repository is: **only material intentionally approved for public disclosure may cross from the private Working Repository into the public Canon Repository**, and this check applies **before any commit to the public repository** — promotion is not the trigger, the commit is.

This obligation is operational, not aspirational, and follows one deterministic sequence:

1. the Compiler owns performing this check for material it authors, at candidate-freeze time (§3), producing disclosure evidence for the exact candidate/publication payload — before any Reviewer's final verdict;
2. the check MUST produce auditable evidence (what was checked, against what boundary, with what result) rather than a bare assertion that it was done;
3. each assigned Reviewer verifies that evidence as part of its own review, before issuing its final gate-bearing verdict (`50-REVIEW-AND-GATES.md` §6) — this is possible only because the evidence already exists per step 1, not because review happens twice;
4. once all final Reviewer `PASS` states exist, the candidate may become Promotion-Ready (§3);
5. the actual public commit or publication step remains **separately** blocked if the verified disclosure evidence is absent, or no longer matches the payload actually being published (e.g. because correction produced a new revision after the check) — Promotion-Ready and Product Owner `PROMOTE` do not by themselves override this;
6. the project or workstream adoption declaration (`60-PROJECT-ADOPTION-AND-VERSIONING.md` §2) MUST identify who performs and who authorizes this check when it is not simply the authoring Compiler alone.

If a correction produces a new frozen revision, the disclosure check MUST be repeated for that revision before Reviewers rely on it again — a check performed against a superseded revision does not carry over.

A public project's own Canon may legitimately reference its own public commits, public dependencies, external specifications, or infrastructure deliberately intended to be public; this general rule does not forbid that.

A stronger rule applies specifically to the **AI-Codex's own public repository**: no external-project names, private paths, commit references, or infrastructure details that could re-identify an unrelated source project, because that repository's purpose is to publish a project-agnostic methodology derived from private precedents without exposing those precedents (the AI-Codex publication-anonymity rule). That stronger anonymity rule governs the AI-Codex's own publication and does not automatically bind an arbitrary project adopting this Codex, unless that project separately adopts it.

## 7. Retention, and the legacy public-history remediation

- **Private Working Repository:** working materials are retained as durable process history by default.
- **Public Canon Repository, prospectively:** under the architecture in §1, this repository never receives raw working material to begin with, so normal operation has no post-promotion workspace-removal step and does not require routine public Git-history rewriting.

**Legacy case:** a repository that already contains raw working material in public history from before this architecture was adopted has a legacy hygiene defect distinct from normal operation, and that defect MUST be remediated explicitly. The exact remediation mechanism is project-specific and MAY include sanitization, migration, history replacement, repository replacement, or another justified method; this Codex does not prescribe one universal history-rewrite procedure. Once remediated, the legacy rule no longer participates in that project's normal lifecycle — §1's prospective architecture governs going forward.

## 8. Promotion Record and atomic lifecycle self-reference normalization

The Promotion Record remains authoritative lifecycle evidence, but a promoted Canon package MUST NOT contradict it with stale Candidate-era self-reference.

This Codex distinguishes two immutable identities:

- the **Reviewed Frozen Identity** — the exact Candidate bytes that received the required gate-bearing `PASS` states;
- the **Canonical Materialization Identity** — the resulting Canon package after the same `PROMOTE` transaction performs the narrowly allowed lifecycle normalization below.

The Reviewed Frozen Identity MUST remain permanently traceable and MUST NOT be rewritten. The Canonical Materialization Identity MAY differ from it only through **Promotion Lifecycle Normalization** and its deterministic derived consequences.

### 8.1. Promotion Lifecycle Normalization

If reviewed authored material that will be materialized into Canon contains lifecycle/self-reference whose truth changes solely because the Product Owner has just issued a valid `PROMOTE`, the promotion transaction MUST normalize that material to the resulting Canonical state before the Canon materialization becomes current or visible.

This normalization is the one closed exception to the ordinary any-edit-requires-re-review rule. It does **not** create a new Frozen Revision and does **not** require a second product review, but only when every constraint below is satisfied.

Allowed changes are limited to lifecycle/self-reference mechanically implied by the same `PROMOTE`, including:

- `Candidate`, `Working Candidate`, `Proposed`, `Frozen Candidate`, or `NOT CANON` lifecycle labels becoming `Canonical` / `Promoted`;
- lifecycle-only title/self-description labels such as `Candidate entry point` becoming `Canonical Revision entry point`;
- `target Canonical Revision` becoming `current Canonical Revision` when the coordinate itself is unchanged;
- candidate-only conditionals such as “not Canon until required PASS + PROMOTE” becoming a Canonical statement and/or deterministic Promotion Record reference;
- deterministic lifecycle locators/self-references whose only new fact is that promotion completed.

The normalization MUST NOT change:

- normative product requirements or their semantic force;
- scope or non-goals;
- product decisions, rationale, rejected alternatives, business rules, security/privacy/authority rules, dependency semantics, or implementation requirements;
- non-lifecycle editorial content merely because cleanup would be convenient;
- any value whose change is not a deterministic consequence of this same promotion.

If any proposed change falls outside the allowlist, or if zero product-semantic diff cannot be mechanically established, the exception does not apply: the Candidate MUST be edited, frozen as a new Frozen Revision, and reviewed normally.

### 8.2. Derived representations and identities

When allowlisted lifecycle normalization changes authored Markdown that participates in FORMAT-2 or another deterministic derived representation, the promotion transaction MUST regenerate only the affected derived data, revalidate schema/source mapping/equivalence, and update the Canonical Materialization Identity accordingly.

A manifest or payload-identity artifact MAY likewise change only to record:

- the normalized authored-file identity;
- mechanically regenerated derived-file identities;
- the resulting Canonical Materialization Identity;
- linkage back to the unchanged Reviewed Frozen Identity.

Those derived changes inherit the no-re-review exception only to the extent they are deterministic consequences of the allowlisted lifecycle normalization.

### 8.3. Promotion Record obligations

A Promotion Record MUST identify unambiguously:

- the Reviewed Frozen Identity;
- the required Compiler `PASS` state(s), where applicable;
- the independent Reviewer `PASS` state(s) and the exact Reviewed Frozen Identity they reviewed;
- the Product Owner `PROMOTE` authorization;
- whether Promotion Lifecycle Normalization occurred;
- the resulting Canonical Materialization Identity;
- the mechanically verified transformation class / zero-product-semantic-diff evidence when the two identities differ;
- the resulting public release/Canonical Revision identity where applicable.

A public-safe identity MAY be a deterministic content manifest, tree/commit identity, or another immutable public identifier. Private Working provenance MAY retain private commit identifiers separately.

### 8.4. Atomicity and failure

`PROMOTE` and its required Promotion Lifecycle Normalization are one lifecycle transaction.

A project MUST NOT expose or declare the new Canonical Revision current while any required lifecycle self-reference normalization, derived-representation regeneration, or identity/provenance update remains incomplete.

If normalization or mechanical verification fails, promotion finalization stops. The prior Canon remains current; the Candidate remains Promotion-Ready unless the failure reveals product/content drift requiring a new review.

After successful finalization, the Canonical Materialization Identity is the immutable Canon package for that `RNNN`, subject only to the explicit legacy migration rule in `60-PROJECT-ADOPTION-AND-VERSIONING.md` §3.
## 9. Human and machine representations of Canon

Every v1.1.0 Canon component MUST retain human-readable authored Markdown as the authoritative representation.

The same reviewed product-semantic state MUST also have a generated machine representation in the JSONL/NDJSON family (FORMAT-2). After promotion normalization, FORMAT-2 MUST represent the Canonical Materialization's lifecycle self-reference while preserving the reviewed product semantics. FORMAT-2 MUST:

- be generated from authored Markdown, not independently maintained by hand;
- contain self-contained semantic records sufficient for ordinary machine consumption without mandatory Markdown fallback;
- carry stable record identity and source mapping;
- support selective retrieval before records enter an LLM context;
- be validated against its schema and source mapping at Candidate freeze;
- be mechanically re-verified at `PROMOTE`;
- be treated as defective if it diverges semantically from the authored Markdown.

Downstream consumers SHOULD read the representation appropriate to their task and MUST NOT be required to re-read and compare both representations on every pass. Expensive equivalence/integrity checks belong at lifecycle boundaries.
