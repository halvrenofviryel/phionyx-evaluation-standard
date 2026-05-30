# Phionyx Evaluation Standard

> Vendor-independent proposal for publishing reviewer-runnable governance evidence for agentic AI runtimes.

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20027513.svg)](https://doi.org/10.5281/zenodo.20027513) [![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)

> The DOI badge above is the **concept DOI** (`10.5281/zenodo.20027513`) — it always resolves to the latest archived version (currently v0.2.0). The per-version DOIs are listed in [Citation](#citation).

The **Phionyx Evaluation Standard** specifies how an agentic AI runtime can publish governance evidence in a form that is reviewer-reproducible, vendor-neutral, and scoped honestly between runtime mechanism and deployer responsibility.

**Where this sits.** This repository is the **Standard** — a vendor-neutral specification. It does not ship a runtime. It defines the scales that any runtime can be measured against:

- **L0–L3** — evaluation maturity (released, v0.1.1 / v0.2.0).
- **D0–D3** — determinism grades (released, v0.1.1 / v0.2.0).
- **CG-L0…CG-L5** — claim-governance maturity (the **v0.3 layer, currently a DRAFT** — no v0.3 tag/release).

The CG-L0…CG-L5 ladder rates the **self-governance gate** `phionyx-pipeline-mcp` (an MCP server, separate package), not the engine. The deterministic engine `phionyx-core` (the SDK) is the **reference implementation** that scores **L3 + D3** on the released axes; it is not itself claim-governance-rated. See [Companion artefacts](#companion-artefacts).

The standard is published in layers:

- **v0.1.1 (released, citable):** five evaluation dimensions — coherence (Φ), entropy and behavioural stability, ethics and governance, drift and silent failure, system health — together with the L0–L3 evaluation maturity levels and the D0–D3 determinism grades.
- **v0.2.0 (released, citable):** the **Evidence-Oriented Runtime Telemetry Profile** — runtime-scoped coverage labels (`Full / Partial / Gap`), the `assessment_signal` schema requirement, the Compliance-Mapping Row Schema (JSON Schema Draft 2020-12), and worked evidence-row examples.
- **v0.3 (draft, unreleased):** the **Noisy-Measurement Claim Governance Profile** — the `CG-L0…CG-L5` claim-governance maturity ladder, the `require_tool` evidence-binding directive, continuity binding, and a detector-calibration profile. A distinct fourth axis from the L0–L3 maturity and D0–D3 determinism ladders. No v0.3 tag or Zenodo deposit yet.

> This repository does not certify AI systems. It defines evidence formats and evaluation profiles that can support external review, assurance, and standards alignment.

---

## v0.2.0 — Evidence-Oriented Runtime Telemetry Profile

The v0.2.0 supplement (released, tagged 2026-05-24) adds one orthogonal layer to v0.1.1. The principal new requirement is that any **Runtime-Full coverage** claim must declare an `assessment_signal` — the specific channel, metric, integrity check, or composite the row's coverage claim is interpreted against. A Runtime-Full row without `assessment_signal` is a schema violation.

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

A draft layer (unreleased — no v0.3 tag) that profiles how a runtime governs the boundary between a model's noisy output and an operational fact. It treats a model output as a **noisy measurement**, defines the claim lifecycle and directive set, and adds a **claim-governance maturity ladder, `CG-L0 … CG-L5`** — a fourth axis, deliberately prefixed `CG-` so it never collides with the v0.1 evaluation `L0–L3` or determinism `D0–D3` ladders.

| Level | Name | Meaning (short) |
| --- | --- | --- |
| CG-L0 | Ungoverned | Output used directly as truth. |
| CG-L1 | Recorded | Decisions/outputs in a signed, replayable record. |
| CG-L2 | Governed claims | Claims mapped + checked + directive emitted + signed record. |
| CG-L3 | Evidence-binding required | `require_tool` + continuity binding (read-but-not-bound). |
| CG-L4 | Calibrated governance | Detectors measured as sensors (e.g. ECE). |
| CG-L5 | Causal / live-deterministic | Causal directive choice + live re-execution. |

**Reference-implementation position (honest).** The CG ladder rates the **gate** `phionyx-pipeline-mcp`, not the engine. That gate has **reached CG-L3** in its `v0.3.0a1` **alpha pre-release** — `require_tool` + continuity binding shipped, enforcement **opt-in / default-off**, so the **stable channel (`v0.2.0`) remains CG-L2**. **CG-L4 is in progress** (calibration machinery shipped; ECE not yet validated). No CG-L4-reached or CG-L5 claim.

See [`standard/phionyx_Evaluation_Standard_v0.3-draft.md`](standard/phionyx_Evaluation_Standard_v0.3-draft.md) for the full profile. No Zenodo DOI is assigned to this draft layer.

---

## v0.1.1 — Behavioural reliability framework

v0.1.1 remains a citable public release and the foundation of v0.2.0. It defines:

- **Five evaluation dimensions:** coherence (Φ), entropy and behavioural stability, ethics and governance, drift and silent failure, system health.
- **L0–L3 evaluation maturity levels:** L0 (Instrumented Observation) → L1 (Diagnostic) → L2 (Assurance) → L3 (Certification-Oriented Evidence Profile).
- **D0–D3 determinism grades:** D0 (Non-Deterministic) → D1 (Weakly) → D2 (Controlled) → D3 (Fully Deterministic).
- **AI Agent Operating Model:** envelope-based agent outputs, four governance gates, evidence requirements, workflow traceability.

See [`standard/phionyx_Evaluation_Standard_v0.1.md`](standard/phionyx_Evaluation_Standard_v0.1.md) for the full v0.1.1 text.

A complete worked example — a single L2 evaluation report for a fictional system (CONDITIONAL outcome) — is at [`examples/sample-evaluation-report.md`](examples/sample-evaluation-report.md).

---

## Citation

Two DOIs serve different purposes:

- **Concept DOI (version-agnostic, always latest):** `10.5281/zenodo.20027513` — this is what the badge points to.
- **Versioned DOIs:** `10.5281/zenodo.20027514` (v0.1.1) · `10.5281/zenodo.20368274` (v0.2.0).

For the latest released version (v0.2.0):

```
Abak, A. T. (2026). Phionyx Evaluation Standard (v0.2.0). Zenodo.
https://doi.org/10.5281/zenodo.20368274
```

For the v0.1.1 release specifically:

```
Abak, A. T. (2026). Phionyx Evaluation Standard (v0.1.1). Zenodo.
https://doi.org/10.5281/zenodo.20027514
```

To cite the standard independent of version, use the concept DOI `10.5281/zenodo.20027513`. `CITATION.cff` tracks the latest released version.

---

## Companion artefacts

- **`phionyx-core` (the SDK / engine), v0.7.2** — the deterministic reference runtime; scores **L3 + D3** on this Standard's released axes. [`halvrenofviryel/phionyx-research`](https://github.com/halvrenofviryel/phionyx-research). Zenodo: [`10.5281/zenodo.20027534`](https://doi.org/10.5281/zenodo.20027534).
- **`phionyx-pipeline-mcp` (the gate)** — the self-governance MCP server the **CG-L0…CG-L5** ladder rates: stable `v0.2.0` = **CG-L2**; alpha `v0.3.0a1` = **CG-L3** (opt-in / default-off). [`halvrenofviryel/phionyx-pipeline-mcp`](https://github.com/halvrenofviryel/phionyx-pipeline-mcp).
- **arXiv Paper 1** (Abak 2026) — Phionyx runtime architecture (currently in moderation). ID added on announcement.

---

## License

[CC BY-SA 4.0](LICENSE).
