# XYZ Decision Space Model

## Policy-as-Code for Agentic AI Input Treatment

**Version:** 0.4-draft
**Status:** Pre-publication — research and reference implementation
**Author:** Luke Haider
**Date:** May 2026

---

## Abstract

Agentic AI systems increasingly rely on pipelines of specialized agents that receive, transform, and pass information across multiple reasoning steps. A persistent failure mode emerges when those agents receive inputs without any formal declaration of how the input is allowed to be treated. An agent may implicitly decide whether a value is a fixed constraint, contextual reference, preference, working hypothesis, or revisable parameter. When that implicit decision is wrong, the result is not necessarily a reasoning failure. It is a governance failure.

The XYZ Decision Space Model is a policy-as-code framework for governing semantic input treatment in agentic AI pipelines. Each data element is assigned a per-agent coordinate binding: X for treatment authority, Y for validity stability, and Z for consequence if mishandled. These coordinates produce a semantic permission class, or posture, that governs what an agent may do with an element before reasoning begins.

The model introduces three core implementation artifacts: a Governing Manifest, a Gateway Layer, and a Posture Envelope. The Governing Manifest declares per-agent bindings at design time. The Gateway Layer applies verification checks, computes integrity hashes, and wraps raw values in posture envelopes. The Posture Envelope carries the element value, coordinate binding, posture, semantic permission, verification metadata, and integrity hash to the receiving agent.

The initial proof of concept is an aviation route-analysis multi-agent system in which filed ATC routes, FIR sequences, SIGMET data, diversion candidates, and internal policy constraints must be handled differently by different agents. The model is designed to prevent agents from treating fixed operational constraints as negotiable preferences while still allowing appropriate reasoning over dynamic or agent-owned elements.

The XYZ Decision Space Model does not claim to replace constitutional AI, tool schemas, formal verification, data contracts, or human oversight. It addresses a specific gap: lightweight, per-agent, per-element declaration of semantic treatment authority before agent reasoning begins.

---

## 1. Motivation

Multi-agent AI systems often decompose complex tasks into specialized reasoning roles. One agent may retrieve information, another may classify risk, another may generate recommendations, and another may verify the final output. These systems can appear well-structured while still leaving a critical question unanswered:

**What is each agent allowed to do with each input it receives?**

In many agentic systems, this question is handled informally through prompt wording. A prompt might say, “Do not change the route,” or “Use this document as authoritative.” But once information moves across agent boundaries, is summarized, restructured, retrieved, or embedded into a larger context, the treatment authority of each element can become ambiguous.

An agent that receives a filed ATC route as plain text has no inherent way to know whether it is:

* a fixed operational constraint;
* a recommended route;
* a route to optimize;
* a reference route for comparison;
* or merely contextual background.

If the agent optimizes or substitutes that route, the failure may look like poor reasoning. But the deeper failure is that the system never formally declared the route’s semantic status.

The XYZ Decision Space Model exists to make that declaration explicit, machine-readable, and enforceable at the boundary where information enters agent reasoning.

---

## 2. Problem Statement

Agentic AI pipelines lack a lightweight standard mechanism for declaring the semantic treatment of individual input elements on a per-agent basis.

This produces several common failure modes:

1. **Constraint-to-preference drift**
   An agent treats a fixed constraint as something that may be optimized or improved.

2. **Context-to-authority promotion**
   An agent treats contextual information as if it were authoritative.

3. **Authority-to-summary degradation**
   An agent summarizes or paraphrases a source in a way that changes its operative meaning.

4. **Dynamic-input staleness**
   An agent consumes time-sensitive information without verifying whether it is still valid.

5. **Cross-agent semantic leakage**
   A downstream agent receives an upstream output without knowing which parts were fixed inputs, which were derived outputs, and which were temporary reasoning artifacts.

6. **Implicit agent discretion**
   Agents make decisions about what they are allowed to revise because the pipeline never explicitly assigned treatment authority.

These failure modes are especially important in high-consequence domains, but they also appear in ordinary business workflows, compliance analysis, contract review, operational planning, and decision-support systems.

---

## 3. Design Objective

The XYZ Decision Space Model is designed to provide a lightweight governance layer that:

* declares how each input element may be treated before reasoning begins;
* supports different treatment rules for the same element across different agents;
* separates authority, stability, and consequence into distinct dimensions;
* enables runtime wrapping, verification, and audit of governed elements;
* provides design heuristics for identifying agents that require stronger oversight;
* remains framework-agnostic and implementable through manifests, prompts, and gateway logic.

The model does not attempt to control the internal cognition of a language model. Instead, it controls the structure, metadata, verification logic, and declared permissions surrounding the inputs that enter the model’s context.

---

## 4. Core Concepts

### 4.1 Element

An **element** is any discrete unit of data, instruction, constraint, context, or intermediate output that enters or is produced within an agentic pipeline.

Examples include:

* user instructions;
* system constraints;
* filed routes;
* retrieved policy documents;
* live weather data;
* model-generated hypotheses;
* tool results;
* risk scores;
* intermediate summaries;
* scratchpad state.

Elements are the atomic units to which XYZ governance is applied.

### 4.2 Per-Agent Coordinate Binding

A **per-agent coordinate binding** is the assignment of X, Y, and Z values to a specific element as received by a specific agent.

The same element may have different coordinate bindings for different agents. This is a core feature of the model.

For example, a destination airport may be immutable for a route decomposition agent, anchored for an airspace risk agent, and negotiable for a diversion analysis agent. The difference is not inconsistency. It reflects that each agent has a different authority relationship to the same element.

### 4.3 Treatment Posture

A **treatment posture** is the semantic permission class assigned to an element. It tells an agent what operations it may perform on the element.

The five posture classes are:

* `IMMUTABLE`
* `ANCHORED`
* `NEGOTIABLE`
* `AGENT_OWNED`
* `EPHEMERAL`

Posture is primarily derived from treatment authority. Consequence affects how strictly the posture is enforced. Ephemeral status is lifecycle-based and applies to temporary internal working state.

### 4.4 Governing Manifest

The **Governing Manifest** is a machine-readable YAML or JSON file that declares:

* governed elements;
* agents;
* per-agent coordinate bindings;
* posture labels;
* semantic permissions;
* verification rules;
* override conditions;
* audit levels;
* compliance checks;
* governance thresholds.

The manifest is the policy-as-code artifact of the XYZ model.

### 4.5 Gateway Layer

The **Gateway Layer** is the runtime boundary between raw data and agent reasoning. Before any agent receives an element, the gateway looks up the relevant per-agent binding, applies verification logic, hashes the value, and wraps it in a posture envelope.

The gateway also validates governed elements during handoffs between agents.

### 4.6 Posture Envelope

A **Posture Envelope** is the governed payload delivered to an agent. It includes the element value plus the metadata required to govern its treatment.

A typical envelope includes:

```json
{
  "element_id": "atc_route",
  "value": "KSEA DCT TOU J523 YVR ...",
  "posture": "IMMUTABLE",
  "coordinates": {
    "x": -1.0,
    "y": 1.0,
    "z": 1.0
  },
  "semantic_permission": "Use as fixed input. Do not reinterpret, modify, substitute, omit, or optimize.",
  "override_conditions": "NONE",
  "integrity_hash": "sha256-hash-value",
  "wrapped_at": "2026-05-14T18:25:00Z"
}
```

Agents receive governed payloads rather than unannotated raw values.

---

## 5. The Three Axes

The XYZ model separates authority, stability, and consequence into three orthogonal dimensions.

### 5.1 X — Treatment Authority

**Question:** Who has authority to change this element’s meaning?

X is not merely a provenance tag. It does not ask where the element came from. It asks who, if anyone, has authority to alter the element’s operative meaning inside the pipeline.

| X Value | Meaning                                                                   | Example                                                    |
| ------: | ------------------------------------------------------------------------- | ---------------------------------------------------------- |
|    -1.0 | Fully external authority. Agent has no authority to change meaning.       | ATC clearance, filed route, regulation, signed instruction |
|    -0.5 | Predominantly external authority. Agent may contextualize but not revise. | Verified policy document, operator standing instruction    |
|     0.0 | Shared or negotiated authority. Agent may interpret within scope.         | User preference, retrieved reference material              |
|    +0.5 | Predominantly agent authority. Agent has primary discretion.              | Agent-scored risk assessment, recommendation               |
|    +1.0 | Full agent authority. Agent owns the element.                             | Scratchpad state, working hypothesis                       |

X determines semantic permission. It is the primary driver of posture.

### 5.2 Y — Validity Stability

**Question:** How temporally stable and epistemically reliable is this value?

Y captures whether the value can be trusted as current and valid at the time it is used. Y does not determine semantic authority. A value can be externally authoritative and still dynamic. A value can be agent-generated and stable within a session.

| Y Value | Meaning                                          | Example                                                               |
| ------: | ------------------------------------------------ | --------------------------------------------------------------------- |
|    +1.0 | Fully stable within the relevant pipeline window | Mathematical constant, signed contract, verified filed route snapshot |
|    +0.5 | Stable within session                            | Session-scoped user preference, pre-departure briefing                |
|     0.0 | Semi-stable                                      | NOTAM set, rolling forecast, periodically updated policy              |
|    -0.5 | Dynamic                                          | Active SIGMET, live traffic data, real-time feed                      |
|    -1.0 | Highly uncertain                                 | Probabilistic inference, model-generated estimate                     |

Y determines freshness and verification behavior.

### 5.3 Z — Consequence

**Question:** What happens if this element is mishandled?

Z represents the consequence of semantic misuse, stale use, omission, substitution, or inappropriate modification.

| Z Value | Meaning                | Example                                              |
| ------: | ---------------------- | ---------------------------------------------------- |
|     0.0 | Negligible consequence | Display formatting, minor annotation                 |
|     0.3 | Low consequence        | Style preference, minor quality issue                |
|     0.5 | Moderate consequence   | Suboptimal recommendation, correction required       |
|     0.7 | High consequence       | Operationally significant failure                    |
|     1.0 | Critical consequence   | Unsafe, illegal, mission-fatal, or regulatory breach |

Z determines enforcement intensity, audit depth, escalation thresholds, and governance pressure.

---

## 6. Treatment Postures

Treatment posture is the permission class that tells an agent what it may do with an element.

### 6.1 IMMUTABLE

The agent may use the element as a fixed input. The agent may not reinterpret, modify, substitute, summarize in a meaning-changing way, omit, or optimize the element.

An agent may reason **with** an immutable element, but may not reason **over** its meaning.

Example: An agent may use `KSEA` as a fixed destination in a route calculation. It may not decide that `KPDX` is a better destination and substitute it.

### 6.2 ANCHORED

The agent may use the element for context and reasoning. It may not change the element’s operative meaning or substitute an alternative. If the anchored element conflicts with another element, the agent must escalate rather than independently resolve the conflict.

Example: An internal policy document may be used as authoritative context. The agent may explain or apply the policy, but may not rewrite its requirement.

### 6.3 NEGOTIABLE

The agent may reason over the element, weigh it, revise it, or replace it within declared override conditions.

Example: A fuel target may be balanced against operational constraints if the manifest declares it negotiable.

### 6.4 AGENT_OWNED

The agent has full discretion over the element. It may generate, revise, discard, or replace the element.

Example: An agent-generated risk score or ranked recommendation may be agent-owned, subject to audit and verification requirements.

### 6.5 EPHEMERAL

Ephemeral elements are temporary internal working state. They are not authoritative and are not passed downstream unless explicitly promoted to governed elements.

Ephemeral status is lifecycle-based. It is not merely a consequence of high agent authority and low consequence.

Example: A scratchpad note, temporary hypothesis, or intermediate reasoning artifact is ephemeral unless the system promotes it to a named element with a declared binding.

---

## 7. Posture Decision Logic

The posture decision logic should be complete, deterministic, and free of overlapping classifications.

The recommended logic is:

1. If the element lifecycle is `ephemeral`, assign `EPHEMERAL`.
2. Else if `X <= -0.7`, assign `IMMUTABLE`.
3. Else if `-0.7 < X <= -0.3` and `Z >= 0.4`, assign `ANCHORED`.
4. Else if `-0.7 < X <= -0.3` and `Z < 0.4`, assign `NEGOTIABLE`.
5. Else if `-0.3 < X < +0.5`, assign `NEGOTIABLE`.
6. Else if `X >= +0.5`, assign `AGENT_OWNED`.

Z does not generally grant or remove semantic authority. Its main role is to determine how strictly the assigned posture is enforced. The limited use of Z near the `ANCHORED` / `NEGOTIABLE` boundary reflects a practical design choice: low-consequence, partially external elements may be treated as negotiable when the cost of mishandling is low.

The conservative boundary rule is:

> If an element lies near a posture boundary, apply the more restrictive posture unless the manifest explicitly declares otherwise.

---

## 8. Verification and Enforcement Modifiers

Posture determines what an agent may do. Y and Z determine how strictly the pipeline verifies and enforces that posture.

### 8.1 Y Verification Modifier

|           Y Range | Verification Requirement                                                              |
| ----------------: | ------------------------------------------------------------------------------------- |
|       `Y >= +0.5` | No staleness check required unless independently declared.                            |
| `0.0 <= Y < +0.5` | Staleness check recommended if `Z >= 0.5`.                                            |
| `-0.5 <= Y < 0.0` | Staleness check required if `Z >= 0.4`. Action on stale: refetch or flag provisional. |
|        `Y < -0.5` | Staleness check required regardless of Z. Action on stale: refetch or halt.           |

### 8.2 Z Enforcement Modifier

|          Z Range | Enforcement Behavior                                                              |
| ---------------: | --------------------------------------------------------------------------------- |
|       `Z >= 0.7` | Full audit log. Violations halt or escalate. Semantic compliance checks required. |
| `0.4 <= Z < 0.7` | Summary audit log. Violations logged and flagged. Escalation recommended.         |
|        `Z < 0.4` | Minimal logging. Violations noted. No automatic escalation unless declared.       |

### 8.3 Enforcement Weight

A simple enforcement-weight heuristic may be calculated as:

```text
enforcement_weight = base_posture_strictness × (1 + z)
```

Where default base posture strictness values are:

| Posture     | Base Strictness |
| ----------- | --------------: |
| IMMUTABLE   |             1.0 |
| ANCHORED    |             0.7 |
| NEGOTIABLE  |             0.4 |
| AGENT_OWNED |             0.1 |
| EPHEMERAL   |             0.0 |

This is a design heuristic, not a validated operational risk metric.

---

## 9. Gateway Layer

The Gateway Layer enforces the manifest boundary before any governed element reaches an agent.

### 9.1 Gateway Operation

At runtime, the gateway performs the following steps:

1. Receive `element_id`, value, and `receiving_agent_id`.
2. Look up the element’s per-agent binding in the Governing Manifest.
3. Halt if no binding exists for that element-agent pair.
4. Apply Y-based staleness or freshness checks.
5. Compute an integrity hash of the value.
6. Construct a posture envelope.
7. Deliver the governed payload to the receiving agent.

Pseudo-flow:

```text
gateway(element_id, value, receiving_agent_id):
    binding = manifest.lookup(receiving_agent_id, element_id)

    if binding is missing:
        halt("undeclared element for receiving agent")

    apply_y_verification(binding, value)
    hash = sha256(value)

    envelope = {
        element_id,
        value,
        posture,
        coordinates,
        semantic_permission,
        override_conditions,
        integrity_hash,
        wrapped_at
    }

    return envelope
```

### 9.2 Handoff Validation

At agent-to-agent handoff, the gateway validates governed elements before they proceed downstream.

Validation includes:

1. integrity checks;
2. semantic compliance checks;
3. audit logging;
4. escalation or halt behavior when required.

---

## 10. Integrity Checks vs. Semantic Compliance Checks

The XYZ model distinguishes between structural mutation and semantic misuse.

### 10.1 Integrity Checks

Integrity checks detect whether the element value changed.

Example: If an immutable ATC route has a different hash at handoff than it had when wrapped, the route was structurally modified.

Integrity checks are necessary but insufficient.

### 10.2 Semantic Compliance Checks

Semantic compliance checks detect whether an element was used incorrectly even if its raw value did not change.

Examples include:

* `no_substitution` — the agent did not replace the element with an alternative;
* `no_expansion` — the agent did not expand a structural sequence by inference;
* `no_promotion` — the agent did not elevate adjacent or contextual data into authoritative status;
* `no_optimization` — the agent did not treat a constraint as a preference;
* `completeness_check` — required supporting basis was included.

This distinction is critical. A hash can prove that a value did not change. It cannot prove the agent used the value correctly.

---

## 11. Agent Reasoning Protocol

The agent system prompt should declare the XYZ governance framework before role instructions and task instructions.

Recommended system prompt structure:

```text
[1] Governing framework declaration
[2] XYZ coordinate reference
[3] Posture semantic permissions
[4] Agent reasoning protocol
[5] Agent role and responsibilities
[6] Task instructions
[7] Governed input envelopes
```

For every governed input, the agent should follow this protocol:

1. Read the posture label first.
2. Treat the posture as the permission class.
3. Read the X/Y/Z coordinates second.
4. Use X to calibrate authority tension.
5. Use Y to calibrate freshness and uncertainty.
6. Use Z to calibrate caution, audit sensitivity, and escalation preference.
7. Apply judgment only within permitted semantic operations.
8. Flag any input that lacks a posture envelope.

When a boundary is ambiguous, the agent should prefer the more restrictive posture.

---

## 12. Governance Design Scores

The XYZ model includes three governance design scores. These scores are heuristics for system design and oversight placement. They are not validated operational risk metrics.

### 12.1 Agent Autonomy Score

The Agent Autonomy Score measures how much treatment authority an agent has over the elements it handles, weighted by consequence.

```text
AAS = Σ [ ((xᵢ + 1) / 2) × zᵢ ] / Σ zᵢ
```

Ephemeral elements are excluded unless explicitly promoted.

Interpretation:

| AAS Range | Design Class |
| --------: | ------------ |
|   0.0–0.2 | Executor     |
|   0.2–0.4 | Constrained  |
|   0.4–0.6 | Supervised   |
|   0.6–0.8 | Delegated    |
|   0.8–1.0 | Autonomous   |

### 12.2 Agent Risk Score

The Agent Risk Score measures how much temporal instability an agent operates under, weighted by consequence.

```text
ARS = Σ [ ((1 - yᵢ) / 2) × zᵢ ] / Σ zᵢ
```

Interpretation:

| ARS Range | Design Class |
| --------: | ------------ |
|   0.0–0.2 | Low          |
|   0.2–0.4 | Managed      |
|   0.4–0.6 | Elevated     |
|   0.6–0.8 | High         |
|   0.8–1.0 | Critical     |

### 12.3 Governance Pressure

Governance Pressure combines autonomy and risk into a single oversight design heuristic.

```text
GP = AAS × ARS × 2
```

A default value above `0.5` may indicate that an agent should have a formal verification gate, human review path, or stronger audit requirements. This threshold is a configurable starting point, not a validated safety boundary.

---

## 13. Decision Space Visualization

The Decision Space is a two-dimensional visualization of an agent’s element bindings.

* X axis: Treatment Authority
* Y axis: Validity Stability
* Bubble size: Z Consequence
* Color: Treatment Posture

```text
Y: Validity Stability

 +1.0 stable        externally governed          agent-governed
       │                 large bubbles = high consequence
       │
  0.0  ├─────────────────────────────── X: Treatment Authority
       │
 -1.0 uncertain
       └──────────────────────────────────────────────
        -1.0 external authority              +1.0 agent authority
```

The visualization allows reviewers to see an agent’s governance profile at a glance.

Large bubbles on the left side represent high-consequence elements that the agent does not control. Large bubbles on the right side represent consequential elements over which the agent has substantial discretion. Large bubbles low on the chart represent consequential elements with low validity stability and therefore stronger verification needs.

---

## 14. Aviation Proof of Concept

### 14.1 System Context

The initial proof of concept is based on an aviation route-analysis multi-agent system. The system decomposes a mission route, evaluates airspace risk, identifies diversion options, checks internal policy implications, and produces planning output.

The relevant agents include:

* Route Analysis Parent Agent;
* Route Decomposition Specialist;
* Airspace Security Specialist;
* Airport and Medical Diversion Specialist;
* Internal Policy Specialist;
* Verification Gate.

The core operational requirement is that certain elements, such as filed ATC routes and FIR sequences, must survive handoffs without semantic modification.

### 14.2 Example Elements

| Element                  |    X |    Y |   Z | Posture     | Rationale                                                     |
| ------------------------ | ---: | ---: | --: | ----------- | ------------------------------------------------------------- |
| Destination              | -1.0 | +1.0 | 1.0 | IMMUTABLE   | Fixed mission parameter for route decomposition               |
| ATC Route / Flight Level | -1.0 | +1.0 | 1.0 | IMMUTABLE   | Filed and verified route constraint                           |
| FIR Sequence             | -1.0 | +1.0 | 1.0 | IMMUTABLE   | Structural route sequence must not be expanded or substituted |
| SIGMET Data              | -0.5 | -0.8 | 0.8 | ANCHORED    | Authoritative weather hazard content, but dynamic             |
| FIR Risk Scores          | +0.5 | -0.3 | 0.8 | AGENT_OWNED | Agent-produced assessment                                     |
| Diversion Candidates     | +0.6 | -0.4 | 0.7 | AGENT_OWNED | Agent-generated candidate set                                 |
| Medical Capability       | +0.4 | -0.3 | 0.9 | NEGOTIABLE  | Agent-assessed, high-consequence evaluation                   |
| Internal Policy          | -0.4 | +0.7 | 0.7 | ANCHORED    | Authoritative but contextual policy source                    |
| Fuel Target              | -0.2 | +0.6 | 0.4 | NEGOTIABLE  | Operator preference subject to constraints                    |
| Route Hypothesis         | +0.9 | -0.5 | 0.3 | AGENT_OWNED | Working output; may become governed if promoted               |

### 14.3 Example Failure Mode

A downstream agent receives a filed ATC route and FIR sequence as plain strings. The agent identifies a more fuel-efficient route and substitutes or expands the original structure. The reasoning may be useful in another context, but it is invalid for this element because the agent had no authority to revise it.

XYZ diagnosis:

* ATC route binding: `X = -1.0`, `Y = +1.0`, `Z = 1.0`
* Derived posture: `IMMUTABLE`
* Permitted behavior: use as fixed input
* Prohibited behavior: reinterpret, modify, substitute, optimize, or expand

With XYZ governance, the gateway wraps the route and FIR sequence as immutable posture envelopes. The agent may reason with them, but not over their meaning. At handoff, semantic compliance checks verify that no substitution, expansion, adjacent FIR promotion, or inferred optimization occurred.

### 14.4 Oversight Placement

The XYZ scoring model can also identify which agents require stronger oversight.

A diversion specialist may have meaningful authority over high-consequence and dynamic elements, such as medical capability, airport suitability, weather constraints, and diversion candidates. This combination increases both autonomy and risk, producing higher governance pressure.

The model therefore supports the design decision to place a verification gate downstream of that agent.

---

## 15. Differentiation from Adjacent Frameworks

The XYZ Decision Space Model does not replace existing AI governance methods. It addresses a narrower gap.

| Framework               | Governs                                       | Leaves Open                                                 |
| ----------------------- | --------------------------------------------- | ----------------------------------------------------------- |
| Constitutional AI       | Output behavior and refusal patterns          | Input treatment before reasoning begins                     |
| BDI Architecture        | Beliefs, desires, intentions                  | Which beliefs are semantically fixed versus revisable       |
| RAG Metadata Filtering  | Retrieval selection                           | Semantic treatment of retrieved content after retrieval     |
| Tool / Function Schemas | API format and required fields                | Meaning, authority, and mutability of data in those schemas |
| Data Contracts          | Data format, schema, and quality expectations | Agent reasoning permissions over that data                  |
| Formal Verification     | State-machine or protocol correctness         | Probabilistic agent behavior over natural language inputs   |

The specific contribution of XYZ is the combination of:

* per-agent coordinate binding;
* design-time semantic treatment declaration;
* posture envelopes;
* gateway-based wrapping and validation;
* integrity plus semantic compliance checks;
* governance design scoring.

---

## 16. Implementation Pattern

A minimal implementation requires three components.

### 16.1 Governing Manifest

A YAML or JSON file declares elements, agents, bindings, verification rules, and compliance checks.

### 16.2 Gateway Runtime

A lightweight runtime component reads the manifest, wraps elements, computes hashes, applies freshness checks, validates handoffs, and logs violations.

### 16.3 Agent Prompt Protocol

Each agent receives a system prompt that defines the XYZ model, posture permissions, and the required behavior when processing governed payloads.

Together, these components allow XYZ to operate without modifying the underlying model architecture.

---

## 17. Limitations

The XYZ Decision Space Model is an early-stage governance framework and reference implementation. Important limitations remain.

1. **Coordinate assignment is judgment-based.**
   The quality of the manifest depends on the quality of the design process used to assign X, Y, and Z values.

2. **Semantic compliance checks are domain-specific.**
   Hashing is straightforward. Detecting semantic misuse requires pipeline-specific validators and review logic.

3. **Scores are heuristics.**
   AAS, ARS, and GP are useful for design review but are not validated operational risk metrics.

4. **Conflicting authorities require arbitration.**
   The current model requires additional rules for cases where one immutable element conflicts with another higher-order safety or legal requirement.

5. **Sub-element inheritance requires care.**
   Compound elements may contain sub-elements with different treatment rules.

6. **Prompt compliance is not guaranteed.**
   The model improves governance structure but does not guarantee that a language model will always comply with instructions.

7. **Operational use requires validation.**
   High-consequence domains require independent validation, audit, human oversight, and domain-specific controls.

---

## 18. Open Questions

The following questions remain for future development:

1. What structured interview or review process should be used to assign X/Y/Z coordinates consistently?
2. How should semantic compliance checks be standardized across domains?
3. How should coordinate bindings be updated in long-running pipelines?
4. How should compound elements inherit or override parent coordinate bindings?
5. How should governance pressure thresholds be calibrated with empirical pipeline data?
6. How should conflicts between immutable elements be arbitrated?
7. What is the right balance between manifest complexity and practical usability?
8. How can this model integrate with existing orchestration frameworks such as LangGraph, Microsoft Copilot Studio, or custom agent runtimes?

---

## 19. Repository Status

This document is part of a reference repository intended to support research, experimentation, and an MIT Applied Agentic AI capstone project.

The repository is expected to include:

* the whitepaper;
* an executive summary;
* a formal model specification;
* JSON schemas;
* sample manifests;
* aviation proof-of-concept examples;
* reference Python implementation;
* basic tests;
* capstone support materials.

---

## 20. Disclaimer

This document describes a research and reference implementation framework. It is not certified for operational aviation use, safety-critical use, legal compliance, or regulatory decision-making.

The aviation examples are simplified demonstrations intended to illustrate agentic AI governance concepts. They should not be used as operational procedures.

---

## Recommended Citation

Haider, L. (2026). *XYZ Decision Space Model: Policy-as-Code for Agentic AI Input Treatment* (v0.4-draft). Pre-publication working document.

---

## Changelog

| Version   | Date       | Notes                                                                                                                                                             |
| --------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0.1-draft | 2026-05-04 | Initial working concept.                                                                                                                                          |
| 0.2-draft | 2026-05-05 | Added per-agent coordinate binding, gateway architecture, and agent prompt protocol.                                                                              |
| 0.3-draft | 2026-05-13 | Added 2D decision space visualization and Z-as-bubble-size encoding.                                                                                              |
| 0.4-draft | 2026-05-14 | Clarified X/Y/Z axes, revised immutable language, formalized EPHEMERAL, separated integrity and semantic compliance checks, reframed scores as design heuristics. |
