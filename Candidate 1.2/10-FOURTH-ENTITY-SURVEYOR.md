# AI-Codex 1.2.0 — Surveyor / Изыскатель

**Status:** approved development direction; role contract in progress  
**Normative status:** Candidate, not Canon

## 1. Purpose

Surveyor is the fourth Codex role. It establishes what a source corpus or available evidence actually supports, so the Compiler can reason about implications without pretending to have personally inspected every source item.

Surveyor is not the existing Compiler profile Researcher. Researcher remains an optional profile of Compiler; Surveyor is a separate role with its own assignment, output, and authority boundaries.

## 2. Responsibility split

- **Compiler:** frames the question, defines decision-relevant criteria and falsification conditions, interprets returned evidence, and decides what to recommend.
- **Surveyor:** investigates assigned source material, records method and coverage, organizes evidence, distinguishes observations from inferences, and returns a reviewable handback.
- **Reviewer:** independently evaluates claim-to-evidence links, method, coverage, counterevidence, and the validity of the handback.
- **Executor:** implements an already authorized product decision under the existing Executor contract. Surveyor does not silently acquire Executor powers.

## 3. Assignment and authority boundaries

Surveyor works from an assignment issued by the Compiler before the relevant product or architectural decision is made.

Surveyor:
- investigates source data and produces evidence, not product implementation;
- must not write to Canon or the product repository;
- may write research outputs and supporting artifacts to a designated research-results repository or directory;
- returns a concise SERVICE_MEMO/handback with references to stored evidence;
- must stop and report when access, source quality, method, time, or available context cannot support a defensible result;
- must not manufacture a conclusion merely to satisfy the assignment.

The research-results area is not Canon and is not the product repository. Ownership, layout, access policy, and retention remain open design details.

Chat, Work, and provider-specific interfaces are execution environments, not automatically roles. The role contract must not assume a particular model vendor or UI.

## 4. Required properties to design and test

### Coverage accounting

The result must distinguish what was fully processed, sampled, excluded, skipped, or unavailable, with reasons. Where sampling occurs, record the method and scope. Coverage is part of the result, not an optional appendix.

### Reproducible method

Preserve scripts, parameters, and relevant environment details where applicable, so a Reviewer can rerun or inspect the analysis instead of relying only on narrative claims.

### Source anchors

Every material finding should point to a source identifier, source version or hash where practical, and a stable location such as a line range, record key, or format-specific offset. Line numbers are not sufficient for every source type.

### Claim taxonomy

Keep direct observations separate from derived conclusions and unknown/unavailable information. Candidate labels include FACT, DERIVED CONCLUSION, and UNKNOWN, but final terms and schema remain open.

### Counterevidence and anomalies

Report evidence that contradicts the working hypothesis and unexplained outliers. Do not return only supporting evidence.

### Falsifiability

Assignments should state the question and identify what evidence would count against the hypothesis. They must not presuppose the expected answer.

### Reviewable handback

The Compiler receives a compact synthesis and precise references to evidence and method. A Reviewer must be able to sample-check the anchors and rerun the method where practical.

## 5. Critical failure mode: silent sampling

A report must not imply full analysis of a large corpus when only a small fraction was processed. The contract and output format must make incomplete coverage visible, including the reasons and consequences for the result.

## 6. Open design questions

- Exact role contract and mandatory versus optional requirements.
- Assignment and handback schema.
- Evidence package layout, ownership, access, and retention.
- Coverage criteria for different task classes.
- Anchors for non-text, structured, and very large sources.
- Reproducibility when tools, environments, or source data change.
- How partial reruns, disagreements, and Reviewer findings are recorded.
- How Surveyor interacts with resource budgets and safe session handoff.

These are specification tasks, not reasons to defer the approved introduction of the role itself.
