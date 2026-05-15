# CHANGELOG

All notable changes to the **XYZ Decision Space Model** are documented here.

This project follows a lightweight semantic versioning convention while the model remains in pre-publication draft status:

* **Major version** changes indicate a conceptual restructuring of the model.
* **Minor version** changes indicate new framework components, terminology changes, or implementation-level additions.
* **Patch version** changes indicate wording, formatting, consistency, or example corrections.

Current status: **Pre-publication internal working draft**

---

## [0.4-draft] — 2026-05-14

### Summary

Major conceptual cleanup and consistency revision. This version reframes the XYZ Decision Space Model as a **policy-as-code layer for agentic AI input treatment**, clarifies the three axes, formalizes posture logic, separates integrity enforcement from semantic compliance enforcement, and reframes governance scoring as design heuristics rather than validated operational risk metrics.

### Added

* Added formal framing of XYZ as **policy-as-code for agentic AI input treatment**.
* Added clearer distinction between:

  * **X — Treatment Authority**
  * **Y — Validity Stability**
  * **Z — Consequence**
* Added a complete **Posture Decision Table** intended to eliminate gaps in posture mapping.
* Added **EPHEMERAL** as a formal fifth posture for temporary internal working state.
* Added explicit distinction between:

  * **Integrity checks** — hash, schema, unchanged value, field preservation.
  * **Semantic compliance checks** — no substitution, no expansion, no promotion, no optimization.
* Added `compliance_rules` block to the Governing Manifest schema.
* Added semantic compliance examples for aviation route analysis:

  * `no_substitution`
  * `no_expansion`
  * `no_promotion`
  * `no_optimization`
* Added more precise explanation of how the gateway applies Y-based staleness checks before delivery.
* Added clearer description of the **Decision Space Visualization**:

  * X axis = treatment authority
  * Y axis = validity stability
  * Bubble size = Z consequence
  * Color = posture
* Added explicit statement that **AAS, ARS, and GP are governance design heuristics**, not validated operational risk metrics.
* Added refined aviation proof-of-concept failure mode analysis for:

  * ATC route reinterpretation
  * Staleness without verification

### Changed

* Renamed **X — Control Axis** to **X — Treatment Authority**.
* Renamed **Y — Stability Axis** to **Y — Validity Stability**.
* Refined **Z — Impact Axis** into **Z — Consequence**.
* Replaced earlier “no reasoning permitted” IMMUTABLE language with:

  > May use as a fixed input; may not reinterpret, modify, substitute, summarize, omit, or optimize.
* Clarified that an agent may reason **with** an IMMUTABLE element, but may not reason **over** its meaning.
* Reframed posture as a **semantic permission class** rather than a broad risk label.
* Reframed Y as a verification / staleness modifier rather than a determinant of immutability.
* Reframed Z as an enforcement / audit / escalation modifier rather than a direct source of semantic authority.
* Reworded novelty claim to avoid overstatement. New framing:

  > Existing approaches address adjacent problems, but they do not provide a lightweight per-agent, per-element, design-time declaration of semantic treatment authority with derived autonomy and risk scoring.
* Updated the agent reasoning protocol to prioritize posture first, coordinates second, and calibrated behavior third.
* Updated Python reference implementation to include:

  * `EPHEMERAL` posture enum
  * `semantic_permission`
  * `compliance_rules`
  * separation of integrity checks and semantic compliance checks
  * exclusion of EPHEMERAL elements from AAS / ARS computation

### Fixed

* Fixed earlier ambiguity where “immutable” was used across both X and Y, blurring treatment authority and validity stability.
* Fixed contradiction where EPHEMERAL appeared in examples but was not part of the posture enum.
* Fixed overbroad IMMUTABLE language that could have prevented agents from reasoning with fixed inputs.
* Fixed incomplete posture derivation logic from prior draft by introducing a complete posture mapping table.
* Fixed overclaiming that no other framework addresses related concerns.
* Fixed risk-score framing to avoid implying empirical validation where none has yet occurred.

### Known Issues / Follow-up Items

* The Posture Decision Table should receive one more consistency pass to avoid overlap between **AGENT_OWNED** and **EPHEMERAL**.
* EPHEMERAL should likely be treated as a **lifecycle posture** rather than a posture derived only from X/Z coordinates.
* Example element assignments should be checked against the posture table for consistency. Current items needing review include:

  * SIGMET Data
  * FIR Midpoint Data
  * Fuel Target
* The language “posture is derived from X and Z” should be refined if Z is primarily an enforcement modifier.
* A standard library of semantic compliance checks remains to be defined.
* Conflict resolution between competing high-authority elements remains open.

---

## [0.3-draft] — 2026-05-13

### Summary

Visualization and authorship cleanup release. This version added the 2D decision space visualization model and formalized the visual encoding of X, Y, and Z for agent-level governance review.

### Added

* Added the **Decision Space Visualization** model.
* Added 2D visual encoding:

  * X axis as horizontal position.
  * Y axis as vertical position.
  * Z as bubble size.
  * Posture as color.
* Added initial interactive architect concept via `xyz_architect.html`.
* Added per-agent visual comparison of element bindings.
* Added governance pressure / autonomy-risk quadrant visualization concept.

### Changed

* Corrected author name to **Haider**.
* Clarified that the decision space visualization is a fixed 2D representation and does not require 3D rotation.
* Improved the conceptual link between element distribution and agent governance profile.

### Fixed

* Fixed authorship metadata.
* Fixed early visualization ambiguity around how Z should be represented.

---

## [0.2-draft] — 2026-05-05

### Summary

Major architecture expansion. This version moved the model from a conceptual axis framework into an agent-governance architecture with per-agent bindings, posture envelopes, gateway enforcement, agent instantiation protocol, and derived governance scoring.

### Added

* Added **per-agent coordinate binding**.
* Added **Governing Manifest** concept.
* Added **Gateway Layer** architecture.
* Added **Posture Envelope** structure.
* Added **Agent Instantiation Protocol**.
* Added **XYZ Coordinate Reference Scale**.
* Added label-versus-coordinate distinction:

  * Posture label = behavioral rule.
  * Coordinates = calibration within the rule.
* Added **Boundary Proximity Rule**.
* Added initial **Agent Autonomy Score (AAS)**.
* Added initial **Agent Risk Score (ARS)**.
* Added initial **Governance Pressure (GP)**.
* Added initial Python reference implementation.
* Added aviation route-analysis proof-of-concept.

### Changed

* Shifted from global element scoring to **per-agent element scoring**.
* Clarified that the same element may carry different coordinates for different receiving agents.
* Expanded the model from a conceptual tool into an enforceable pipeline design pattern.
* Updated the implementation model to use per-agent bindings rather than global coordinates.

### Fixed

* Fixed early limitation where an element appeared to have one universal posture regardless of agent role.
* Fixed lack of enforcement mechanism by introducing gateway-based wrapping and validation.
* Fixed lack of agent prompt integration by adding system prompt installation logic.

---

## [0.1-draft] — 2026-05-04

### Summary

Initial working draft of the XYZ Decision Space Model.

### Added

* Introduced the core X-Y-Z coordinate concept.
* Defined the initial three axes:

  * X — control / fixedness versus agent discretion.
  * Y — stability / dynamic uncertainty.
  * Z — importance / consequence.
* Introduced the idea of using coordinates to govern agent treatment of inputs.
* Established the core problem statement: agents implicitly reinterpret inputs when semantic treatment is not declared.
* Introduced the aviation route-analysis use case as the motivating example.

### Notes

* This version was primarily conceptual.
* Posture logic, gateway enforcement, per-agent binding, and governance scoring were not yet fully developed.

---

## Roadmap / Pending Future Versions

### Planned for [0.4.1-draft]

* Perform consistency pass on posture table and examples.
* Resolve AGENT_OWNED / EPHEMERAL overlap.
* Clarify whether posture is derived primarily from X, or from X with limited Z/lifecycle modifiers.
* Align all aviation proof-of-concept coordinate examples with the posture decision rules.
* Refine “agents receive envelopes, never raw values” language into a more implementation-realistic governed payload model.

### Planned for [0.5-draft]

* Add formal manifest examples for the aviation route-analysis proof of concept.
* Add validator pseudocode for common semantic compliance checks.
* Add manifest scoring interview / coordinate assignment methodology.
* Add conflict-resolution protocol for competing high-authority elements.
* Add sub-element inheritance rules for compound elements.
* Add runtime coordinate revision protocol for long-running pipelines.

### Planned for [1.0]

* Freeze core terminology.
* Freeze posture decision logic.
* Freeze manifest schema.
* Provide complete working reference implementation.
* Provide at least one end-to-end worked example with manifest, gateway, agent prompt, and validation log.
* Define publication-ready scope, claims, and limitations.
