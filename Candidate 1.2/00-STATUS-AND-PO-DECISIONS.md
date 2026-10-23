# AI-Codex 1.2.0 — Status and Product Owner Decisions

**Status:** direct Product Owner development decisions  
**Normative status:** Candidate material, not Canon  
**Baseline:** AI-Codex v1.1.0 remains unchanged

## PO-1 — Fourth entity and role/profile clarification

The introduction of a fourth Codex entity is a mandatory development direction for v1.2.0. Its final name is **Surveyor / Изыскатель**.

The prompt/instruction contract for Surveyor will be developed and refined during the 1.2.0 assembly. It does not need to be fully designed before work begins.

The same assembly must clarify the instructions for the three existing entities where needed to keep responsibilities, handoffs, and authority boundaries coherent.

| Type | English | Russian | Status |
|---|---|---|---|
| Role | Compiler | Компилятор | Existing, unchanged |
| Compiler profile | Researcher | Исследователь | Existing, unchanged |
| Role | Reviewer | Ревьювер | Existing, unchanged |
| Reviewer profile | Skeptic | Скептик | Existing, unchanged |
| Role | Executor | Экзекутор | Existing, unchanged |
| Executor profile | Coder | Кодировщик | New optional profile for v1.2.0 |
| Role | Surveyor | Изыскатель | New fourth role for v1.2.0 |

A role defines responsibility and authority. A profile defines an optional working posture and does not expand authority. Coder applies to suitable coding tasks; it does not narrow Executor's broader implementation responsibility.

### Intended Surveyor boundary

Surveyor receives an assignment from the Compiler before a product or architectural decision is made. It investigates source material and produces evidence rather than implementing the product. It must not write to Canon or the product repository. It may write research outputs to a designated research-results area and returns a concise handback with precise references to supporting evidence.

The detailed contract, evidence package, coverage rules, and handback schema remain to be specified and tested.

## PO-2 — Repeatable Codex acceptance and key challenge

The Product Owner has approved development of a mechanism for repeatable Codex acceptance in v1.2.0.

Its purpose is to provide a practical alternative to trusting a model's self-report that it previously accepted Codex or rereading the entire Codex on every new assignment. A key challenge can detect some cases where a required fragment is no longer available in the active context. It is not a complete context-loss report, proof of understanding, or authentication mechanism.

The design direction is described in [20 — Repeatable Codex acceptance and key challenge](20-CODEX-ACCEPTANCE-KEY-CHALLENGE.md).

## What these decisions authorize

These two product directions are approved for implementation work in v1.2.0. The exact normative text, schemas, generation and validation tools, tests, and final package still require development and independent review.

These decisions do not promote Candidate material into Canon, change the v1.1.0 release, approve all other ideas in the 1.2.0 backlog, or authorize an unreviewed release.

## Release discipline

Working discussions may contain exploration, rejected options, and intermediate drafts; they are not automatically normative. Before review, the Candidate must be reconciled against direct PO decisions and the exact package must be frozen.
