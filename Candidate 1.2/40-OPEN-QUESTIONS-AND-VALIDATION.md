# AI-Codex 1.2.0 — Open Questions and Validation Plan

**Status:** work plan for specification and testing  
**Normative status:** Candidate, not Canon

## 1. Surveyor

- Complete and review the role contract and authority boundaries.
- Define assignment and handback schemas, including required artifact references.
- Set coverage requirements by task type and specify how sampling/exclusions are reported.
- Define stable source anchors for text, structured files, binaries, and large corpora.
- Define reproducibility expectations for scripts, parameters, tools, and changing source data.
- Specify partial completion, STOP, unknowns, anomalies, counterevidence, and Reviewer verification.
- Define research-result storage and retention without confusing research artifacts with Canon or product state.
- Clarify resource budget and session-handoff requirements.

## 2. Codex acceptance and key challenge

- Confirm the normative file inventory against the exact v1.1.0 release payload.
- Specify marker syntax, identifier namespace, answer format, uniqueness rules, and allowed placement.
- Build a generator that initially distributes markers according to the approved document groups.
- Ensure manual post-generation movement is supported and preserved.
- Build a validator that checks marker presence, file association, syntax, allowed region, and uniqueness without restoring default positions.
- Specify challenge selection, answer validation, and the recipient's applicable profile.
- Define how key reuse is tracked per recipient over the most recent N challenge-bearing memos and across conversation boundaries, if required.
- Specify first-message behavior, missing/incorrect responses, NOT_FOUND, version mismatch, context-state transitions, and STOP conditions.
- Introduce the new SERVICE_MEMO schema version without changing v1.1.0 schema semantics.
- Test ordinary chat behavior separately from tool-enabled environments where markers can be found by file search.
- Test missing key, wrong answer, wrong version, recovery success, recovery failure, profile-specific challenge, and attempts to copy a response without validation.
- Document the guarantee boundary: this is a practical check of selected-fragment availability, not complete context telemetry or authentication.

## 3. General v1.2.0 release discipline

- Keep the current v1.1.0 Canon unchanged during 1.2.0 development.
- Maintain exact, reviewable Candidate revisions.
- Do not treat a Compiler recommendation, SERVICE_MEMO, or a Reviewer result as PO authorization.
- Freeze the complete package before independent review.
- Re-review every changed Candidate revision after review findings are corrected.
- Publish or promote only after the applicable independent gates and a separate direct PO decision.
- Keep public Candidate contents free of private project examples, credentials, and private Working-repository operational details.

## 4. Baseline and corpus identity

The v1.1.0 corpus must be checked against the exact release commit, not assumed identical because a working clone reports a nearby commit. The key inventory and generator configuration must name the verified source revision. The current marker counts are a design plan and must be reconciled with the verified corpus before implementation is frozen.

## 5. Decisions still required

- Final Surveyor contract and schemas.
- Final SERVICE_MEMO schema version and field semantics.
- Whether any part of the 24-item backlog is added to the implementation scope beyond PO-1 and PO-2.
- Exact N and tracking mechanism for key reuse.
- Validator acceptance rules for manually moved markers.
- Final test suite and acceptance criteria for the full 1.2.0 Candidate.
