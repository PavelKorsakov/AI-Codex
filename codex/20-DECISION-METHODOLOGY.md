# 20 — Decision Methodology

Release target: **AI-Codex v1.1.0**

Authoritative lifecycle status is external to this immutable payload and is resolved through the applicable Promotion Record.

This document answers how a decision must be born to be intellectually honest, independent of where it is stored (`10-DECISION-STORAGE-SYSTEM.md`) or how it is challenged (`50-REVIEW-AND-GATES.md`).

## 1. Epistemic discipline

1. Fact before assumption. Do not invent a missing fact.
2. Unknown remains unknown. If something has not been established, it MUST be labeled unknown rather than assumed.
3. Do not repair a hypothesis. Establish or reproduce the defect before proposing a fix.
4. Begin from the actual observed state (code, repository, runtime, logs, artifacts), not from memory of what it probably is.
5. Prefer one narrow, discriminating experiment over several simultaneous broad changes.
6. Do not re-test an already-excluded layer without a new reason to doubt it.
7. A report is a Claim; only Evidence sufficient for independent reproduction or verification turns it into a Fact (see `00-CODEX-SCOPE-AND-TERMS.md`).
8. A successful process does not by itself imply a correct result.
9. Raw evidence MUST NOT be silently corrected. Correction or normalization is a distinct, traceable step.
10. Provenance is part of the result: an important artifact should be able to answer where it came from.

## 2. Product before implementation

11. A product decision precedes its technical realization; implementation detail MUST NOT be smuggled in as if it were the product requirement.
12. An implementation example offered during product discussion is illustrative, not a requirement, unless explicitly adopted as one.
13. Research precedes design wherever material facts are not yet known.
14. A confirmed requirement MUST NOT be silently weakened for implementation convenience.
15. MVP scope and future capability MUST be kept explicitly separate; unimplemented future behavior MUST NOT be presented as current behavior.
16. Canonical state and derived/representational forms MUST be distinguished; a convenient derived format MUST NOT quietly become the canonical source of truth.

## 3. Claim taxonomy

Every material assertion in a decision or its review belongs to one of the following classes, and the obligation attached to it follows from the class:

- **FACT / OBSERVATION** — requires verifiable Evidence where reasonably obtainable.
- **DERIVED CONCLUSION** — requires an inspectable reasoning chain grounded in Evidence, not just a stated conclusion.
- **PRODUCT DECISION** — requires explicit rationale and clear ownership (who decided), not a false claim of empirical proof.
- **CONVENTION / DEFINITION** — becomes authoritative by explicit adoption, not by evidence.
- **UNKNOWN** — MUST remain explicitly marked `UNKNOWN` until resolved; it MUST NOT be silently converted into any of the above.

This taxonomy exists so that an evidence requirement does not become blanket bureaucracy: the amount and kind of justification required scales with the claim's class, not with a uniform paperwork rule.

## 4. States of a decision in progress

A decision is at all times in exactly one of four states (`00-CODEX-SCOPE-AND-TERMS.md`, Decision lifecycle state), and participants MUST NOT conflate them — in particular, review approval MUST NOT be treated as equivalent to being in force:

- **Open** — no answer yet exists.
- **Proposed** — a candidate answer under active consideration, not yet approved.
- **Promotion-Ready** — all required review `PASS` states exist on one frozen revision, but the Product Owner has not yet authorized promotion; the decision is review-approved but not yet in force.
- **Canonical** — promoted into Project Canon by Product Owner authorization; currently in force.

A Canonical decision MUST NOT be reopened merely because a participant prefers a different option. Reopening requires a new fact, a contradiction, a demonstrated impossibility, or a genuine change in requirements. A Promotion-Ready decision that has not yet been promoted is not yet binding and remains subject to the freshness check in `10-DECISION-STORAGE-SYSTEM.md` §3 before a delayed promotion.

**A direct Product Owner instruction that conflicts with current Canon does not silently become the new Canon.** The Product Owner's immediate-control authority (`00-CODEX-SCOPE-AND-TERMS.md` §4, Product Owner) lets them stop, cancel, pause, or reopen the affected work at once, but changing what Canon actually says requires initiating a new Candidate that proposes the change and carrying it through this same Open → Proposed → Promotion-Ready → Canonical lifecycle (typed normative precedence, `60-PROJECT-ADOPTION-AND-VERSIONING.md` §5). An instruction that is purely operational (pausing, sequencing, reprioritizing work) and does not assert a different Canon content is not a conflict under this rule and does not require reopening anything.

## 5. Serial product design and the Service Memo

Where a Product Owner is working through a sequence of product questions with a Compiler, the Compiler SHOULD surface one material question at a time, with a small number of understandable alternatives (e.g. A/B/C) and a short explanation of each, rather than several open architectural questions in the same message. A Compiler's own recommendation MAY be offered, but the Product Owner decides.

### 5.1. Closed-loop vs. open-loop communication

Natural-language Markdown/prose is appropriate for a **closed interpretation loop**, where ambiguity can be corrected immediately before downstream action.

For an **open interpretation loop** — especially TASK, Review Assignment, or Service Memo transported into an isolated context — load-bearing routing/execution data MUST use the versioned open-loop `FIELD: value` envelope defined by the applicable artifact schema. Free prose MAY accompany the envelope but MUST NOT replace mandatory load-bearing fields.

### 5.2. Service Memo minimum contract

After a substantial round of AI-to-AI working exchange, the responsible participant MUST produce a **Service Memo** for transport to the next context.

A transported Service Memo MUST contain explicit load-bearing fields for at least sender (`FROM`), recipient (`TO`), and purpose (`PURPOSE`). It MUST additionally provide a short operational summary, exact artifact/file pointers with an explanation of what each is for, current state/next action where applicable, and exact revision/branch/output/stop information where those are load-bearing.

For the shipped v1.1.0 envelope, the summary is carried by `SUMMARY`. Where relevant repository/file artifacts exist, their artifact map is carried by repeatable `ARTIFACT: <path> | <purpose>` fields. `READ` or `ATTACHMENT` fields MAY supplement that map but MUST NOT substitute for the required path-plus-purpose explanation. Structural registry validity alone does not prove that these semantic memo obligations were satisfied.

One Service Memo MUST be emitted as one independently transportable copy unit. If the interface offers a distinct block with its own Copy control, the memo SHOULD use it; otherwise a dedicated standalone message or nearest equivalent clear block is sufficient. Surrounding commentary MUST remain outside the copyable memo unit.

The Service Memo is not itself Canon and does not substitute for the underlying Candidate, TASK, review artifact, Product Owner decision, or Promotion Record it references.

## 6. Multi-Compiler mode

Single-Compiler workflow is the default. Multi-Compiler mode is supported but exceptional, and MUST be deliberately enabled by the Product Owner rather than assumed. Two subtypes exist and MUST NOT be collapsed into one workflow, because their synchronization and conflict-resolution needs differ:

- **Same-Problem Multi-Compiler** — two or more Compilers independently work the same problem, to expose different decompositions and assumptions and to produce a later synthesis candidate.
- **Parallel-Domains Multi-Compiler** — two or more Compilers work different, explicitly partitioned domains of the same larger project, to increase throughput; their promoted results are later integrated into the same project.

In Same-Problem mode, each Compiler works in its own Workspace during independent compilation; a Compiler MAY read another Compiler's workspace material but MUST NOT directly rewrite it. Cross-reading and synthesis follow only after each Compiler has produced its own independent package.

A synthesis candidate produced by Same-Problem Multi-Compiler mode is not Compiler-side approved by one Compiler's judgment alone. It requires **`PASS` from every participating Compiler on the same frozen synthesis revision** before the Compiler side of the gate is satisfied (see `10-DECISION-STORAGE-SYSTEM.md` §3 and `50-REVIEW-AND-GATES.md` §2). This preserves the purpose of using multiple Compilers: one Compiler's unresolved objection cannot be silently erased by whichever Compiler happens to assemble the synthesis document. This unanimous-Compiler-PASS step is separate from, and does not substitute for, independent Reviewer approval by an actor who did not participate in compiling the candidate.
