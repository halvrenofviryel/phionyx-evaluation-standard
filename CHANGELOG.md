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
