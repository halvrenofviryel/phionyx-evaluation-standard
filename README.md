# Phionyx Evaluation Standard

**Vendor-independent framework for measuring AI system behavioral reliability — not just accuracy.**

[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
[![Version](https://img.shields.io/badge/version-0.1-blue.svg)](standard/)

Most AI evaluation focuses on benchmark scores. This standard measures what happens *after* deployment: does the system behave consistently? Does it drift? Does it fail silently? Can you prove it?

## What This Defines

Five evaluation dimensions for runtime behavioral assurance:

| Dimension | What It Measures |
|-----------|-----------------|
| **Coherence (Phi)** | Cognitive resonance and response quality over time |
| **Entropy & Stability** | Behavioral consistency under varied inputs |
| **Ethics & Governance** | Safety gate enforcement and ethical reasoning |
| **Drift & Silent Failure** | Behavioral degradation detection |
| **System Health** | Operational metrics and resource efficiency |

## Key Concepts

- **Determinism Grading (D0-D3):** From non-deterministic to fully deterministic
- **Evaluation Levels (L0-L3):** From unmeasured to governance-grade
- **AI Agent Operating Model:** 4-gate architecture (Outbound, Merge, Release, Data)
- **Composite Quality Score (CQS):** Single metric for multi-dimensional behavioral quality

## How It Relates to Existing Frameworks

The standard complements — not replaces — existing frameworks:

| Framework | Focus | This Standard Adds |
|-----------|-------|-------------------|
| NIST AI RMF | Risk management process | Concrete runtime metrics |
| ISO/IEC 42001 | Management system | Behavioral measurement criteria |
| EU AI Act | Compliance requirements | Technical evaluation methods |

## Documents

| Document | Description |
|----------|-------------|
| [Phionyx Evaluation Standard v0.1](standard/phionyx_Evaluation_Standard_v0.1.md) | Full specification |
| [Sample Evaluation Report](examples/sample-evaluation-report.md) | Worked L2 report on a fictional system — illustrates every output in §8 |

## Sample Report

A complete L2 evaluation report on a fictional customer-support agent
(`Helios v2.4`) shows what the standard produces in practice: a
Behavioral Profile, Risk & Drift Report, Ethics Compliance Report,
Stability & Health Summary, Determinism Grade, and Certification
Readiness Status, with a worked CONDITIONAL outcome and concrete
mitigations.

→ [`examples/sample-evaluation-report.md`](examples/sample-evaluation-report.md)

The numbers are illustrative; the structure is exactly what a real
evaluation produces.

## Reference Implementation

The [Phionyx Core SDK](https://github.com/halvrenofviryel/phionyx-research) (AGPL-3.0) provides a reference implementation — 2,571 tests, 46-block pipeline, CQS 0.862.

## Citation

```bibtex
@techreport{abak2026phionyx_standard,
  author      = {Abak, Ali Toygar},
  title       = {Phionyx Evaluation Standard v0.1},
  institution = {Phionyx Research},
  year        = {2026},
  url         = {https://github.com/halvrenofviryel/phionyx-evaluation-standard},
}
```

A machine-readable [`CITATION.cff`](CITATION.cff) is provided for GitHub's “Cite this repository” widget. A persistent Zenodo DOI for archived releases is planned — once issued it will be added here as a badge and to `CITATION.cff` under `identifiers:`.

## Contributing

Feedback, extensions, and translations are welcome. Open an issue or submit a PR.

## License

[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) — free to share and adapt with attribution.

**Author:** Ali Toygar Abak ([Phionyx Research](https://phionyx.ai))
