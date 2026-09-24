# 00 — Codex Scope and Terms

Release target: **AI-Codex v1.1.0**

Authoritative lifecycle status is external to this immutable payload and is resolved through the applicable Promotion Record.

## 1. What the AI-Codex is

The AI-Codex is a versionable, agent-agnostic standard for how a human Product Owner and one or more AI participants collaboratively produce, review, and implement decisions in a software project.

It defines:

1. how a decision is honestly produced (Decision Methodology);
2. where and how a decision physically lives through its lifecycle (Decision Storage System);
3. how an approved decision becomes working software (Implementation Methodology);
4. who may do what (Roles and Authority);
5. what constitutes sufficient independent challenge before a decision is trusted (Review and Gates);
6. how a concrete project binds itself to a specific version of this standard (Project Adoption and Versioning).

## 2. What the AI-Codex is not

- It is not a specific project's Canon. A project's own accepted decisions live in that project's `Canon/`, governed by this Codex but not part of it.
- It is not written around specific actors. It defines role obligations, not model identity or reasoning style.
- It does not encode project-specific business knowledge.
- It does not force different AI systems to reason identically.
- It does not replace Product Owner authority.
- It does not turn review into a vote.
- It does not prescribe a specific technology where the process itself does not require one.
- It does not require paperwork whose only purpose is to exist.
- It does not pretend uncertainty has been eliminated.
- It does not make an Executor responsible for repairing a conceptual defect it discovers in an already-approved Canon; the Executor stops and returns the problem to Decision Methodology.

## 3. Normative language

Within this Codex and any document promoted under it:

- **MUST** — a hard requirement. Its absence is a defect.
- **SHOULD** — a strong default. Deviation requires a stated reason.
- **MAY** — permitted, not required.

A declarative sentence in a Canon document that uses none of these tags is descriptive or explanatory, not binding. Any requirement a participant must actually comply with MUST carry an explicit MUST or SHOULD tag; an untagged sentence describing a safeguard or process step is not by itself enforceable and MUST NOT be relied on as a gate condition. Pure explanatory or illustrative prose MAY remain untagged.

**Every normative MUST in this Codex is Dogma unless this Codex explicitly states a narrower, non-compliance-bearing scope for that specific obligation.** This is the single rule resolving the relationship between "MUST," "hard requirement," "Dogma," and "compliance": they are not four separate concepts with independently negotiable scope, they are one concept named four ways depending on context (§4, `60-PROJECT-ADOPTION-AND-VERSIONING.md` §4). A SHOULD or MAY is never Dogma.

## 4. Core terms

**Product Owner** — the human with product authority over a project: accepts product decisions, authorizes promotion into Canon. Product Owner authority does not itself count as a Reviewer `PASS`; the Product Owner does not act as a tie-breaker inside a review gate; the Product Owner cannot override a valid unresolved Reviewer `BLOCK` by fiat. The Product Owner holds immediate-control authority — may STOP, CANCEL, PAUSE, or REOPEN affected work at once, and may initiate a new Candidate proposing to change or supersede Canon — but this is a distinct authority type from the power to silently rewrite Canon or waive Dogma, which requires the ordinary Candidate → Review → PROMOTE lifecycle (typed normative precedence, below). Whether the same human may, in some workstream, separately occupy a Reviewer role is part of the still-open broader Role Combination matrix (`40-ROLES-AND-AUTHORITY.md` §5) and is not settled by this definition either way — the minimum Actor-level independence floor that does apply to everyone, Product Owner included where relevant, is defined under Reviewer, below.

**Compiler** — the role that produces a candidate decision or implementation plan by researching, deliberating (with the Product Owner and, where applicable, another Compiler), and freezing a revision for review.

**Reviewer** — the role that independently attempts to falsify a frozen candidate rather than confirm it (`50-REVIEW-AND-GATES.md` §1). A workstream may assign one or more blocking Reviewers; see `50-REVIEW-AND-GATES.md` for how the count changes the protocol, and `40-ROLES-AND-AUTHORITY.md` §2 for who may be assigned. **Actor-level independence is candidate-scoped, not permanent:** for a specific Candidate, an Actor that participated in compiling/producing that Candidate MUST NOT supply a blocking Reviewer `PASS` for that same Candidate, and an Actor that executed an implementation Candidate MUST NOT supply a blocking Reviewer `PASS` for that same implementation Candidate. Starting a new session, chat, or memory state does not create independence — independence concerns participation in producing the reviewed artifact, not conversational state. The same Actor MAY hold different Roles on other Candidates or other workstreams, subject to any broader Role Combination rule later adopted.

**Executor** — the role that converts an approved Canon component into working software under an explicit `TASK` contract.

**Adjudicator** — a role, distinct from Compiler and Reviewer, that resolves a specific disputed `BLOCKING` finding when a Reviewer and Compiler remain in material disagreement after the Compiler has offered counter-evidence (`50-REVIEW-AND-GATES.md` §5). The Adjudicator is appointed by the Product Owner or the applicable authority model, MUST be independent of both producing the Candidate and authoring the disputed finding, examines only that narrow dispute and its evidence, does not perform a replacement full review, and does not issue Candidate `PASS`/`BLOCK`.

**Role vs. Actor** — a Role's obligations and authority are fixed by this Codex regardless of which Actor (a specific model, human, or tool) occupies it in a given workstream. Actor identity is a workstream fact, not a vendor/model-family inference: separately assigned participants from the same vendor or model family MAY be distinct Actors, while opening a new chat/session for the same participant does not create a new independent Actor. Candidate-scoped independence is determined by participation in producing the reviewed artifact, not by branding or context-window identity.

**Candidate** — a proposed decision, document, or implementation not yet approved.

**Frozen Revision** — a Candidate at an exact, unchanging state (e.g. an exact commit) submitted for review. A Candidate that is edited after being frozen for review becomes a new Frozen Revision requiring its own review; a prior `PASS` does not automatically carry over.

**Stage 1 / Stage 2 (N>1 review only)** — for a workstream with more than one blocking Reviewer, Stage 1 is each Reviewer's independent first-pass verdict, produced without access to any other Reviewer's findings. Stage 2 is each Reviewer's verdict after cross-examining the others. **A Stage 1 verdict is provisional and is never gate-bearing.** Only a Stage 2 (final, post-cross-examination) verdict counts as that Reviewer's `PASS` or `BLOCK` for any gate purpose (`50-REVIEW-AND-GATES.md` §3, §4). For a single-Reviewer (N=1) workstream, there is no Stage 1/Stage 2 split; the Reviewer's one verdict is gate-bearing directly.

**PASS / BLOCK** — a Reviewer's or Compiler's verdict on an exact Frozen Revision, always meaning the participant's final gate-bearing verdict (see Stage 1/Stage 2, above, for what "final" means under N>1). `BLOCK` keeps the relevant gate closed regardless of how many other participants issued `PASS`; there is no majority vote. A Reviewer's verdict is `BLOCK` whenever at least one unresolved `BLOCKING`-severity finding exists (§ BLOCKING/NON-BLOCKING, below); it is otherwise eligible for `PASS`. The only paths out of a `BLOCK` are: correction that resolves the finding and a subsequent re-review; the Reviewer itself withdrawing or downgrading the finding after accepting the Compiler's counter-evidence; or a Adjudicator resolving a specifically disputed finding (`50-REVIEW-AND-GATES.md` §5). None of these is the Product Owner overriding a valid `BLOCK` by fiat.

**BLOCKING / NON-BLOCKING** — the two normative severities a review finding may have. A finding is `BLOCKING` when the demonstrated defect violates Codex Dogma or an explicit Product Owner decision; creates a contradiction letting a mandatory process produce materially different valid interpretations; permits bypass of a mandatory gate or independence safeguard; or leaves a load-bearing rule undefined such that the required process cannot execute deterministically. A finding is `NON-BLOCKING` when it concerns clarity, maintainability, editorial quality, robustness, or another improvement that does not make the Candidate non-compliant, non-deterministic, or unable to satisfy its mandatory gate. A Candidate MAY receive `PASS` while `NON-BLOCKING` findings remain open.

**Gate** — a point in the lifecycle that a Candidate may not pass without a defined condition being satisfied (e.g. the review gate before promotion, the integration gate before merging to a project's main line, the release/publication gate before publishing).

**Workspace** — a Compiler's or Reviewer's own working area (e.g. `Compiler-A/`, `Reviewer-A/`, `Reviewer-B/`) containing raw process material: drafts, intermediate hypotheses, abandoned branches, cross-model exchange. Workspace material is process-owned, not project-owned, and is not itself Canon.

**Project Canon** — the promoted, project-owned body of accepted decisions for a concrete project, governed by this Codex. A project's declared Canon namespace contains promoted material only. Every versioned Canon component MUST expose a stable human-readable `index.md` and immutable Canonical Revisions (`R001`, `R002`, ...) as defined by `10-DECISION-STORAGE-SYSTEM.md`.

**Canonical Revision (`RNNN`)** — the sequential accepted Canon state of one component/configuration item. `R001` is the first promoted revision, followed by `R002`, `R003`, and so on. It is not a product version, SemVer, Git revision, Frozen Revision, or review-round number.

**FORMAT-2** — the generated machine-oriented JSONL/NDJSON representation of Canon. Markdown remains the authored authoritative representation; FORMAT-2 MUST be generated from it, self-contained enough for ordinary machine consumption without mandatory fallback to Markdown, selectively retrievable, and mechanically validated at lifecycle boundaries.

**Closed interpretation loop / Open interpretation loop** — in a closed loop, ambiguity can be corrected immediately in the same live exchange. In an open loop, an instruction leaves one context and may be acted on before the sender can correct a misunderstanding. Open-loop load-bearing fields therefore require a closed, versioned, deterministic field contract.

**Open-loop operational envelope** — the strict line-oriented `FIELD: value` header used for load-bearing fields of TASK, Review Assignment, and Service Memo. Each artifact `TYPE` has a versioned schema defining required/optional, scalar/repeatable, allowed-value, duplicate, and extension behavior. Unless a TYPE explicitly says otherwise, repeating a scalar field makes the envelope invalid; repeatable field values are appended in encounter order. Complex nested data belongs in referenced attachments.

**Assignment Preflight** — the Compiler/Reviewer check performed before substantive work under an open-loop assignment: validate the assignment against the adopted Codex, project configuration, current Canon, Role authority, lifecycle/storage boundaries, and exact assignment contract; on material conflict, STOP and return the conflict.

**Execution Contract Validation** — the Executor's TASK-intake check: valid TASK → accept and execute; invalid/conflicting TASK → STOP and return the exact defect.

**Role Context Profile** — an optional, non-authoritative posture layer. It MAY influence style, attention priorities, or conversational defaults but MUST NOT change Dogma, authority, gates, independence, Canon, or assignment semantics. v1.1.0 ships reference profiles for Compiler (`Researcher`) and Reviewer (`Skeptic`).

**Bootstrap receipt** — the minimal deterministic acknowledgment emitted after required onboarding parameters are resolved. It identifies the resolved Role, applicable workstream/repository scope, governing Codex/project state, and next required action/state. Successful onboarding is acknowledged rather than unsolicitedly summarized; an unresolved actionable onboarding conflict is reported and stops work.

**Claim / Evidence** — a Claim is an assertion (e.g. "tests pass," "this alternative was considered and rejected"). It becomes a Fact for the purposes of review only once accompanied by Evidence sufficient for another qualified participant to reproduce or independently verify it. A Claim without Evidence remains a Claim.

**TASK** — an open-loop contract handed to an Executor: baseline, scope, allowed/forbidden changes, acceptance criteria, tests/checks, stop conditions, handback requirements. Its load-bearing fields MUST use the versioned open-loop operational envelope defined by this Codex.

**Decision lifecycle state** — a decision-in-progress is at all times in exactly one of: **Open** (no answer yet), **Proposed** (a candidate answer under consideration), **Promotion-Ready** (all required final-stage `PASS` states exist on one frozen revision, but the Product Owner has not yet authorized promotion), or **Canonical** (promoted, in force in Project Canon). A Promotion-Ready candidate is review-approved but not yet in force; only **Canonical** decisions are currently binding. These states MUST NOT be collapsed. This authoritative state is tracked in the Promotion Record (`10-DECISION-STORAGE-SYSTEM.md` §8), not by editing the reviewed payload's own frozen self-description.

**Promotion Record** — a repository-level artifact, external to and separate from the immutable reviewed payload it describes, that tracks a Candidate's authoritative lifecycle status (Candidate → Compiler-gate-passed → Reviewer `PASS`/`BLOCK` → Promotion-Ready → PROMOTED/Canonical) without requiring any edit to the payload itself. See `10-DECISION-STORAGE-SYSTEM.md` §8 for its required content and format latitude.

**Promotion / Promotion-Ready** — the act of moving a Promotion-Ready package into Project Canon, making it Canonical. Only the Product Owner's separate authorization performs this act (`40-ROLES-AND-AUTHORITY.md` §3, `10-DECISION-STORAGE-SYSTEM.md`). This act is recorded in the Promotion Record; it MUST NOT require editing the promoted payload's own already-reviewed content.

**Supersede** — a later canonical decision may replace an earlier one; the earlier decision's history remains traceable rather than silently erased.

**Dogma / Interpretation** — Dogma is every mandatory (MUST-level) requirement of this Codex (§3). Interpretation is anything a participant additionally does or preserves beyond a MUST or SHOULD, because it judges it useful for the specific component. Dogma is a floor, not a ceiling.

**AI-Codex compliant / AI-Codex-derived (modified)** — a project claiming **AI-Codex compliant** status MUST satisfy all Dogma of its pinned Codex version without exception. A project that deliberately departs from any Dogma requirement MUST NOT claim full compliance; it MUST identify itself as **AI-Codex-derived** or **AI-Codex-modified** (or an equivalent explicit label) and record every such deviation. See `60-PROJECT-ADOPTION-AND-VERSIONING.md` §4.

**Implementation Review** — the mandatory independent review gate an implementation candidate MUST pass before integration, distinct from and not necessarily identical in reviewer count or depth to the concept/candidate review gate that precedes Canon promotion. See `30-IMPLEMENTATION-METHODOLOGY.md` §7.

**Working Repository / Canon Repository** — for a public project, the physically separate private repository holding all raw process material, and the physically separate public repository holding only promoted Canon and intentionally public material; this separation is mandatory Dogma for any project with a public Canon (`10-DECISION-STORAGE-SYSTEM.md` §1). For a fully private project, these may coexist in one repository provided the logical distinction is maintained.

**Typed normative precedence** — when two governing layers of a project conflict, this Codex does not resolve the conflict with a single linear "highest authority wins" ladder or a "latest instruction wins" rule. Instead, each layer holds a distinct *type* of authority, and which layer governs depends on the type of conflict, not a fixed rank. The five layers, their authority type, and the deterministic resolution per conflict type are defined in `60-PROJECT-ADOPTION-AND-VERSIONING.md` §5 (the typed-precedence rule):

- **AI-Codex Dogma** — process authority (how a compliant project decides, reviews, stores, promotes, implements, integrates);
- **Project Adoption Declaration** — configuration authority only (Interpretation-level and Dogma-satisfying project mechanisms);
- **Project Canon** — current project truth for downstream work;
- **Product Owner** (direct instruction) — immediate control authority (may STOP/CANCEL/PAUSE/REOPEN/initiate a new Candidate at once), not silent rewrite authority over Canon or Dogma;
- **TASK** — subordinate execution contract.

No layer may silently override a layer whose authority type it does not hold, regardless of how "senior" the issuing party is; a Product Owner's immediate-control authority in particular is real and instantaneous but is not the same authority type as the power to rewrite Canon or waive Dogma or a valid Reviewer `BLOCK`, which requires the Candidate → Review → PROMOTE lifecycle (`10-DECISION-STORAGE-SYSTEM.md` §3) like any other Canon change.
