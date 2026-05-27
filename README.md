# Phionyx Evaluation Standard

> Vendor-independent proposal for publishing reviewer-runnable governance evidence for agentic AI runtimes.

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20027513.svg)](https://doi.org/10.5281/zenodo.20027513) [![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)

The **Phionyx Evaluation Standard** specifies how an agentic AI runtime can publish governance evidence in a form that is reviewer-reproducible, vendor-neutral, and scoped honestly between runtime mechanism and deployer responsibility. The standard is published in two layers:

- **v0.1.1 (released, citable):** five evaluation dimensions — coherence (Φ), entropy and behavioural stability, ethics and governance, drift and silent failure, system health — together with the L0–L3 evaluation maturity levels and the D0–D3 determinism grades.
- **v0.2 (current draft):** the **Evidence-Oriented Runtime Telemetry Profile** — runtime-scoped coverage labels (`Full / Partial / Gap`), the `assessment_signal` schema requirement, the Compliance-Mapping Row Schema (JSON Schema Draft 2020-12), and worked evidence-row examples.

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

## v0.1.1 — Behavioural reliability framework

v0.1.1 remains the citable public draft and the foundation of v0.2. It defines:

- **Five evaluation dimensions:** coherence (Φ), entropy and behavioural stability, ethics and governance, drift and silent failure, system health.
- **L0–L3 evaluation maturity levels:** L0 (Instrumented Observation) → L1 (Diagnostic) → L2 (Assurance) → L3 (Certification-Oriented Evidence Profile).
- **D0–D3 determinism grades:** D0 (Non-Deterministic) → D1 (Weakly) → D2 (Controlled) → D3 (Fully Deterministic).
- **AI Agent Operating Model:** envelope-based agent outputs, four governance gates, evidence requirements, workflow traceability.

See [`standard/phionyx_Evaluation_Standard_v0.1.md`](standard/phionyx_Evaluation_Standard_v0.1.md) for the full v0.1.1 text.

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
