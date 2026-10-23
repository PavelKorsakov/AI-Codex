# AI-Codex 1.2.0 — Repeatable Codex Acceptance and Key Challenge

**Status:** approved product direction; implementation specification in progress  
**Normative status:** Candidate, not Canon

## 1. Goal and guarantee boundary

The mechanism is intended to provide a testable alternative to trusting a model's self-report that it already accepted Codex or rereading the entire Codex on every new assignment.

A key challenge can reveal that a selected fragment is not available in the active context and trigger recovery. It is not a complete report of what was compressed or lost. It does not prove understanding, authenticate a participant, or prove that a file was read when the participant can search the file through tools.

## 2. Acceptance is not assignment acceptance

- Read and accept the adopted Codex for the current conversation.
- Record the acceptance receipt in the participant's own response, outside the SERVICE_MEMO envelope.
- The receipt is valid only for the current conversation. An incoming memo's claim, another chat, or vendor/account memory does not substitute for it.
- Run Assignment Preflight for every new assignment. Role, scope, allowed reading set, and required actions may differ even when Codex acceptance remains valid.

## 3. Distributed visible markers

Markers are distributed through normative documents, not collected in a separate published key table. A marker is plain visible text on a standalone line between paragraphs, for example:

    CODEX-KEY K-50-07 = amber-falcon-73

The marker has no normative meaning. Its answer is reproduced only in the designated response field. The marker is not an instruction and must not tell a receiver to skip Codex acceptance or bypass Assignment Preflight.

### Initial distribution plan

| Document group | Initial marker count |
|---|---:|
| Seven core normative files 00–60 | 3 per file, 21 total |
| OPEN-LOOP-PROTOCOL.md | 2 |
| FORMAT2.md | 2 |
| Optional Researcher and Skeptic profiles | 1 per profile, 2 total |
| index.md | 0 |
| Explanatory/non-normative files | 0 |
| **Initial total** | **27** |

FORMAT2.md is included as a source for this mechanism. index.md is excluded because a marker there would mostly confirm access to navigation rather than the underlying rules. A profile's key is used only when that profile is assigned to the recipient; optional profiles must not be forced into every acceptance check.

The count is an initial distribution plan, not an eternal invariant. Any change to the approved distribution must be reflected in the generator/validator specification and reviewed package.

## 4. Placement and release process

- A generator places markers initially across the relevant documents.
- For core files, the initial layout uses the first, middle, and last H2 sections as distribution regions, rather than fixed absolute line numbers.
- For OPEN-LOOP-PROTOCOL.md and FORMAT2.md, place two markers in distinct relevant sections.
- After generation, the Product Owner manually moves selected markers up or down, or to other suitable nearby locations, so final positions do not follow an obvious mechanical pattern.
- The final hand-adjusted positions are part of the exact release Candidate.
- A validator checks that expected markers remain present in the correct document, identifiers and answers satisfy the format, keys/answers obey the defined uniqueness policy, and markers remain in allowed content regions. It must not demand that manually moved markers return to generator-default positions.
- Marker generation or movement after a review changes the Candidate. The changed package must be reviewed as a new exact revision.

The proposed line format is plain text, not an HTML comment, so it remains visible in rendered repository views. The key format identifies a file and sequence, for example K-50-07; non-numbered documents can use short identifiers such as K-OLP-03 or K-SKP-02. The answer should use two random English words plus a number to reduce transcription errors from lookalike characters. Avoid wiki/template delimiters.

## 5. Challenge and response

- The sender chooses a key from its own copy of the adopted Codex, preferably from a document relevant to the assignment.
- The recipient returns the corresponding answer in the designated CODEX_RESPONSE field.
- The sender validates the answer against its own copy. A challenge that is never validated is only a ritual.
- Each memo carries a challenge for its recipient and a response to the preceding sender's challenge, according to the finalized schema.
- The first memo to a new conversation has no prior response to provide.
- A memo response to a challenge-bearing memo without the required response is invalid.
- CODEX_VERSION identifies the declared version and must not embed a commit or other data that can simply be copied in place of answering the challenge.

A single successful key response shows availability of the selected fragment, not full comprehension or availability of every rule. Key selection is a sample and must not be described as proof of complete Codex retention.

## 6. Recovery and context state

If the expected answer is not available:
1. reread the **full adopted Codex**, not only the file containing the selected marker;
2. retry the challenge;
3. if the answer remains unavailable, report NOT_FOUND and STOP pending diagnosis.

The failure and version mismatch are distinct signals:
- Compare CODEX_VERSION to establish a declared version mismatch.
- NOT_FOUND means the answer was not found after the prescribed recovery attempt. It does not by itself prove a version mismatch or prove the cause of the failure.

Candidate context-state values:
- **INTACT:** challenge answered from currently available context, with no loss detected by this check;
- **RULES_RELOADED:** the challenge failed, full Codex was reread, and the challenge then succeeded; historical claims not supported by current source anchors must not be treated as reliable;
- **DEGRADED:** recovery failed; do not assign new work in this conversation, and hand off from reliable sources to a new conversation.

The state can only worsen within a conversation. Once historical context may have been lost, rereading the normative Codex does not restore the missing work history. A new conversation starts a new acceptance cycle.

If a recipient receives a missing or incorrect response to its challenge, it must not blindly trust the sender's self-reported context state. The exact transition rules and field requirements must be encoded in the normative protocol and validator.

## 7. Schema boundary

The v1.1.0 ai-codex/service-memo/1 schema does not permit arbitrary new fields. Do not backdate the proposed fields into v1.1.0. The required fields and behavior must be specified in a new schema version for v1.2.0.

Candidate field concepts:
- CODEX_VERSION
- CODEX_CHALLENGE
- CODEX_RESPONSE
- CONTEXT_STATE

The finalized schema must specify required/optional behavior, first-message handling, response validation, recovery, version mismatch, and STOP conditions. These fields carry claims and challenge data; they are not cryptographic authentication.

## 8. Key reuse policy

Global non-reuse is not viable for a small pool of approximately 27 keys across a long-running project. The intended direction is to prevent reuse of a key for the same recipient within the most recent N challenge-bearing memos, rather than forbidding reuse forever.

N and the tracking scope remain to be specified. If enforcement depends on history shared across multiple conversations, the implementation must state how that history is available. Do not claim guaranteed cross-chat enforcement without a mechanism to support it.

## 9. Scope boundary

The mechanism is a practical context-availability check, not full context telemetry. A failed challenge is a useful signal to recover or end a conversation. It does not reveal all omitted information, and a successful challenge does not establish that no unrelated information was lost.

A tool-enabled participant may search a file for a marker without reading or retaining the rest of the Codex. In that environment, the challenge proves access to the correct marker/version more than reading. Strong session-bound delivery verification would require a server-backed mechanism and is outside the current 1.2.0 design unless separately approved.
