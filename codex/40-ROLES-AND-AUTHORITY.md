# 40 — Roles and Authority

Release target: **AI-Codex v1.1.0**

Authoritative lifecycle status is external to this immutable payload and is resolved through the applicable Promotion Record.

## 1. Roles are independent of actors

Every obligation and authority in this document attaches to a Role, not to a specific model, product, or person. A concrete workstream assigns Actors to Roles, e.g.:

```text
Role: Compiler    Actor: Model-A
Role: Reviewer    Actor: Model-B
Role: Reviewer    Actor: Model-C
Role: Executor    Actor: Tool-A
Role: Adjudicator Actor: Model-D
```

Changing the Actor assigned to a Role does not change what that Role is obligated or authorized to do.

Actor identity is explicitly assigned for the workstream and MUST NOT be inferred solely from vendor or model family. Two separately assigned participants from the same vendor/model family MAY be distinct Actors. Conversely, opening a new session/chat/context for the same Actor does not create independence.

## 2. Role definitions

**Product Owner** (human) — owns product decisions; participates in compiling a candidate as the source of product direction; authorizes promotion (§3); may decline or postpone promotion even after all required review PASS states exist, but may not override a valid unresolved Reviewer `BLOCK` by fiat. What is decided: Product Owner authority does not itself count as a Reviewer `PASS`, and the Product Owner does not act as a tie-breaker inside the review gate. What is not yet decided beyond the minimum in §5 below: the full Role Combination matrix.

**Compiler** — produces a candidate decision or plan through research and deliberation with the Product Owner (and, in Multi-Compiler mode, cross-reading another Compiler's independent package); freezes revisions for review; reports the state of required PASS signals to the Product Owner; performs corrections in response to Reviewer findings; owns the public-disclosure check for material it authors (`10-DECISION-STORAGE-SYSTEM.md` §6). Before substantive work under an open-loop assignment, the Compiler performs Assignment Preflight (§7).

**Reviewer** — independently attempts to falsify a frozen candidate. Before substantive review work, the Reviewer performs Assignment Preflight (§7) and MUST NOT begin substantive project analysis until an exact Review Assignment is received. The number of blocking Reviewers assigned to a workstream is chosen by the Product Owner or the applicable delegated authority model for that project (the reviewer-assignment rule); that assignment configuration MUST NOT be set up in a way that defeats the Actor-level independence rule in §5. See `50-REVIEW-AND-GATES.md` for how the protocol scales with reviewer count.

**Adjudicator** — resolves one specifically disputed `BLOCKING` finding when the Reviewer and Compiler remain in material disagreement after the Compiler has offered counter-evidence (`00-CODEX-SCOPE-AND-TERMS.md` §4, `50-REVIEW-AND-GATES.md` §5). Not a fourth reviewer of the whole Candidate and not a promotion-authority holder.

**Executor** — implements an approved Canon component under a `TASK` contract (`30-IMPLEMENTATION-METHODOLOGY.md`); performs Execution Contract Validation at TASK intake; does not silently repair a conceptual defect in Canon; does not move gates it has not been explicitly authorized to move.

## 3. Promotion authority

A successful review gate makes a candidate Promotion-Ready. In a single-Compiler workstream, this requires Compiler PASS plus every assigned Reviewer's final (gate-bearing) PASS on the same frozen revision. In a Same-Problem Multi-Compiler workstream, "Compiler PASS" requires PASS from **every participating Compiler** on the same frozen synthesis revision (the Same-Problem Multi-Compiler unanimity rule; see `10-DECISION-STORAGE-SYSTEM.md` §3 and `20-DECISION-METHODOLOGY.md` §6) — this Compiler-side unanimity is separate from, and does not substitute for, the independent Reviewer PASS(es) required afterward, and is not itself a review verdict. A successful gate does not automatically promote the candidate. The Compiler reports the Promotion-Ready state to the Product Owner. Only the Product Owner's separate, explicit authorization ("PROMOTE") moves a Promotion-Ready candidate into Project Canon. This authorization is not an additional review vote, and it does not create or override a valid Reviewer `BLOCK` in either direction.

### 3.1. Immediate control is not the same authority as changing Canon

The Product Owner's authority to STOP, CANCEL, PAUSE, or REOPEN affected work is immediate and does not wait for any gate (`00-CODEX-SCOPE-AND-TERMS.md` §4, Product Owner). This is a distinct authority type from the power to change what Project Canon says, which — like any other Canon change — requires initiating a Candidate and carrying it through the ordinary Candidate → Review → PROMOTE lifecycle (`10-DECISION-STORAGE-SYSTEM.md` §3, typed normative precedence in `60-PROJECT-ADOPTION-AND-VERSIONING.md` §5). A Product Owner instruction that stops work is always effective immediately; a Product Owner instruction that asserts different Canon content is not itself a Canon change until that lifecycle completes.

## 4. What this document deliberately leaves open

The relationship between roles as *obligations* (this document) and roles as *permissions over specific repository or infrastructure actions* is intentionally not fully specified here; a project's adoption declaration (`60-PROJECT-ADOPTION-AND-VERSIONING.md`) MUST bind concrete permissions to roles where a mandatory gate would otherwise be non-executable (e.g. who may integrate, `30-IMPLEMENTATION-METHODOLOGY.md` §6).

## 5. Candidate-scoped Actor independence (the candidate-scoped independence rule) — decided; broader combination — OPEN/TBD

**Decided, minimum floor:** for a specific Candidate, an Actor that participated in compiling/producing that Candidate MUST NOT supply a blocking Reviewer `PASS` for that same Candidate; an Actor that acted as Executor for an implementation Candidate MUST NOT supply a blocking Reviewer `PASS` for that same implementation Candidate. Starting a new session, chat, context, or memory state does not create independence — independence is about participation in producing the reviewed artifact, not conversational state. The same Actor MAY hold different Roles on other Candidates, other independently produced revisions, or other workstreams, subject to any broader Role Combination rule later adopted. Reviewer-assignment authority (§2, Reviewer) MUST NOT be configured in a way that defeats this minimum.

**OPEN/TBD, broader question:** which other Role combinations a single Actor MAY hold within one workstream beyond the minimum above (e.g. may a Compiler also Integrate; may a Reviewer later become an Executor for a different candidate; may the same Actor occupy different Roles on different parallel branches), and which of those combinations are forbidden as creating unacceptable self-review. This requires a further Product Owner decision and MAY be resolved separately from, and later than, the decided minimum above.

## 6. OPEN/TBD — emergency path

Not yet decided: whether any bypass of the normal gate sequence should be affirmatively created for emergency conditions, and if so, who may invoke it, what must be recorded, and what mandatory retrospective review follows. **This absence does not make the current process non-deterministic**, because no bypass is authorized anywhere in this Codex, none exists, and every mandatory gate (`50-REVIEW-AND-GATES.md`, `30-IMPLEMENTATION-METHODOLOGY.md` §7) continues to apply exactly as written with no implicit escape hatch. What remains genuinely open is only whether such a bypass should be *added* — this requires a Product Owner decision before an affirmative bypass path can be treated as settled, and this Codex takes no position on whether one is desirable.


## 7. Bootstrap, Assignment Preflight, and optional Role Context Profiles

### 7.1. Minimal bootstrap

A zero-memory Actor MUST resolve the load-bearing bootstrap parameters required by the workstream before substantive work begins. Role is the closest-to-universal parameter; repository, topic/workstream, adopted Codex/project configuration, exact assignment/dispute, or other prerequisites MAY also be required depending on Role.

Until required parameters are resolved, the Actor MUST ask directly for what is missing and MUST NOT speculate or begin partial substantive work.

The Actor MAY perform whatever internal reading, provenance resolution, and integrity checks are necessary. If those checks succeed, the Actor MUST acknowledge successful onboarding with one minimal deterministic receipt rather than volunteering a Codex recap, Promotion Record narrative, proof-of-reading transcript, or other verification story unless the Product Owner explicitly asks for one.

The receipt MUST show:

- the resolved Role;
- the resolved repository/workstream or equivalent scope when applicable;
- the governing Codex/project state resolved for the work;
- the next required action/state.

If onboarding reveals a material unresolved conflict, the Actor MUST report the exact actionable conflict and STOP.

Known parameters MUST NOT be re-asked. Dependency order governs: a Role-dependent field cannot be resolved before Role is known.

Role-specific intake then applies:

- Compiler and Reviewer perform Assignment Preflight (§7.2);
- Executor performs Execution Contract Validation (`30-IMPLEMENTATION-METHODOLOGY.md` §2.1) rather than the interactive Compiler/Reviewer preflight ritual;
- Adjudicator resolves the exact disputed finding, appointment, narrow scope, and independence prerequisites before substantive adjudication.

### 7.2. Assignment Preflight

Before substantive work under an open-loop assignment, Compiler and Reviewer MUST validate the assignment against:

- the adopted AI-Codex version;
- Project Adoption/configuration;
- current Project Canon;
- assigned Role and Role authority;
- lifecycle/storage boundaries;
- the exact assignment contract.

A material conflict causes:

```text
STOP
DO NOT EXECUTE
RETURN THE CONFLICT
```

A successful preflight does not require a permanent artifact unless the workstream explicitly requires one.

### 7.3. Role Context Profiles

AI-Codex v1.1.0 ships optional reference Role Context Profiles:

- Compiler: `Researcher`;
- Reviewer: `Skeptic`.

A profile is non-authoritative. It MAY shape posture, style, attention priorities, and useful defaults, but MUST NOT grant authority, weaken Dogma, change gates/PASS/BLOCK, alter Reviewer independence, change Canon, answer OPEN/TBD questions, or override an assignment.

Role MUST be known before a Role-dependent profile is offered. Profile selection is scoped to the workstream, not merely the chat/session. Use is optional and SHOULD remain observable during dogfooding so later Codex releases can evaluate whether the profiles materially improve outcomes.
