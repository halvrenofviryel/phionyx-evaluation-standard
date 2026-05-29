## [v0.3-draft] — 2026-05-29

The **Noisy-Measurement Claim Governance Profile** — a draft layer profiling how a runtime governs the noisy-output -> operational-fact boundary. Does not replace v0.1.1 or v0.2.

### Added
- **Claim-Governance maturity ladder `CG-L0..CG-L5`** at `standard/phionyx_Evaluation_Standard_v0.3-draft.md` — a fourth axis (`CG-` prefixed to avoid colliding with evaluation `L0-L3` and determinism `D0-D3`).
- **Directive set** including `require_tool` (evidence-binding) and the **continuity-binding** and **detector-calibration** profiles.
- **Worked L2 evaluation report** at `examples/sample-evaluation-report.md` — a complete report for a fictional system with a CONDITIONAL outcome (illustrative numbers).
- README section documenting the v0.3-draft layer.

### Reference-implementation position (honest)
- CG-L3 **reached** in `phionyx-pipeline-mcp` `v0.3.0a1` alpha pre-release (`require_tool` + continuity binding shipped; enforcement opt-in / default-off; stable channel remains CG-L2).
- CG-L4 **in progress** (calibration machinery shipped; ECE not yet validated). No CG-L4-reached or CG-L5 claim.

### Note
- No Zenodo DOI assigned to this draft layer; v0.1.1 (DOI 10.5281/zenodo.20027514) remains the canonical citation.

## [v0.2.0] — 2026-05-24

The **Evidence-Oriented Runtime Telemetry Profile** — a self-contained supplement to v0.1.1. The v0.1.1 standard document and Zenodo deposit (DOI 10.5281/zenodo.20027514) remain unchanged and citable.

### Added

- **Evidence-Oriented Runtime Telemetry Profile** as a supplement document at `standard/phionyx_Evaluation_Standard_v0.2.md`, defining the row format that agentic-AI runtimes use to publish reviewer-runnable evidence.
- **Runtime-scoped coverage labels** — `Runtime-Full / Runtime-Partial / Runtime-Gap` — distinct from the v0.1 L0–L3 maturity and D0–D3 determinism axes.
- **`assessment_signal` requirement** — schema-enforced on Runtime-Full rows via JSON Schema `allOf` `if/then` (Draft 2020-12). A Full row without `assessment_signal` is a schema violation.
- **Compliance-Mapping Row Schema (v0.1)** at `schemas/evidence/compliance_mapping_row.schema.json`. Implementation-neutral, vendor-neutral.
- **Three worked evidence-row examples** at `examples/evidence/`:
  - `eu_ai_act_article_12_full_row.json` (Runtime-Full, `assessment_signal: governance_envelope.integrity.canonical_json_hash_chain`).
  - `eu_ai_act_article_14_partial_row.json` (Runtime-Partial, no `assessment_signal`).
  - `eu_ai_act_article_10_gap_row.json` (Runtime-Gap, empty mechanisms/evidence).
- **Disambiguation table** in §3 of the supplement separating D0–D3, L0–L3, Full/Partial/Gap, and `assessment_signal` axes.

### Clarified

- **Composite Quality Score (CQS)** is positioned as a summary indicator, not a default `assessment_signal`. Any governance claim based on CQS MUST also declare the underlying `assessment_signal`(s). Composite-only scoring SHOULD NOT be used for trajectory-failure claims where channel masking is possible.
- **No certification claim.** The supplement explicitly states the standard does not certify AI systems; it specifies an evidence format that can support external review, assurance, and standards alignment.

### Unchanged

- v0.1.1 standard document (`standard/phionyx_Evaluation_Standard_v0.1.md`) and its Zenodo deposit (DOI 10.5281/zenodo.20027514) remain citable and unmodified.

### Deferred (planned for v0.2.1)

- Schema test suite at `tests/schemas/test_compliance_mapping_row_schema.py`.
- Standalone assessment-signal registry at `docs/assessment_signals.md`.
- Governed Response Envelope JSON Schema at `schemas/evidence/governed_response_envelope.v0_1.schema.json`.
- Light-touch wording-consistency pass on v0.1 (residual "certification" phrasing) — published as a v0.2 supplement note rather than as edits to the Zenodo-frozen v0.1.1 file.


# Changelog

## v0.1.1 (2026-05-04)

First Zenodo-archived release. The previous v0.1.0 release was tagged
before the repo was connected to Zenodo, so this minor bump exists
specifically so the standard receives a persistent academic identifier.

### Documentation

- L3 evaluation level renamed from "Certification-Grade Evaluation" to
  "Certification-Oriented Evidence Profile" throughout
  `standard/phionyx_Evaluation_Standard_v0.1.md`. The standard does
  not certify systems; it produces evidence suitable for certification
  under external schemes (NIST AI RMF, ISO/IEC 42001, EU AI Act). The
  rename makes that distinction explicit.
- Reference-implementation paragraph: "Implements D3 ... L3" →
  "Targets D3 ... under controlled reproducibility tests; independent
  third-party validation pending."
- Naming-note callout under the L3 section explaining the rename and
  v0.1 backward-compatibility scope.

### Added

- `examples/sample-evaluation-report.md` — fictional reference report
  showing the structure of an evaluation written against the standard.
- `.zenodo.json` — full Zenodo metadata block (title, description,
  creators, license, keywords, related identifiers).

### Changed

- `CITATION.cff` — `type: dataset` set, email + extra keywords added,
  commented `identifiers:` slot prepared for the DOI Zenodo will mint
  on this release.

No technical changes to the evaluation methodology itself.

## v0.1.0 (2026-04-23)

- Initial public release
- 5 evaluation dimensions
- Determinism grading (D0–D3)
- Evaluation levels (L0–L3)
- AI Agent Operating Model with 4-gate architecture
