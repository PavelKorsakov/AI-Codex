# FORMAT-2 — Machine Representation Reference

AI-Codex v1.1.0 defines FORMAT-2 as a generated JSONL/NDJSON machine representation of authored Canon Markdown.

## Authority

Markdown is authored and authoritative.

FORMAT-2 is generated from that Markdown and MUST NOT be independently hand-maintained.

Semantic divergence between the two representations is a package defect.

## Record model

The reference file is:

`format2/index.jsonl`

Each line is one self-contained JSON record with at least:

- `schema`;
- stable `id`;
- `kind`;
- release identity;
- source file + synthetic extraction anchor + source content hash;
- full machine-consumable semantic `text`;
- normative tags where present;
- references/dependencies where extracted.

The reference JSON Schema is:

`schemas/canon-record.schema.json`

### Current v1.1.0 extraction granularity

The shipped generator emits:

- one `document-preamble` record for content before the first H2 heading;
- one complete record per H2 section, including all nested H3+ content until the next H2;
- `kind: "section"` for these generated records.

Other `kind` values in the schema are reserved for compatible future generators/reference tooling; the v1.1.0 release does not claim that its own generated corpus already uses those finer-grained kinds.

This granularity is intentionally coarse but complete: a record contains the complete selected section text rather than a summary or excerpt.

### Record identity and source.anchor

For the shipped v1.1.0 generator:

- preamble anchor: `document-preamble`;
- H2 anchor: lowercase heading text after removing Markdown emphasis/backtick markers, Unicode NFKD normalization, replacement of each run of non-letter/non-digit characters with `-`, trimming leading/trailing `-`, then truncation to 96 characters;
- record ID: lowercase source filename stem + `:` + the synthetic anchor.

`source.anchor` is a **synthetic extraction selector**, not a promise that the same string is a literal Markdown/GitHub URL fragment. Consumers MUST NOT treat it as a browser fragment without applying an explicitly documented renderer-specific conversion.

Generation MUST fail rather than silently emit duplicate record IDs if this normalization would collide.

### links and local section references

The `links` array is a non-exhaustive retrieval hint, not a declaration of complete transitive dependency closure.

Load-bearing local section references remain present in the record `text`. For ordinary machine retrieval, resolve them inside FORMAT-2 as follows:

1. an unqualified `§N` / `§§N...` reference targets the same `source.file`;
2. an explicitly named `file.md §N` reference targets that named source file;
3. select the unique section record for the target file whose H2 section number begins with `N`;
4. if the target is ambiguous or cannot be resolved uniquely, widen retrieval to all FORMAT-2 records for the target source file rather than assuming a rule or silently choosing one.

This resolver is for retrieval convenience. The full source Markdown remains authoritative, but ordinary dependency resolution SHOULD NOT require reopening Markdown merely to recover semantic content already present in FORMAT-2.

## Retrieval

Consumers SHOULD filter before records enter an LLM context.

Useful selectors include:

- record ID;
- source document;
- section;
- record kind;
- normative force;
- reference/dependency.

For the shipped coarse generator, filtering by `kind` alone is not useful because all generated records are `section`; combine selectors such as source file + section/ID/normative force.

Token cost is determined primarily by the selected records entering context, not by total bytes stored in the generated JSONL file.

## Lifecycle

At Candidate freeze:

1. validate Markdown structure required by the generator;
2. generate FORMAT-2;
3. validate JSON records;
4. verify source mapping/hash identity;
5. freeze Markdown + generated FORMAT-2 together.

At PROMOTE, mechanically re-verify the mapping.

Ordinary downstream readers do not compare both representations again unless new evidence indicates corruption or drift.
