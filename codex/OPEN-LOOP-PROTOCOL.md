# Open-Loop Operational Protocol

AI-Codex v1.1.0 uses a small deterministic line-oriented envelope for load-bearing fields of:

- TASK;
- Review Assignment;
- Service Memo.

## Grammar

Reference field-line grammar:

```text
FIELD ":" SP VALUE LF
```

For v1.1.0:

- `FIELD` names are case-sensitive;
- `VALUE` MUST be non-empty unless the selected TYPE schema explicitly permits an empty value; no shipped v1.1.0 TYPE does;
- no blank line is permitted between field lines;
- at most one empty physical line MAY appear after the final field line and before the body delimiter for readability; a parser MUST ignore that one line;
- when a Markdown body exists, the first physical line exactly equal to `---` after the envelope fields terminates the envelope;
- when no body exists, the delimiter MAY be omitted and EOF terminates the envelope;
- `---` lines appearing after the first body delimiter belong to the Markdown body.

A deterministic delimiter separates the envelope from an optional Markdown body:

```text
---
```

The body may contain explanation, examples, rationale, or conversational tone. A parser MUST NOT need the body to recover load-bearing routing/execution data.

## Schema identity

Every envelope begins with:

```text
TYPE: <artifact type>
SCHEMA: <schema id/version>
```

The machine-readable reference registry is:

`schemas/open-loop-registry.json`

For each TYPE it defines:

- required fields;
- optional fields;
- scalar fields;
- repeatable fields;
- field value types/enums where applicable;
- duplicate behavior;
- extension behavior;
- semantic constraints that cannot be reduced to simple field presence/cardinality.

Duplicate behavior is deterministic:

- a scalar field appearing more than once makes the envelope invalid;
- a repeatable field MAY appear more than once; its values are appended in encounter order.

Structural schema validity is necessary but not sufficient for semantic contract validity. A structurally valid TASK, Review Assignment, or Service Memo still MUST satisfy the normative content obligations for that artifact class.

## No accidental new language

The envelope does not use semantic indentation, anchors, YAML implicit typing, arbitrary nesting, or hidden coercion.

Complex nested data belongs in a referenced attachment.

For Service Memo artifact maps, the repeatable `ARTIFACT` value uses:

```text
ARTIFACT: <path> | <purpose>
```

`SUMMARY` is mandatory for Service Memo. `FROM_ROLE` / `TO_ROLE` are optional there unless Role identity is load-bearing for the handoff.

Where relevant repository/file artifacts exist, `ARTIFACT` entries MUST identify them with both path and purpose. `READ` or `ATTACHMENT` alone do not satisfy that artifact-map content obligation.

For TASK, `ALLOWED`, `TEST`, and `HANDBACK` are structurally required at least once because the normative TASK contract requires those semantic categories. If a category genuinely has no applicable item, state that explicitly rather than omitting the field.

## Example Review Assignment

```text
TYPE: REVIEW_ASSIGNMENT
SCHEMA: ai-codex/review-assignment/1
FROM: ChatGPT
FROM_ROLE: Compiler
TO: Reviewer-A
TO_ROLE: Reviewer
PURPOSE: INDEPENDENT_REVIEW_STAGE1
REPOSITORY: owner/repository
BRANCH: review/candidate
CANDIDATE_REVISION: <exact immutable revision>
READ: <candidate entry point>
READ: <authorized context>
OUTPUT: <result path>
STOP_AFTER_COMPLETION: true

---
Optional human-readable context.
```
