# Phionyx Evaluation Standard — v0.2 Supplement

**Title:** Evidence-Oriented Runtime Telemetry Profile
**Version:** 0.2.0-draft (unreleased)
**Status:** Public draft, supplement to v0.1.1
**Author:** Ali Toygar Abak (Phionyx Research)
**License:** CC-BY-SA-4.0
**Predecessor:** [`phionyx_Evaluation_Standard_v0.1.md`](./phionyx_Evaluation_Standard_v0.1.md)

---

## Foreword

This document is a **supplement** to v0.1.1, not a replacement. Everything in v0.1.1 (the five evaluation dimensions, the L0–L3 evaluation maturity levels, the D0–D3 determinism grades, the AI Agent Operating Model, and the existing reference implementation) remains in force and citable; the v0.1.1 Zenodo deposit (DOI [`10.5281/zenodo.20027514`](https://doi.org/10.5281/zenodo.20027514)) is unchanged.

v0.2 adds one orthogonal layer: an **Evidence-Oriented Runtime Telemetry Profile**. The profile specifies how runtime evidence rows that map a runtime's mechanisms onto external governance frameworks (e.g. EU AI Act, NIST AI RMF, OWASP Agentic AI Threats, ISO/IEC 42001) should be published, scoped, and assessed. The profile is published as JSON Schema (Draft 2020-12) at [`schemas/evidence/compliance_mapping_row.schema.json`](../schemas/evidence/compliance_mapping_row.schema.json) and is grounded in arXiv Paper 2 (Abak 2026): *"Evidence-Oriented Runtime Telemetry for Agentic AI Governance: Trajectory Failures, Assessment Signals, and Reviewer-Runnable Evidence"*.

The v0.2 profile is **vendor-neutral**. It does not assume the Phionyx Core runtime; it specifies a row format that any agentic-AI runtime can adopt to publish governance-evidence rows in a reviewer-reproducible shape.

This supplement does not certify AI systems. It defines an evidence format that can support external review, assurance, and standards alignment.

---

## 1. Runtime-Scoped Coverage Labels

The v0.2 profile introduces a coverage-label vocabulary distinct from the v0.1 maturity (L0–L3) and determinism (D0–D3) axes.

A coverage label is **runtime-scoped** when it describes the runtime's technical contribution to a framework clause, conditional on the deployer programme stated in the row, and explicitly does not assert satisfaction of the legal or regulatory obligation as a whole. Coverage labels are drawn from `Full / Partial / Gap`, read as `Runtime-Full / Runtime-Partial / Runtime-Gap`:

- **`Full` (Runtime-Full).** The runtime supplies a complete technical mechanism for the runtime-scoped portion of the framework clause, under the assumptions and deployer programme stated in the row.
- **`Partial` (Runtime-Partial).** The runtime supplies part of the technical mechanism; the framework clause includes obligations the runtime alone cannot fully realise.
- **`Gap` (Runtime-Gap).** The runtime does not supply this mechanism, by design or due to scope. Gap rows are first-class evidence — they make explicit which obligations the deployer must source from elsewhere.

No coverage label, including `Full`, asserts legal, regulatory, or third-party conformance. Every row carries a non-empty `deployer_responsibility` field stating what the deploying organisation must supply.

A worked Full row appears in [`examples/evidence/eu_ai_act_article_12_full_row.json`](../examples/evidence/eu_ai_act_article_12_full_row.json); a Partial row in [`eu_ai_act_article_14_partial_row.json`](../examples/evidence/eu_ai_act_article_14_partial_row.json); a Gap row in [`eu_ai_act_article_10_gap_row.json`](../examples/evidence/eu_ai_act_article_10_gap_row.json).

---

## 2. Assessment Signal Requirement

This is the principal new requirement of v0.2.

> **Definition (Assessment signal).** An *assessment signal* is the specific channel, metric, or composite a row's coverage claim is interpreted against.

> **Requirement.** A coverage row that does not declare its `assessment_signal` cannot claim Runtime-Full coverage; the schema rejects such rows. The field is required on Full rows; permitted to be omitted on Partial and Gap rows.

The schema enforces the requirement with a conditional `allOf` rule (Draft 2020-12 `if/then`):

```json
{
  "if":   { "properties": { "coverage": { "const": "Full" } } },
  "then": { "required": ["assessment_signal"] }
}
```

**Why this is at the schema level.** Coherence summaries, trust scores, and composite quality scores are typically reported as scalars derived from multiple channels. A coverage claim assessed against a composite scalar can be misleading when channel masking is possible — the composite stays roughly flat while one channel falls and another rises. Forcing the row to declare its `assessment_signal` makes the choice of interpretation visible and disagreeable: a reviewer who believes a different signal should be used can publish a counter-row with a different `assessment_signal` in the same schema, and the disagreement enters the public record as structurally as the original claim.

### 2.1 Assessment-signal forms

The `assessment_signal` value is intentionally not constrained to a fixed enum; valid forms are runtime-shaped. The five trajectory-failure fixtures referenced in Paper 2 §6.5 use four distinct assessment signals:

| Fixture | Failure mode | Assessment signal |
| --- | --- | --- |
| NPC drift | Persona / role drift | `phi_cognitive` |
| Tool loop | Recursive tool-loop | `phi_cognitive` + `similarity_to_prev` extension |
| Scope creep | Capability scope-creep | `capability_budget` extension |
| Agent drift | Retry-loop + hallucinated completion | `phi_cognitive` + `claim_state_consistency` extension |
| Audit discontinuity | Audit-chain gap on record deletion | `governance_envelope.integrity.canonical_json_hash_chain` |

Five fixtures, four distinct signals, one shared row schema. This distribution is the empirical motivation for placing the assessment-signal choice on the row schema rather than in runtime documentation: different failure modes require different assessment signals, so the choice belongs at the row level where reviewers can see and contest it.

### 2.2 What the schema validates and what it does not

The schema validates that `assessment_signal` is **declared and non-empty** on Full rows. It does **not** validate that the declared signal is the correct one for the framework clause. That judgement remains with the row author and any disagreeing reviewer. The protocol's contribution is making the choice visible, not adjudicating it.

---

## 3. Disambiguation: Coverage vs Maturity vs Determinism

v0.2 adds a coverage axis alongside the v0.1 maturity and determinism axes. They measure different things and are not interchangeable. The following table is canonical:

| Concept | What it measures | Where it is used | Introduced in |
| --- | --- | --- | --- |
| **D0–D3** | Determinism grade of the system control path | System characterisation | v0.1 |
| **L0–L3** | Evaluation maturity / evidence readiness | Programme-level reporting | v0.1 |
| **`Full / Partial / Gap`** | Runtime-scoped coverage of a single framework clause | Standards-mapping rows | v0.2 |
| **`assessment_signal`** | Signal a Full claim is interpreted against | Full rows in standards-mapping | v0.2 |

A v0.1 system at L3 with grade D3 may nevertheless publish a `Gap` row against EU AI Act Article 10 — and should, because Article 10 is not addressable by a runtime. Conversely, a `Full` row says nothing about the runtime's overall maturity or determinism; it is scoped to one clause under one set of assumptions.

The four axes compose; they do not overlap.

---

## 4. Compliance-Mapping Row Schema

The Compliance-Mapping Row Schema is published at [`schemas/evidence/compliance_mapping_row.schema.json`](../schemas/evidence/compliance_mapping_row.schema.json) under JSON Schema Draft 2020-12. The schema's `$id` is `https://phionyx.ai/schemas/evidence/compliance_mapping_row.schema.json`.

### 4.1 Required fields

- `framework`, `clause` — the row's mapping target.
- `coverage` — the `Full / Partial / Gap` enum (§1; runtime-scoped).
- `coverage_scope` — free-text qualifier explicitly stating the scope of the coverage label.
- `mechanisms` — runtime mechanisms (file paths, function names, contract identifiers, schema URIs) that contribute. Permitted to be empty only on `Gap` rows.
- `evidence` — reviewer-reproducible artefact identifiers, typically commit-pinned URLs. Permitted to be empty only on `Gap` rows.
- `deployer_responsibility` — non-empty string describing what the deployer must supply (required regardless of coverage).
- `assessment_signal` — **required when `coverage == "Full"`**; optional otherwise. Detailed in §2.
- `last_verified` — ISO date.
- `assumptions` — array of conditions under which the row applies.
- `authored_by` — identifier of the row's author so reviewer disagreements can be attributed precisely.

### 4.2 Optional fields

- `extensions` — controlled-extensibility object. The top-level row schema sets `additionalProperties: false`, so anonymous fields are rejected; runtime-specific or framework-specific metadata lives inside `extensions`.

### 4.3 Conditional constraints

Two `allOf` rules apply:

1. When `coverage` is `Full` or `Partial`, `mechanisms` and `evidence` must each contain at least one item.
2. When `coverage` is `Full`, `assessment_signal` is required.

A schema-conformant validator (e.g., `python -m jsonschema`, `ajv`) will reject any row that violates these rules. The reference validator [`validate_evidence_row.py`](https://github.com/halvrenofviryel/phionyx-research/blob/c8fa1f9/docs/strategic/launch_drafts/standalone_evidence_row/validate_evidence_row.py) ships with the Phionyx public repository.

---

## 5. Composite Quality Score — clarification under v0.2

v0.1.1 introduces a Composite Quality Score (CQS) as a summary indicator of multi-dimensional behavioural quality. v0.2 clarifies its role under the Evidence-Oriented Runtime Telemetry Profile:

> **Composite Quality Score is a summary indicator, not an assessment signal by default.** Any governance claim based on CQS MUST also declare the underlying `assessment_signal`(s) the claim is actually being assessed against. Composite-only scoring SHOULD NOT be used for trajectory-failure claims where channel masking is possible.

The structural argument is documented in Paper 2 §6.4 (NPC coherence-drift trace): on a four-turn drift trajectory, the cognitive channel falls cleanly from 0.59 to 0.09 while the physical channel rises with arousal from 1.38 to 2.88; the composite scalar stays roughly flat in `[0.69, 0.79]`. A protocol that classified on the composite would not flag this trajectory as drift. A `Full` row that declared its `assessment_signal` as `phi_total` for this clause would be wrong by construction; a row that declared `phi_cognitive` would be correct.

This clarification does not deprecate CQS. It scopes its use: CQS remains a useful programme-level summary; it is not a substitute for the per-row assessment-signal declaration.

---

## 6. Cross-References

- **arXiv Paper 2** (Abak 2026), *"Evidence-Oriented Runtime Telemetry for Agentic AI Governance: Trajectory Failures, Assessment Signals, and Reviewer-Runnable Evidence"* — primary technical exposition. The arXiv identifier is added to this document at the v0.2.0 release.
- **arXiv Paper 1** (Abak 2026), companion architecture paper. The arXiv identifier is added to this document at the v0.2.0 release.
- **Phionyx Core SDK v0.3.0** — reference implementation. Zenodo concept DOI: [`10.5281/zenodo.20027534`](https://doi.org/10.5281/zenodo.20027534); v0.3.0 versioned DOI: [`10.5281/zenodo.20027535`](https://doi.org/10.5281/zenodo.20027535). Public repository: [`halvrenofviryel/phionyx-research`](https://github.com/halvrenofviryel/phionyx-research).
- **Phionyx Evaluation Standard v0.1.1** — predecessor document. Zenodo concept DOI: [`10.5281/zenodo.20027513`](https://doi.org/10.5281/zenodo.20027513); v0.1.1 versioned DOI: [`10.5281/zenodo.20027514`](https://doi.org/10.5281/zenodo.20027514).

---

## 7. What is not in v0.2.0-draft (deferred)

The following items are planned for the v0.2.0 release window or later releases. They are **not** part of this draft:

- A schema test suite (`tests/schemas/test_compliance_mapping_row_schema.py`).
- A Governed Response Envelope JSON Schema (`schemas/evidence/governed_response_envelope.v0_1.schema.json`).
- A standalone assessment-signal registry (`docs/assessment_signals.md`).
- A pass over v0.1.1 to retire residual "certification" wording (v0.1.1 is Zenodo-frozen; the supplement defers a wording-cleanup release to v0.2.0 final).
- Updates to the v0.1 AI Agent Operating Model to thread `assessment_signal`, `coverage_scope`, and `deployer_responsibility` through agent output evidence.

These are tracked for the v0.2.0 release; this document will be re-issued at release with the deferred items addressed.

---

## 8. Versioning and citation

This is a draft. Until v0.2.0 release:

- Cite **v0.1.1** for canonical citation: Abak, A. T. *Phionyx Evaluation Standard*, v0.1.1, Zenodo, 2026. DOI: [`10.5281/zenodo.20027514`](https://doi.org/10.5281/zenodo.20027514).
- Reference the v0.2.0-draft branch only for the Evidence-Oriented Runtime Telemetry Profile material introduced here.

`CITATION.cff` is **not** bumped at v0.2.0-draft. It will be bumped at v0.2.0 release with a new versioned Zenodo deposit.
