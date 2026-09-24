# Migrating AI-Codex v1.0.0 Projects to v1.1.0

Migration is an explicit project act. It does not rewrite the compliance status, provenance, or review history of decisions made under v1.0.0.

## 1. Pin v1.1.0 explicitly

Update the Project Adoption Declaration only through the project's authorized process.

Record the exact AI-Codex release and public commit used.

## 2. Audit the declared Canon namespace

Find any Working/Candidate/review/correction material staged under the project's declared Canon namespace.

v1.1.0 reserves that namespace for promoted Canonical material only.

Do not silently relocate or rewrite historical canonical evidence. Build a migration Candidate when Canon-visible structure must change.

## 3. Normalize Canonical Revisions

For each existing promoted component:

- map the already-promoted current Canon state to `R001` unless the project already has a compliant `RNNN` history;
- preserve the original promotion provenance;
- treat `R001` as the Canonical Revision identity of the already-promoted accepted state, not as a claim that a historical product/SemVer release occurred at migration time;
- create the stable component `index.md`;
- preserve historical `RNNN` revisions immutably.

Future promoted changes become `R002`, `R003`, and so on. If a migration Candidate itself changes accepted Canon semantic content rather than only recording/normalizing the already-promoted state and its provenance, that changed Canon state follows the ordinary Candidate → Review → PROMOTE lifecycle and receives the next applicable `RNNN`.

## 4. Add deterministic lifecycle evidence discovery

The component entry point must deterministically locate review/promotion lifecycle evidence for every Canonical Revision.

Recommended reference layout:

```text
Canon/<component>/
  index.md
  R001/
  lifecycle/
    R001/
```

The literal `lifecycle/` directory name is a reference mechanism; deterministic discoverability is the required property.

## 5. Generate FORMAT-2

For current Canonical Revisions:

- keep Markdown authored/authoritative;
- generate JSONL/NDJSON FORMAT-2;
- validate it against the adopted schema;
- verify source mapping;
- freeze/promote the machine representation with the same semantic revision.

Do not manually maintain a parallel machine truth.

## 6. Adopt open-loop envelopes

New TASK, Review Assignment, and transported Service Memo artifacts use the v1.1.0 line-oriented envelope.

Existing historical artifacts do not need retrospective rewriting.

## 7. Bootstrap and preflight

New zero-memory Compiler/Reviewer sessions:

- resolve missing bootstrap parameters;
- acknowledge successful onboarding minimally instead of reciting the Codex;
- perform Assignment Preflight before substantive work.

Executors perform Execution Contract Validation at TASK intake.

## 8. Optional profiles

A workstream may opt into:

- `Researcher` for Compiler;
- `Skeptic` for Reviewer.

The profiles are non-authoritative and do not change Role authority.

## 9. Validate the migration

Before declaring the project migrated, verify:

- pinned v1.1.0 identity;
- the declared Canon namespace contains promoted Canonical material only; any legacy Working/Candidate/review/correction/staging material previously placed there has been explicitly migrated or otherwise remediated without falsifying provenance;
- each versioned component has a current `RNNN`;
- lifecycle evidence is deterministically discoverable;
- FORMAT-2 exists and validates where required;
- new open-loop artifacts use valid envelopes;
- historical provenance remains truthful.
