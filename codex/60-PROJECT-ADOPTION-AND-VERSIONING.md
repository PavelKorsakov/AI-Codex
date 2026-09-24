# 60 — Project Adoption and Versioning

Release target: **AI-Codex v1.1.0**

Authoritative lifecycle status is external to this immutable payload and is resolved through the applicable Promotion Record.

This document governs how a concrete project binds itself to this Codex, and how the Codex itself changes over time without invalidating past decisions made under an earlier version.

## 1. Pinning an exact version

A project MUST declare the exact AI-Codex version it operates under, by release and exact commit, not by an informal reference such as "the latest Codex":

```text
AI-Codex version: v1.2.0
Commit: <exact SHA>
```

A project MUST NOT claim to follow "whatever the Codex currently says" — a later change to the Codex must not retroactively change the rules under which an existing project decision was made.

AI-Codex release version and Project Canon Canonical Revision are different coordinates. AI-Codex itself uses release versions such as `v1.1.0`; a versioned Project Canon component uses `R001`, `R002`, ... for its accepted Canon states. A project MUST NOT treat `RNNN` as SemVer or as an implementation/product release number.

## 2. Project-level configuration

A project's adoption declaration is where project-specific configuration choices this Codex intentionally leaves open are recorded, including but not limited to:

- actor-to-role assignments for the workstream(s) in use;
- the concrete Git workflow satisfying the properties required by `30-IMPLEMENTATION-METHODOLOGY.md` §6 (e.g. fast-forward-only vs. PR-based integration, branch naming and deletion policy, CI requirements);
- the number and identity of blocking Reviewers per workstream (`50-REVIEW-AND-GATES.md` §3);
- whether the project's Canon is public, and if so, its physically separate Canon Repository location (`10-DECISION-STORAGE-SYSTEM.md` §1);
- who performs and who authorizes the public-disclosure check before a public-repository commit, when it is not simply the authoring Compiler alone (`10-DECISION-STORAGE-SYSTEM.md` §6);
- which role is authorized to integrate an implementation candidate and which role performs post-integration validation (`30-IMPLEMENTATION-METHODOLOGY.md` §6);
- any project-specific allowed deviation from this Codex, stated explicitly rather than assumed.

## 3. Migration

Moving a project to a newer Codex version MUST be an explicit, recorded act, not an implicit drift. Historical decisions remain attributable to the Codex version in force at the time they were made; a version migration does not retroactively mark a past decision non-compliant.

A migration from v1.0.0 to v1.1.0 MUST explicitly evaluate at least:

- whether any pre-promotion material currently lives under a declared Canon namespace;
- how existing Canon components map to `R001` without falsifying historical promotion provenance;
- creation of the stable component `index.md` and deterministic lifecycle-evidence locator;
- generation/validation of FORMAT-2 for current Canon revisions;
- adoption of the open-loop envelope schemas for new TASK / Review Assignment / Service Memo traffic;
- bootstrap/preflight behavior changes;
- any project-local Adoption text that previously permitted Candidate staging under `Canon/`.

Historical immutable Canon content MUST NOT be silently rewritten merely to resemble the v1.1.0 reference layout. Structural normalization that changes Canon-visible material follows the applicable Candidate → Review → PROMOTE lifecycle.

## 4. Compliance and deviation

A project claiming **AI-Codex compliant** status MUST satisfy all Dogma of its pinned Codex version without exception (`00-CODEX-SCOPE-AND-TERMS.md`, AI-Codex compliant / AI-Codex-derived). The project adoption declaration (§2) may configure only explicitly configurable choices, Interpretation-level behavior, and project-specific mechanisms that still satisfy every mandatory Codex property — it MUST NOT be used to silently override Dogma (review gates, evidence obligations, or other hard requirements) while the project continues to claim full compliance.

A project MAY deliberately depart from Dogma. If it does, it MUST NOT describe itself as AI-Codex compliant; it MUST identify as **AI-Codex-derived** or **AI-Codex-modified** (or an equivalent explicit label) and record every such deviation in its adoption declaration.

## 5. Typed normative precedence (the typed-precedence rule)

When two governing layers of a project conflict, this Codex does not use a single linear "highest authority wins" ladder, and it does not use a "latest instruction wins" rule. Each layer holds a distinct type of authority, and which layer governs depends on the type of conflict:

- **AI-Codex Dogma — process authority.** Governs how a compliant project decides, reviews, stores, promotes, implements, and integrates. Neither a Project Adoption Declaration, a TASK, nor a direct Product Owner instruction may silently override Dogma while the project still claims AI-Codex compliance (§4).
- **Project Adoption Declaration — configuration authority only.** MAY configure explicitly configurable Codex choices, Interpretation-level behavior, and concrete project mechanisms that satisfy Dogma. MUST NOT override Dogma. MUST NOT silently rewrite Project Canon.
- **Project Canon — current project truth.** While in force, downstream work MUST follow it. A conflicting direct Product Owner statement does not silently replace it. The Product Owner MAY immediately STOP, CANCEL, PAUSE, or REOPEN affected work, and MAY initiate a new Candidate proposing to change or supersede Canon — but the new truth becomes Canon only after that Candidate completes the ordinary Candidate → Review → PROMOTE lifecycle (`10-DECISION-STORAGE-SYSTEM.md` §3).
- **Direct Product Owner instruction — immediate control, not silent override.** MAY immediately control whether work continues. MUST NOT bypass Codex Dogma while retaining compliant status, fabricate a Reviewer PASS, override a valid unresolved `BLOCK` directly, or silently replace existing Canon. A direct instruction conflicting with Canon MUST cause the affected work to stop/reopen and return to Decision Methodology (`20-DECISION-METHODOLOGY.md` §4) unless the instruction is purely operational and does not assert different Canon content.
- **TASK — subordinate execution contract.** Subordinate to AI-Codex Dogma, permitted Adoption configuration, and current Project Canon. MUST NOT redefine product truth or weaken a mandatory process rule. If a TASK conflicts with any higher governing layer, the Executor MUST stop and return the conflict (`30-IMPLEMENTATION-METHODOLOGY.md` §4).

**Deterministic conflict resolution, at minimum:**

- Codex process conflict → Codex Dogma governs, for an AI-Codex-compliant project.
- Adoption-vs-Dogma conflict → Dogma governs, or the project explicitly becomes AI-Codex-derived/modified (§4).
- TASK-vs-Canon conflict → Canon governs; the TASK stops (`30-IMPLEMENTATION-METHODOLOGY.md` §4).
- Direct Product Owner instruction vs. Canon conflict → the affected work stops/reopens; Canon changes only via Candidate → Review → PROMOTE.
- Product Owner preference vs. a valid Reviewer `BLOCK` → the `BLOCK` remains until correction, disproof, or adjudication resolves it (`50-REVIEW-AND-GATES.md` §5).

## 6. Self-governance of the Codex itself

The AI-Codex is itself produced under the same discipline it defines: a change to the Codex is a Candidate that MUST pass the same review-and-promotion discipline (`50-REVIEW-AND-GATES.md`, `10-DECISION-STORAGE-SYSTEM.md`) before becoming the new authoritative version.

A release Candidate for the AI-Codex itself MUST be assembled and reviewed as a coherent release package, not only as changed normative `00–60` files. Before freeze, the package MUST include current public-facing documentation and required supporting artifacts, including a root `CHANGELOG.md`, and MUST perform a deterministic version sweep for stale current-version/status/path claims. Historical references to prior releases MAY remain when clearly historical.

The Candidate manifest MUST cover the intended public payload sufficiently completely for Reviewers to assess package/version consistency, excluding only lifecycle evidence that cannot legitimately exist until the review/PROMOTE events occur. Any post-review edit to the reviewed payload triggers the ordinary new-Frozen-Revision rule.

This document still does not prescribe one universal backward-compatibility or deprecation policy beyond explicit migration and release-package integrity; later versions MAY add one if operational evidence requires it.
