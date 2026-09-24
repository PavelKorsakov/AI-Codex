# AI-Codex v1.1.0

AI-Codex is an agent-agnostic operating standard for collaborative software development involving a human Product Owner and one or more AI participants.

v1.1.0 adds explicit Canonical Revisions, a promoted-only Canon namespace, deterministic lifecycle-evidence discovery, generated JSONL/NDJSON FORMAT-2, closed open-loop contracts for TASK / Review Assignment / Service Memo, bootstrap/preflight rules, and optional Role Context Profiles.

Start with [index.md](index.md).

Markdown is the authored authoritative representation. Machine consumers may use [format2/index.jsonl](format2/index.jsonl), generated from the same frozen semantic state.

Authoritative lifecycle status is external to the immutable reviewed payload and is resolved through the stable Promotion Record locator declared in `index.md`.
