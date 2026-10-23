# AI-Codex 1.2.0 — Remaining Development Direction

**Status:** working backlog; not blanket-approved scope  
**Rule:** only direct Product Owner decisions explicitly recorded in 00-STATUS-AND-PO-DECISIONS.md authorize implementation. The presence of an idea here does not itself approve it.

## A. Work windows, budgets, and session completion

1. Pass assignment budget with the assignment, including time/context/resource limits and an upfront feasibility assessment.
2. Explore awareness of the remaining work window, including an agent-kick concept inspired by finite-time research.
3. Require a correct durable session ending: completed and checked work, saved evidence, remaining work, limits, and exact continuation point.
4. Protect independent Reviewer depth from pressure to fit the remaining quota. If the window is insufficient, record incompleteness honestly.
5. Improve reliable handoff between conversations so important decisions and final messages are not lost at chat boundaries.

## B. Profiles, roles, and assignment structure

6. Evaluate Lightweight / Standard / Full profiles and their tradeoffs.
7. Standardize explicit assignment requirements, expected outputs, evidence, and available budget.
8. Clarify role responsibility and Reviewer independence.
9. Evaluate a separate adversarial second coder whose job is to break, find errors, and improve an implementation.

## C. SERVICE_MEMO and result validation

10. Align operational Skills with the normative SERVICE_MEMO schema and prevent local conventions from being presented as Canon.
11. Decide whether UNKNOWN should be reserved in v1.2.0; do not present it as a v1.1.0 reserved value without authority.
12. Decide whether to formalize the RESULT model.
13. Test complete and incomplete inputs, required artifact formatting, honest unknowns, and prohibition on invented facts.
14. Separate structural validation, semantic Review, and authority/provenance verification.
15. Evaluate future message boundary markers and fields without backdating them into v1.1.0 or adding unregistered fields to its schema.

## D. Process reliability

16. Evaluate Review Coverage: what was checked, not checked, and why.
17. Evaluate typed attachments with explicit type, purpose, and verification expectations.
18. Evaluate a bounded correction cycle/circuit breaker.
19. Define STOP behavior for context or working-condition degradation.
20. Strengthen authority provenance so sender claims are not mistaken for independently verified authority.

## E. Loading and discoverability

21. Evaluate loading required materials on demand without losing discoverability of applicable normative rules.
22. Test whether a participant can find the relevant rules, schemas, tools, and instructions at the point of need.

## F. Prototypes and empirical tests

23. Evaluate a minimal CLI/validator for schemas and process rules.
24. Run behavioral tests on realistic scenarios, including missing data, session limits, handoff, authority, and honest reporting. Operational defects and experiments are evidence for decisions, not automatic permission to change Dogma.

## Explicitly not automatically in 1.2.0

- Long-term project memory L0–L4, archive reconstruction, context-pressure estimates based on actual token use, and automatic chat-transition preparation.
- Provider plugin/connector service, separate per-role stores/indexes, and Executor sandbox.
- Large architectural refactors, broad universalization beyond the current target, or unapproved schema redesign.
- Literal turning a model into a pumpkin at midnight as a normative requirement. The substantive objective is timely boundary detection and correct session closure.

These items may be reconsidered through explicit scope decisions. Their presence here is not approval, rejection, or permission to bypass the usual Candidate/review/promotion lifecycle.
