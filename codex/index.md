# AI-Codex v1.1.0 — Entry Point

Release target: **v1.1.0**

Authoritative lifecycle state is external to this immutable payload.

## Normative content

Read these together:

- [00-CODEX-SCOPE-AND-TERMS.md](00-CODEX-SCOPE-AND-TERMS.md)
- [10-DECISION-STORAGE-SYSTEM.md](10-DECISION-STORAGE-SYSTEM.md)
- [20-DECISION-METHODOLOGY.md](20-DECISION-METHODOLOGY.md)
- [30-IMPLEMENTATION-METHODOLOGY.md](30-IMPLEMENTATION-METHODOLOGY.md)
- [40-ROLES-AND-AUTHORITY.md](40-ROLES-AND-AUTHORITY.md)
- [50-REVIEW-AND-GATES.md](50-REVIEW-AND-GATES.md)
- [60-PROJECT-ADOPTION-AND-VERSIONING.md](60-PROJECT-ADOPTION-AND-VERSIONING.md)

## Machine representation

Generated self-contained JSONL/NDJSON projection:

- [format2/index.jsonl](format2/index.jsonl)
- [FORMAT2.md](FORMAT2.md)
- [schemas/canon-record.schema.json](schemas/canon-record.schema.json)

Markdown above remains authored and authoritative. FORMAT-2 is generated and validated at lifecycle boundaries.

## Open-loop protocol

- [OPEN-LOOP-PROTOCOL.md](OPEN-LOOP-PROTOCOL.md)
- [schemas/open-loop-registry.json](schemas/open-loop-registry.json)

The open-loop protocol covers load-bearing fields for TASK, Review Assignment, and Service Memo.

## Optional Role Context Profiles

- [profiles/Researcher.md](profiles/Researcher.md) — Compiler
- [profiles/Skeptic.md](profiles/Skeptic.md) — Reviewer

Profiles are optional and non-authoritative.

## Scope and boundaries

See [00-CODEX-SCOPE-AND-TERMS.md](00-CODEX-SCOPE-AND-TERMS.md) §§1–2.

## Rationale and rejected alternatives

See [RATIONALE.md](RATIONALE.md).

## Adoption and migration

- [ADOPTION.md](ADOPTION.md)
- [MIGRATION-v1.0.0-to-v1.1.0.md](MIGRATION-v1.0.0-to-v1.1.0.md)

## Content identity

See [CONTENT-MANIFEST.md](CONTENT-MANIFEST.md).

## Review evidence and authoritative lifecycle status

Stable public Promotion Record locator for this release line, **relative to the public repository root**:

`promotion/v1.1.0.md`

From this `codex/index.md` location, the equivalent public relative path is:

`../promotion/v1.1.0.md`

The Working Repository may hold the mutable lifecycle record at a different Working-only path while the Candidate is under review. Publication MUST materialize it at the repository-root locator above without editing this reviewed payload.

Until a valid Promotion Record at the stable public locator records Product Owner `PROMOTE` for the exact reviewed payload, the Candidate is **not Canon**.

Review evidence is linked from the Promotion Record and remains external to the immutable reviewed payload.

## Changelog

See [../CHANGELOG.md](../CHANGELOG.md).

## Source of truth

The normative source of truth is the seven numbered Markdown documents listed above, interpreted together with the Promotion Record for lifecycle status. FORMAT-2 is the generated machine projection of that same frozen semantic state.
