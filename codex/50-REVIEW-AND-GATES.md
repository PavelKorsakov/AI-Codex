# 50 — Review and Gates

Release target: **AI-Codex v1.1.0**

Authoritative lifecycle status is external to this immutable payload and is resolved through the applicable Promotion Record.

This document governs what counts as sufficient independent challenge, and what opens or closes a gate. It applies both to compiled concepts before implementation and to implementation before integration.

## 1. What a Reviewer is asked to do

Before substantive review work, a Reviewer MUST receive an exact open-loop Review Assignment whose load-bearing fields conform to the versioned `REVIEW_ASSIGNMENT` envelope schema. The assignment MUST identify the exact Frozen Revision, authorized reading set (directly or by one authoritative referenced artifact), output location, Reviewer identity/Role, and applicable stop/handback conditions.

The Reviewer performs Assignment Preflight before reading the Candidate substantively. A material assignment-vs-Codex/Canon/authority/lifecycle conflict causes STOP and return of the exact conflict.

After bootstrap but before an exact Review Assignment exists, the Reviewer MUST NOT begin substantive project analysis, form findings, propose product fixes, inspect a Candidate speculatively, or infer review scope by repository archaeology. It MAY acknowledge readiness, report access/bootstrap failure, perform explicitly requested mechanical readiness checks, and state that it is awaiting assignment.

A Reviewer MUST NOT confirm that another participant did good work as its default posture. A Reviewer MUST attempt to falsify the candidate: read the relevant contract and context, examine the actual frozen material (not a description of it), check boundaries, look for missed scenarios and failure paths, and identify hidden assumptions — then issue an independent verdict.

## 2. PASS / BLOCK semantics

Review applies to one exact Frozen Revision. `PASS` or `BLOCK` is a verdict on that revision, not on the component in general. A later revision does not automatically inherit a prior `PASS`; **any** change to the reviewed content of a frozen revision — not a change judged "material" by the party who made it — MUST trigger review again on the new revision. This is the default, unconditional rule for an edited candidate. It is distinct from, and not narrowed by, the freshness-check materiality assessment in `10-DECISION-STORAGE-SYSTEM.md` §3, which applies only to a *delayed promotion* of an unedited, already-approved candidate. Review is not a vote: a single unresolved, valid `BLOCK` keeps the relevant gate closed regardless of how many other participants issued `PASS` (`00-CODEX-SCOPE-AND-TERMS.md` §4), and there is no preset maximum number of correction/review cycles.

## 3. Reviewer topology scales with reviewer count

The number of blocking Reviewers on a workstream is a configuration choice, not a fixed constant (`40-ROLES-AND-AUTHORITY.md` §2). The protocol below MUST scale to however many are assigned, and every assignment MUST satisfy the Actor-level independence floor in `40-ROLES-AND-AUTHORITY.md` §5.

### N = 1

There is no reviewer-to-reviewer cross-examination to perform, and no Stage 1/Stage 2 split exists for this case. The single Reviewer's one verdict is gate-bearing directly. The workflow is ordinary adversarial ping-pong:

```text
Producer (Compiler, for a concept candidate; Executor, for an implementation
candidate) produces frozen candidate
  → Reviewer performs adversarial review, issues PASS or BLOCK
  → findings return to the producer and Product Owner
  → candidate is corrected, or a disputed BLOCKING finding is disproved,
    sent to an Adjudicator (§5), or resolved via post-adjudication verdict (§5)
  → new frozen revision reviewed again, if corrected
  → repeat until the terminal condition below is met on the same frozen revision
```

`PASS`/`BLOCK` is a Reviewer or Compiler verdict (`00-CODEX-SCOPE-AND-TERMS.md` §4); this Codex does not define an Executor `PASS` signal. The terminal condition above is therefore not identical for the two candidate types:

- **Concept candidate:** Compiler `PASS` + the Reviewer's final `PASS`, on the same frozen revision.
- **Implementation candidate:** the implementation has reached its defined review-ready/acceptance state (`30-IMPLEMENTATION-METHODOLOGY.md` §3) + the Reviewer's final `PASS`, on the exact revision required for integration. No separate formal Executor `PASS` exists or is required; the Executor does not issue a gate-bearing verdict of its own.

### N > 1

Every assigned Reviewer follows a two-stage process on the same exact frozen candidate. **Stage 1 is provisional. Stage 2 is gate-bearing.** A Stage 1 verdict, PASS or BLOCK, MUST NOT be treated as satisfying the promotion or integration gate under any circumstance, even if every Reviewer's Stage 1 verdict happens to be PASS. The same concept-vs-implementation terminal-condition distinction from N=1 applies here: every assigned Reviewer's Stage 2 `PASS` is gate-bearing in both cases, and Compiler `PASS` is additionally required only where the relevant gate actually defines a Compiler `PASS` (i.e. concept/candidate review, `10-DECISION-STORAGE-SYSTEM.md` §3) — implementation review does not acquire a Compiler-PASS or Executor-PASS requirement it does not otherwise have.

**Stage 1 — independent pass (provisional).** Every Reviewer MUST receive the same frozen candidate and the same authorized source context. No Reviewer receives any other Reviewer's verdict, findings, or reasoning before completing and freezing its own first review. Each Stage 1 artifact MUST be committed and preserved separately from every other artifact in this cycle, including the Stage 2 artifact that later supersedes it for gate purposes — the Stage 1 record remains auditable even though it is not gate-bearing.

**Stage 2 — cross-examination pass (gate-bearing).** Only after every Stage 1 review is frozen, each Reviewer receives every other Reviewer's review. Each Reviewer MUST: challenge the material findings and coverage claims of the others; perform a genuine spot-audit of a self-chosen sample of the others' evidence/reproduction/coverage claims, deep enough to be meaningful (no fixed percentage is mandated); reconsider its own conclusions in light of what the others found; and produce an updated, final result. This Stage 2 result — not the Stage 1 result — is that Reviewer's `PASS`/`BLOCK` for every gate defined in this Codex.

Only after every Reviewer's Stage 2 result exists MUST the results be returned to the Compiler and Product Owner for correction and synthesis; a Stage 1 result MUST NOT be returned or acted on as if it were final while any Reviewer's Stage 2 pass is still outstanding. If correction produces a new frozen revision, the full review cycle (both stages, for every Reviewer) MUST repeat on that revision.

For exactly two Reviewers, this reduces to Review 1 / Review 2 (Stage 1, provisional) followed by Review 1.1 / Review 2.1 (Stage 2, gate-bearing) — one concrete instance of the general rule above, not a separate protocol.

### Independence guarantee

A Reviewer topology that withholds a candidate from one Reviewer until other Reviewers have already reached agreement does not satisfy Stage 1's independence requirement and is not a valid instance of this protocol. Every blocking Reviewer on a workstream receives the frozen candidate at the same point and reviews it without access to any other Reviewer's conclusions until Stage 1 is complete for all of them.

## 4. Gate rule

Promotion eligibility requires the final, gate-bearing `PASS` (§3) from the Compiler and from every blocking Reviewer assigned to the workstream, on the same frozen revision. There is no majority-voting fallback. In a Same-Problem Multi-Compiler workstream, "PASS from the Compiler" means unanimous `PASS` from every participating Compiler on that revision (the Same-Problem Multi-Compiler unanimity rule; `40-ROLES-AND-AUTHORITY.md` §3) — Compiler-to-Compiler agreement is a separate, prior gate and does not itself satisfy the independent-Reviewer requirement below.

This concept/candidate review gate is distinct from the implementation review gate required before integration (`30-IMPLEMENTATION-METHODOLOGY.md` §7); the two are not required to share the same reviewer count or topology, though both follow the N=1/N>1 mechanics in §3.

## 5. Finding severity, disproof, and the Adjudicator (the finding-severity and adjudication rule)

Every finding a Reviewer records carries one of two severities, defined in `00-CODEX-SCOPE-AND-TERMS.md` §4: `BLOCKING` or `NON-BLOCKING`. A Reviewer's verdict MUST be `BLOCK` whenever at least one unresolved `BLOCKING` finding exists on the reviewed revision, and MAY be `PASS` while any number of `NON-BLOCKING` findings remain open.

Every finding, of either severity, MUST include both a `CLAIM` and its supporting `EVIDENCE / REPRODUCTION`. A finding with an empty or missing evidence field is not a valid finding and does not by itself justify or excuse a verdict either way — an empty FINDINGS list is not evidence of thoroughness. During Stage 2 cross-examination (§3), the spot-audit obligation exists specifically to make an empty or boilerplate first pass visible to another participant rather than self-certifying.

**Disproof:** the Compiler MAY challenge any finding with counter-evidence. If the Reviewer accepts that counter-evidence, the Reviewer MUST withdraw the finding or downgrade it to `NON-BLOCKING`, whichever is appropriate — the Reviewer does not get to keep a disproved finding at its original severity out of inertia.

**Adjudication:** if the Reviewer and Compiler remain in material disagreement over a `BLOCKING` finding after disproof has been attempted, the dispute MAY be escalated to an independent Adjudicator (`00-CODEX-SCOPE-AND-TERMS.md` §4, `40-ROLES-AND-AUTHORITY.md` §2). The Adjudicator examines only the disputed finding, its evidence, the Compiler's counter-evidence, and the governing Canon/Codex rules needed to decide that narrow dispute — it does not perform a replacement full review of the Candidate and does not itself issue Candidate `PASS`/`BLOCK`. The Adjudicator's decision is one of `BLOCKING FINDING VALID`, `FINDING DISPROVED`, or `FINDING NON-BLOCKING`, and resolves that finding for the current frozen Candidate revision only. Product Owner `PROMOTE` authority remains separate from adjudication and MUST NOT be used to directly override a valid unresolved `BLOCK` instead of going through this process.

**Post-adjudication verdict:** an Adjudicator's decision does not by itself update the gate-bearing state — the Reviewer MUST act on it. After the Adjudicator resolves a disputed finding, the Reviewer MUST issue a new, separately preserved post-adjudication verdict artifact for the same frozen Candidate revision, treating the Adjudicator's decision as final for that specific finding. If no unresolved `BLOCKING` finding remains after applying the Adjudicator's decision, the Reviewer MUST issue `PASS`. If another unresolved `BLOCKING` finding remains (one the Adjudicator was not asked to resolve), the verdict remains `BLOCK`. This new post-adjudication artifact — not the earlier, now-superseded verdict — becomes that Reviewer's current gate-bearing verdict for the same frozen revision. Prior review artifacts, including the pre-adjudication verdict, remain preserved and MUST NOT be overwritten (§7). None of this makes the Adjudicator a Reviewer, and none of it permits Product Owner override in place of this procedure.

**OPEN/TBD:** whether the Stage 2 reciprocal spot-audit must additionally be recorded in a structured Review Coverage Ledger (explicit `CHECK / TARGET / METHOD / EVIDENCE / RESULT` entries per review dimension) as mandatory Dogma, or whether the form of that spot-audit is left to Reviewer discretion (Interpretation) as long as the substantive obligation in §3 is met. This is not yet decided.

## 6. Authorized review context and disclosure verification

A Reviewer's authorized reading set for a given review round — which candidate files, which Product Owner decisions, and which prior artifacts (if any) it may or must read — MUST be stated explicitly for that round by whoever assigns the review (`40-ROLES-AND-AUTHORITY.md` §2), not assumed from general project familiarity. A zero-memory Reviewer MUST be able to determine what it is permitted to read without guessing. Where a public-disclosure check is required, the Compiler's disclosure evidence exists before the Reviewer's final verdict (`10-DECISION-STORAGE-SYSTEM.md` §6); verifying that evidence is part of the Reviewer's ordinary review coverage for that round, not a separate later pass.

## 7. Review artifacts and versioning

A Reviewer MUST NOT overwrite a previous round's review artifact, including a Stage 1 artifact once Stage 2 exists. Each review round MUST produce its own new, separately committed artifact, so the history of independent review followed by reconciliation remains auditable.
