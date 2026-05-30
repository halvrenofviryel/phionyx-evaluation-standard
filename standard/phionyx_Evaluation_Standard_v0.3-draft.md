# Noisy-Measurement Claim Governance Profile

**Phionyx Open Evaluation Profile — v0.3-draft layer**

> **Status: DRAFT.** This is a new draft layer (v0.3-draft) added on top of the
> existing profile. It **does not replace** v0.1.1 (citable baseline) or v0.2
> (Evidence-Oriented Runtime Telemetry); those remain untouched.
>
> **No Zenodo DOI** is assigned to this draft layer. A DOI is minted only when
> the layer stabilizes.
>
> This repository does **not** certify AI systems. This profile defines a model
> for evaluating how a runtime governs noisy model outputs; it is not a
> certification.

## Neutrality & Conflict of Interest

This profile is authored by the Phionyx project, which also maintains the
reference implementation of a conforming runtime. To keep that dual role
honest:

- The criteria in this profile are defined to be implementable by **any**
  runtime, independent of Phionyx. Conformance is defined by this profile's
  normative text, not by any particular implementation.
- The reference implementation is **informative, not normative**. Where this
  document describes what the Phionyx runtime ships, that description illustrates
  the criteria; it does not define them.
- Any Phionyx grades or maturity levels reported in this document are
  **self-assessed by the author**. Independent third-party validation is
  **pending**.

## Purpose

This profile defines, vendor-independently, how to evaluate the governance of
**noisy model output as measurement**: the boundary between a language model's
output and an operational fact. It specifies the claim lifecycle, the directive
set, a maturity ladder, a detector-calibration profile, and replay terminology.

The model is: a language model output is a **noisy measurement** (a sensor
reading), not truth. The profile evaluates how a runtime governs the transition
from such a reading to action.

## 1. The claim lifecycle (standard form)

```
model output
   -> candidate claim
      -> measurement mapping
         -> knowledge boundary
            -> confidence / staleness signals
               -> directive
                  -> signed audit record
                     -> replay (record-bound)
```

A conforming runtime SHOULD expose each stage as an inspectable step, and MUST
record the directive and its basis in an audit record.

## 2. The directive set

A conforming runtime emits one directive per governed claim. The directive set
is:

| Directive | Definition |
|-----------|------------|
| `pass` | The claim met evidence and state requirements; it may proceed. |
| `hedge` | The claim may proceed only with an explicit caveat about uncertainty or staleness. |
| `regenerate` | The claim is too weakly supported; a better-supported claim must be produced. |
| `block` | The claim is not permitted to become action. |
| `require_tool` | A factual or external claim asserted **without same-turn evidence** must bind evidence (via a tool action) **before** it is permitted to become action. |

**`require_tool` — full definition (for this profile):** when a candidate claim
concerns a fact or external state, and no evidence supporting it was produced in
the same turn, the runtime requires an evidence-binding action (e.g. reading the
source, running the test, checking the state) before the claim may proceed. The
intent is to forbid "stated, not observed" claims from becoming action.

> Implementation note: in the reference implementation, `require_tool` shipped
> in the `v0.3.0a1` alpha pre-release; enforcement is **opt-in and default-off**
> (env-gated) and not in the stable channel. The profile defines it so that
> conforming runtimes can target it; conformance reports MUST state whether a
> runtime emits it and whether enforcement is enabled.

## 3. Maturity ladder (CG-L0 .. CG-L5) and the naming-collision note

### 3.1 Naming-collision note (important)

This profile **already** uses two short ladders:

- **Evaluation-evidence maturity: L0–L3** (from v0.1.1) — how much evidence
  backs an *evaluation result*.
- **Determinism maturity: D0–D3** (telemetry/replay determinism) — how
  reproducible a runtime's behaviour is.

To avoid a clash with those existing ladders, the claim-governance ladder
introduced in this profile is named **CG-L0 .. CG-L5** (Claim-Governance
maturity). The `CG-` prefix is mandatory in this profile precisely so that
`CG-L2` is never confused with the evaluation `L2` or determinism
`D2`.

**Cross-ladder mapping (informative, not equivalence):**

| Claim-Governance (this profile) | Relation to evaluation L0–L3 | Relation to determinism D0–D3 |
|---------------------------------|------------------------------|-------------------------------|
| CG-L0 Ungoverned | independent axis | independent axis |
| CG-L1 Recorded | evidence L1+ (a record exists) | D1+ (record reproducible) |
| CG-L2 Governed claims | evidence L2 (claim-level evidence) | D1–D2 (record-bound replay) |
| CG-L3 Evidence-binding required | evidence L3 (binding required) | D2 (record-bound, integrity-checked) |
| CG-L4 Calibrated governance | evidence L3 + measured detector calibration | D2–D3 |
| CG-L5 Causally-governed | beyond current evidence ladder | D2–D3 (record-bound reproducibility) |

The mapping is informative. CG levels measure *claim governance*; the L0–L3 and
D0–D3 ladders measure *evaluation evidence* and *determinism* respectively. A
runtime can be high on one axis and low on another.

### 3.2 The ladder

| Level | Name | Meaning |
|-------|------|---------|
| CG-L0 | Ungoverned | Output used directly as truth; no claim extraction, no record. |
| CG-L1 | Recorded | Decisions/outputs recorded; no gate over claims. |
| CG-L2 | Governed claims | Claims mapped, checked against knowledge boundary + epistemic signals, directive (`pass/hedge/regenerate/block`) emitted, signed record, record-bound replay. |
| CG-L3 | Evidence-binding required | Runtime can require evidence-binding (`require_tool`) and measure whether prior context was bound (continuity binding). |
| CG-L4 | Calibrated governance | Detectors treated as sensors; their calibration measured over labelled data (e.g. ECE). |
| CG-L5 | Causally-governed | Directives are selected by causal reasoning over predicted effects, evaluated against live telemetry. Inherits record-bound replay from CG-L2/L3; the new capability is causal selection. Determinism here is record-bound reproducibility. |

### 3.3 Reference-implementation status (informative)

The reference runtime (Phionyx) has **reached CG-L3** in its `v0.3.0a1` alpha pre-release:
evidence-binding (`require_tool`) and continuity-binding are shipped (opt-in / default-off;
the stable channel remains **CG-L2**). **CG-L4 is in progress** — the detector-calibration
machinery is shipped and wired to the gate, but its calibration (ECE) is not yet validated
(§4). **CG-L5 is in progress** — a causal-reasoning engine exists in the runtime; causal
directive-selection over predicted effects is not yet wired into governance. In keeping with the claim ≤ evidence principle, each level reports exactly what is shipped versus pending.

## 4. Detector-calibration profile

> **Every detector is also a sensor.**

The epistemic signals that drive directives (confidence, staleness,
knowledge-boundary) are produced by detectors. Each detector is itself a noisy
sensor with its own error. A conforming runtime at CG-L4 MUST measure detector
calibration rather than assume it.

### Record schema (informative)

For each detector, over a labelled dataset, record:

- `detector_id`
- `predicted_signal`
- `observed_label`
- a calibration measure such as **expected calibration error (ECE)** — the gap
  between the detector's stated confidence and its actual hit rate.

### Honest status of the reference implementation

In the reference implementation, the detector-calibration machinery (a
calibration ledger that records each detector call as a measured sensor reading)
is **shipped**, but calibration is **tested, not yet validated**: the labelled
measurement window has not opened, so **ECE is currently unmeasured**. The
reference runtime therefore places CG-L4 **in progress**, not reached, and makes
no calibrated-governance claim. Conformance reports MUST state whether
calibration has been measured, and if so, the dataset and the resulting ECE.

## 5. Continuity-binding profile

A measure of whether prior established context (constraints, prior findings,
the prior plan) was **bound into** the current plan or action — as opposed to
retrieved-but-not-bound. This is a CG-L3 capability. In the reference
implementation it shipped in the `v0.3.0a1` alpha pre-release (enforcement
opt-in / default-off). Conformance reports MUST state whether the runtime
measures it.

## 6. Replay terminology

"Replay" in this profile means **record-bound replay**: reconstructing the governance
decision from the signed, hash-chained audit record. The recorded parameters — including
the recorded time value (`dt`) — are part of the record, so the same record yields the same
decision. It does not re-run the model. A conforming runtime MUST reproduce decisions from
the record.

## 7. Benchmark format (profile-defined, runner external)

The profile defines the **case/label schema** for evaluating claim governance;
the reference research repository provides the runner. A benchmark case
specifies: a scenario, a raw claim, the evidence available, the expected
governance result, and the expected directive. Calibration of any time-decay
signal MUST control for wall-clock confounds by fixing the decay setting during
evaluation.

> A benchmark referenced in public materials MUST have a real, sufficiently
> sized seed before it is cited as evidence. An empty or undersized referenced
> benchmark is not evidence.

## 8. What this profile does NOT claim

- It does **not** make a model truthful or deterministic.
- It does **not** guarantee that a passed claim is correct.
- It does **not** certify any AI system, including the reference
  implementation.
- It does **not** turn detector scores into oracles.

## Document control

- v0.3-draft — Noisy-Measurement Claim Governance Profile. Draft layer added on
  top of v0.1.1 and v0.2 without replacing them. No Zenodo DOI assigned (draft).
  CG-L0..CG-L5 ladder named with the `CG-` prefix to coexist with the existing
  evaluation L0–L3 and determinism D0–D3 ladders.
  - Reference-implementation position updated 2026-05-29: CG-L3 **reached** in the
    `phionyx-pipeline-mcp` `v0.3.0a1` alpha pre-release (evidence-binding +
    continuity-binding shipped; enforcement opt-in/default-off; stable channel
    remains CG-L2). CG-L4 **in progress** (calibration machinery shipped; ECE not
    yet validated). No CG-L4-reached or CG-L5 claim. "claim ≤ evidence."
