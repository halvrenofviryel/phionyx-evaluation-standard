# Phionyx Evaluation Standard

> Vendor-independent proposal for publishing reviewer-runnable governance evidence for agentic AI runtimes.

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20027513.svg)](https://doi.org/10.5281/zenodo.20027513) [![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)

The **Phionyx Evaluation Standard** specifies how an agentic AI runtime can publish governance evidence in a form that is reviewer-reproducible, vendor-neutral, and scoped honestly between runtime mechanism and deployer responsibility. The standard is published in layers:

- **v0.1.1 (released, citable):** five evaluation dimensions — coherence (Φ), entropy and behavioural stability, ethics and governance, drift and silent failure, system health — together with the L0–L3 evaluation maturity levels and the D0–D3 determinism grades.
- **v0.2 (current draft):** the **Evidence-Oriented Runtime Telemetry Profile** — runtime-scoped coverage labels (`Full / Partial / Gap`), the `assessment_signal` schema requirement, the Compliance-Mapping Row Schema (JSON Schema Draft 2020-12), and worked evidence-row examples.
- **v0.3-draft (claim-governance layer):** the **Noisy-Measurement Claim Governance Profile** — the `CG-L0…CG-L5` claim-governance maturity ladder, the `require_tool` evidence-binding directive, continuity binding, and a detector-calibration profile. A distinct fourth axis from the L0–L3 maturity and D0–D3 determinism ladders.

> This repository does not certify AI systems. It defines evidence formats and evaluation profiles that can support external review, assurance, and standards alignment.

---

## v0.2.0-draft — Evidence-Oriented Runtime Telemetry Profile

The v0.2 supplement adds one orthogonal layer to v0.1.1. The principal new requirement is that any **Runtime-Full coverage** claim must declare an `assessment_signal` — the specific channel, metric, integrity check, or composite the row's coverage claim is interpreted against. A Runtime-Full row without `assessment_signal` is a schema violation.

| Artefact | Path |
| --- | --- |
| v0.2 supplement document | [`standard/phionyx_Evaluation_Standard_v0.2.md`](standard/phionyx_Evaluation_Standard_v0.2.md) |
| Compliance-Mapping Row Schema | [`schemas/evidence/compliance_mapping_row.schema.json`](schemas/evidence/compliance_mapping_row.schema.json) |
| Worked Full row | [`examples/evidence/eu_ai_act_article_12_full_row.json`](examples/evidence/eu_ai_act_article_12_full_row.json) |
| Worked Partial row | [`examples/evidence/eu_ai_act_article_14_partial_row.json`](examples/evidence/eu_ai_act_article_14_partial_row.json) |
| Worked Gap row | [`examples/evidence/eu_ai_act_article_10_gap_row.json`](examples/evidence/eu_ai_act_article_10_gap_row.json) |

The supplement is self-contained: it defines the row format, the schema, and the assessment-signal requirement directly. Worked rows demonstrate Runtime-Full, Runtime-Partial, and Runtime-Gap cases mapped onto EU AI Act articles.

### Quick check — validate an evidence row

```bash
pip install jsonschema
python3 -c "
import json, jsonschema
schema = json.load(open('schemas/evidence/compliance_mapping_row.schema.json'))
row    = json.load(open('examples/evidence/eu_ai_act_article_12_full_row.json'))
jsonschema.validate(row, schema)
print('OK')
"
```

A Runtime-Full row with `assessment_signal` removed will fail validation.

---

## v0.3-draft — Noisy-Measurement Claim Governance Profile

A draft layer that profiles how a runtime governs the boundary between a model's noisy output and an operational fact. It treats a model output as a **noisy measurement**, defines the claim lifecycle and directive set, and adds a **claim-governance maturity ladder, `CG-L0 … CG-L5`** — a fourth axis, deliberately prefixed `CG-` so it never collides with the v0.1 evaluation `L0–L3` or determinism `D0–D3` ladders.

| Level | Name | Meaning (short) |
| --- | --- | --- |
| CG-L0 | Ungoverned | Output used directly as truth. |
| CG-L1 | Recorded | Decisions/outputs in a signed, replayable record. |
| CG-L2 | Governed claims | Claims mapped + checked + directive emitted + signed record. |
| CG-L3 | Evidence-binding required | `require_tool` + continuity binding (read-but-not-bound). |
| CG-L4 | Calibrated governance | Detectors measured as sensors (e.g. ECE). |
| CG-L5 | Causal / live-deterministic | Causal directive choice + live re-execution. |

**Reference-implementation position (honest):** the reference runtime (Phionyx) has **reached CG-L3** in the `phionyx-pipeline-mcp` `v0.3.0a1` **alpha pre-release** — `require_tool` + continuity binding shipped, enforcement **opt-in / default-off**, so the stable channel remains **CG-L2**. **CG-L4 is in progress** (calibration machinery shipped; ECE not yet validated). No CG-L4-reached or CG-L5 claim.

See [`standard/phionyx_Evaluation_Standard_v0.3-draft.md`](standard/phionyx_Evaluation_Standard_v0.3-draft.md) for the full profile. No Zenodo DOI is assigned to this draft layer.

---

## v0.1.1 — Behavioural reliability framework

v0.1.1 remains the citable public draft and the foundation of v0.2. It defines:

- **Five evaluation dimensions:** coherence (Φ), entropy and behavioural stability, ethics and governance, drift and silent failure, system health.
- **L0–L3 evaluation maturity levels:** L0 (Instrumented Observation) → L1 (Diagnostic) → L2 (Assurance) → L3 (Certification-Oriented Evidence Profile).
- **D0–D3 determinism grades:** D0 (Non-Deterministic) → D1 (Weakly) → D2 (Controlled) → D3 (Fully Deterministic).
- **AI Agent Operating Model:** envelope-based agent outputs, four governance gates, evidence requirements, workflow traceability.

See [`standard/phionyx_Evaluation_Standard_v0.1.md`](standard/phionyx_Evaluation_Standard_v0.1.md) for the full v0.1.1 text.

A complete worked example — a single L2 evaluation report for a fictional system (CONDITIONAL outcome) — is at [`examples/sample-evaluation-report.md`](examples/sample-evaluation-report.md).

---

## Citation

For the released v0.1.1:

```
Abak, A. T. (2026). Phionyx Evaluation Standard (v0.1.1). Zenodo.
https://doi.org/10.5281/zenodo.20027514
```

The v0.2 supplement is currently a **draft branch**. Cite v0.1.1 for the canonical reference; reference the v0.2-draft branch only for the Evidence-Oriented Runtime Telemetry Profile material.

A new versioned Zenodo deposit will be created at v0.2.0 release; `CITATION.cff` is updated at that point.

---

## Companion artefacts

- **Phionyx Core SDK v0.7.0** — reference runtime implementation. [`halvrenofviryel/phionyx-research`](https://github.com/halvrenofviryel/phionyx-research). Zenodo: [`10.5281/zenodo.20027534`](https://doi.org/10.5281/zenodo.20027534).
- **arXiv Paper 1** (Abak 2026) — Phionyx runtime architecture (currently in moderation). ID added on announcement.

---

## License

[CC BY-SA 4.0](LICENSE).
