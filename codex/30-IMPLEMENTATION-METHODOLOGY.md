# 30 — Implementation Methodology

Release target: **AI-Codex v1.1.0**

Authoritative lifecycle status is external to this immutable payload and is resolved through the applicable Promotion Record.

This document answers how an approved Project Canon component becomes working software without distortion. It governs the Executor role and its interaction with Compiler, Reviewer, and Product Owner.

## 1. Baseline

An Executor MUST know its exact starting point — a commit/tree/revision — rather than an informal reference such as "current main." Before substantial work, the Executor SHOULD inspect the real repository state: branch, HEAD, upstream, clean/dirty status, remote state, and ancestry where relevant. Unexpected drift from the expected baseline is a stop condition (§4), not an invitation to informally reconcile it while already mid-task.

## 2. TASK as a contract

Work is delegated to an Executor as a `TASK`, not an open-ended request. Because a TASK is an open-loop execution contract, its load-bearing fields MUST use the versioned strict `FIELD: value` envelope defined by the TASK schema. An optional Markdown body MAY explain rationale or context, but the Executor MUST be able to recover the execution contract without natural-language interpretation of that body.

A valid TASK states at least:

- baseline;
- scope;
- allowed changes;
- forbidden changes, where relevant;
- acceptance criteria;
- required tests/checks;
- stop conditions;
- handback requirements.

In the shipped v1.1.0 TASK envelope, the minimum semantic categories above map to required fields `BASELINE`, `SCOPE`, `ALLOWED`, `ACCEPTANCE`, `TEST`, `STOP_CONDITION`, and `HANDBACK`; `FORBIDDEN` remains optional because it applies only where relevant. A category that genuinely has no applicable item MUST be stated explicitly rather than silently omitted.

The TASK `TYPE` schema MUST define required/optional fields, scalar vs. repeatable fields, allowed values/types, duplicate behavior, and extension behavior. A repeated scalar field makes the envelope invalid unless that TYPE explicitly specifies another behavior; repeatable fields preserve values in encounter order. Complex nested data SHOULD be placed in a referenced attachment rather than encoded through ad-hoc indentation or improvised nested envelope syntax.

Structural schema validity is necessary but not sufficient for a valid TASK. Execution Contract Validation MUST also verify the semantic content obligations above and the applicable Canon/Codex constraints.

### 2.1. Execution Contract Validation

At TASK intake, the Executor performs Execution Contract Validation rather than an interactive product-design preflight:

```text
TASK valid
→ ACCEPT
→ execute

TASK invalid / conflicting
→ STOP
→ return the exact defect
```

An Executor MUST NOT negotiate an ambiguous TASK into a new product decision. Missing or conflicting load-bearing fields are a stop condition to be returned to the assigning authority.

A TASK SHOULD be small enough to review as a coherent unit and reproduce its tests, but large enough to leave a meaningful working state on completion. One TASK MUST NOT silently become several unrelated tasks; opportunistic cleanup outside the stated scope is not permitted even when the surrounding code invites it. Structural refactoring SHOULD freeze external behavior unless a behavior change is explicitly in scope; a legacy oddity MUST be understood before it is removed, not assumed to be dead weight.

## 3. Acceptance is not a single flag

"Works" is not one boolean. Distinguish at least: implementation complete, checks pass, review-ready, correction required, independently approved, integration-ready, integrated, released. A successful process (e.g. green tests) does not by itself establish a correct result (`20-DECISION-METHODOLOGY.md` §1.8) — tests can be green because a fixture already contains the needed state, a mock hides a real cold path, or only the happy path executed. Where the environment is part of the behavior being verified, acceptance SHOULD exercise the real path (real browser, real PTY, real filesystem, cold start, or equivalent) rather than relying on synthetic substitutes alone.

## 4. Stop conditions

A TASK MUST state the conditions under which the Executor does not continue, for example: baseline mismatch, a file appearing outside declared scope, a test revealing an unknown contract, production differing from the expected state, or a need for a product decision the Executor is not authorized to make. Stopping and preserving evidence at that point is success of the process, not failure of the Executor — a stop condition exists specifically to protect the TASK contract from being quietly worked around.

**A TASK that conflicts with current Project Canon is always a stop condition, without exception.** A TASK is subordinate to AI-Codex Dogma, permitted Project Adoption configuration, and current Project Canon (typed normative precedence, `60-PROJECT-ADOPTION-AND-VERSIONING.md` §5); it MUST NOT redefine product truth or weaken a mandatory process rule. If an Executor discovers such a conflict — including one introduced by a direct instruction accompanying the TASK — it MUST stop and return the conflict rather than resolve it by guessing which side to follow.

## 5. The boundary the Executor does not cross

If implementation surfaces a genuine conceptual defect in the already-approved Canon it was given, the Executor MUST NOT silently redesign around it. It stops and returns the problem to Decision Methodology (`20-DECISION-METHODOLOGY.md`) for the Product Owner and Compiler to resolve, the same way any other Open decision would be resolved.

## 6. Repository discipline

- Working branch and the project's main line carry different authority; an Executor does not move the main line, merge, tag, or publish unless an explicit gate grants it.
- Commit/push of a working branch is not integration; integration is a separate gate; release/publication is separate again from integration.
- History-rewriting operations (force-push, amend, reset, rebase where it discards provenance) are exceptional and require explicit authorization when they would weaken the auditable history of how the result was produced.
- After integration, the resulting canonical repository state MUST be validated, not merely the pre-merge local state; the project's adoption declaration (`60-PROJECT-ADOPTION-AND-VERSIONING.md` §2) MUST identify which role is authorized to integrate and which role performs this post-integration validation, so the gate is executable rather than assumed.

This Codex intentionally universalizes **properties** rather than one fixed mechanism: exact baseline, auditable provenance, no silent history rewriting, an explicit integration gate, and a reproducible resulting state are required everywhere. The specific Git workflow used to satisfy them (fast-forward-only, PR-based merge, branch deletion policy, CI shape) is a project-level choice recorded under `60-PROJECT-ADOPTION-AND-VERSIONING.md`, not fixed by this document.

## 7. Independent implementation review is mandatory before integration

An Executor MUST NOT self-accept its own implementation for integration, and an Actor that executed an implementation Candidate MUST NOT supply a blocking Reviewer `PASS` for that same implementation Candidate — this is the implementation-specific instance of the general candidate-scoped Actor independence rule (`00-CODEX-SCOPE-AND-TERMS.md` §4, Reviewer). At least one independent Reviewer MUST review and `PASS` an implementation candidate before it may pass the integration gate. This is Dogma: it applies to every implementation workstream, though the count and depth of reviewers configuring it is risk-based (below), not fixed.

- Review applies to an exact frozen implementation candidate; once handed to a Reviewer, it MUST NOT be silently modified while under review — a needed change produces a new frozen candidate and re-enters review, the same discipline that governs conceptual candidates (`50-REVIEW-AND-GATES.md`).
- An unresolved `BLOCK` closes the integration gate regardless of how many other signals are green.
- Integration MUST apply to the exact implementation revision that received the required final Reviewer `PASS` — not a revision the Executor or Compiler asserts is "the same in substance." **Any** edit to a frozen, reviewed implementation candidate creates a new revision; a prior `PASS` does not carry over to that new revision, and this is not conditioned on a self-assessment by the party who made the edit that the change was "material." This is the same unconditional any-change rule that governs concept review (`50-REVIEW-AND-GATES.md` §2), applied here to implementation; it is distinct from, and not narrowed by, the delayed-promotion freshness rule, which concerns an *unedited* candidate whose promotion was postponed, not an edited one (`10-DECISION-STORAGE-SYSTEM.md` §3).
- Self-checks, automated tests, and the Executor's own report are evidence inputs to the Reviewer's judgment (`00-CODEX-SCOPE-AND-TERMS.md`, Claim/Evidence), not a substitute for independent review.
- **Reviewer count and depth are configurable by risk**, per the assigning party's judgment for that workstream — this Codex does not mandate one universal risk profile or a fixed reviewer count for implementation review. Where a single Reviewer is assigned, the ordinary N=1 producer/Reviewer correction loop applies, with the Executor as producer (`50-REVIEW-AND-GATES.md` §3). Where more than one Reviewer is assigned, the same independent-first-pass-then-cross-examination mechanics already defined for multiple concept Reviewers apply (`50-REVIEW-AND-GATES.md` §3).
- Concept/candidate review (before Canon promotion) and implementation review (before integration) are related but distinct gates; nothing in this Codex requires them to share the same mandatory reviewer topology or count — each is configured on its own terms.

## 8. Execution environment

An execution environment (e.g. a disposable VM) MAY be temporary, rebuildable, or discarded freely. Project knowledge MUST NOT depend on that environment's survival; durable knowledge belongs in the repository (`10-DECISION-STORAGE-SYSTEM.md`), not in the disposable environment that happened to produce it.
