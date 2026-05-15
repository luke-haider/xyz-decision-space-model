# Project Context

## XYZ Decision Space Model / AAI Capstone

This file is the source-of-truth context note for the `xyz-decision-space-model` repository. Its purpose is to reduce context drift when developing the model, repository structure, documentation, examples, schemas, and reference implementation.

---

## Fixed Framing

The repository has three related concepts that must remain distinct:

| Concept | Role in Repository |
|---|---|
| **XYZ Decision Space Model** | The reusable governance model for semantic input treatment in agentic AI pipelines |
| **AAI / Aging Aircraft Index** | The primary MIT Applied Agentic AI certificate capstone application |
| **RIA / Route Intelligence Agent** | The original concept driver and secondary aviation proof-of-concept |

The correct framing is:

```text
XYZ = reusable governance model
AAI = primary capstone application
RIA = original concept driver / secondary proof-of-concept
```

Do not allow RIA to replace AAI as the capstone framing. RIA remains valuable because it clearly illustrates semantic immutability of route constraints. AAI is the broader capstone application because it demonstrates a full agentic data lifecycle: source retrieval, normalization, deduplication, clustering, health scoring, reporting, and verification.

---

## Repository Purpose

This repository is intended to serve as a concept evidence package and reference implementation for the XYZ Decision Space Model.

It should demonstrate that XYZ is not only an abstract essay, but a model that can be:

- explained clearly;
- specified formally;
- represented as policy-as-code;
- applied to a real agentic workflow;
- mapped to concrete data elements and agents;
- enforced through a gateway pattern;
- supported by schemas, examples, and reference code.

The repository supports MIT capstone review, personal learning, and future development of agentic AI governance concepts.

---

## Core Model Summary

The XYZ Decision Space Model governs how agents may treat inputs before reasoning begins.

Each element receives a per-agent coordinate binding:

- **X — Treatment Authority:** who has authority to change the element’s meaning?
- **Y — Validity Stability:** how temporally stable and reliable is the value?
- **Z — Consequence:** what happens if the element is mishandled?

The coordinate binding produces a semantic permission class called a **posture**.

Postures:

- `IMMUTABLE`
- `ANCHORED`
- `NEGOTIABLE`
- `AGENT_OWNED`
- `EPHEMERAL`

The posture is delivered to the receiving agent through a governed payload called a **Posture Envelope**.

---

## Key Model Rules

These rules should remain stable unless the model version is intentionally revised.

### Axis Rules

- **X = Treatment Authority**  
  X is about who may change the element’s meaning. It is not merely provenance.

- **Y = Validity Stability**  
  Y is about temporal stability, freshness, and confidence. It is not immutability.

- **Z = Consequence**  
  Z is about the consequence of mishandling. Z mainly affects enforcement intensity, audit depth, and escalation.

### Posture Rules

- `IMMUTABLE` means the agent may reason **with** the element but may not reason **over** its meaning.
- `ANCHORED` means the agent may use the element for context but may not change its operative meaning.
- `NEGOTIABLE` means the agent may reason over the element within declared override conditions.
- `AGENT_OWNED` means the agent has discretion over the element, subject to audit and verification rules.
- `EPHEMERAL` is lifecycle-based temporary working state and should not be passed downstream unless promoted.

### Enforcement Rules

- Hashing detects structural mutation.
- Semantic compliance checks detect meaning misuse.
- A hash can prove that a value did not change; it cannot prove the agent used the value correctly.
- AAS, ARS, and GP are governance design heuristics, not validated operational risk metrics.

---

## AAI Capstone Context

AAI is the primary MIT capstone application.

AAI is an aviation reliability-intelligence workflow that uses multiple agents to retrieve, normalize, deduplicate, cluster, score, and report aircraft health signals from operational and maintenance sources.

### AAI Data Sources

AAI may use:

- Top of Descent machine-generated reports;
- pilot end-of-day reports;
- maintenance end-of-day reports;
- service issue notes;
- MEL references;
- component or inspection reference data;
- free-text operational observations.

### AAI Agentic Workflow

```text
Raw Reports
   │
   ▼
Retrieval
   │
   ▼
Normalization
   │
   ▼
Deduplication
   │
   ▼
Clustering
   │
   ▼
Health Scoring
   │
   ▼
Report Generation
   │
   ▼
Verification / Human Review
```

### AAI Governance Challenge

AAI must preserve the distinction between:

1. raw source evidence;
2. anchored operational or maintenance context;
3. derived structured records;
4. inferred clusters;
5. agent-generated scores and recommendations;
6. final reports;
7. temporary working state.

XYZ is used to declare these distinctions before each agent reasons over the data.

---

## RIA Concept Driver Context

RIA is the original concept driver and a secondary aviation proof-of-concept.

RIA is useful because it clearly illustrates semantic immutability:

- filed ATC routes must not be treated as negotiable preferences;
- FIR sequences must not be expanded by inference;
- adjacent FIRs must not be promoted into transit FIRs;
- fixed route constraints must not be optimized away by downstream agents.

RIA should be preserved in the repository as supporting context, but it should not be presented as the primary capstone application.

---

## Current Repository Structure

Expected repository structure:

```text
xyz-decision-space-model/
│
├── README.md
├── PROJECT_CONTEXT.md
├── CHANGELOG.md
│
├── docs/
│   ├── whitepaper.md
│   ├── executive-summary.md
│   └── model-spec.md
│
├── capstone/
│   ├── mit-capstone-summary.md
│   ├── aai-agentic-system-overview.md
│   └── aai-xyz-governance-mapping.md
│
├── examples/
│   ├── aging-aircraft-index/
│   │   ├── README.md
│   │   └── manifest.yaml
│   │
│   └── route-intelligence-agent/
│       └── README.md
│
├── schemas/
│   ├── governing-manifest.schema.json
│   └── posture-envelope.schema.json
│
├── src/
│   └── xyz_dsm/
│       ├── posture.py
│       ├── scoring.py
│       └── gateway.py
│
└── tests/
    ├── test_posture.py
    ├── test_scoring.py
    └── test_gateway.py
```

Not all files may exist yet. This structure describes the intended direction.

---

## Documentation Rules

When creating or editing documentation:

- Keep AAI as the primary capstone application.
- Keep RIA as the original concept driver and secondary proof-of-concept.
- Avoid claiming operational aviation certification or safety-critical readiness.
- Avoid overclaiming novelty.
- Prefer the phrase: “Existing approaches address adjacent problems, but XYZ provides a lightweight per-agent, per-element declaration of semantic treatment authority before reasoning begins.”
- Frame AAS, ARS, and GP as design heuristics.
- Preserve aviation disclaimers.
- Use clear Markdown headings and tables.
- Prefer concrete examples over abstract claims.

---

## Implementation Rules

When creating schemas, manifests, or code:

- Ensure example postures match the posture decision logic.
- Preserve the distinction between source values and derived values.
- Keep semantic compliance checks explicit and domain-specific.
- Treat `EPHEMERAL` as lifecycle-based.
- Do not let Z grant semantic authority; Z should primarily modulate enforcement.
- Do not let Y imply immutability; Y should control freshness and verification.
- Include source links or source IDs wherever derived records are created.
- Design examples to be understandable without proprietary data.

---

## In Scope

The repository may include:

- model documentation;
- executive summaries;
- formal specifications;
- AAI capstone documentation;
- RIA concept-driver documentation;
- example YAML manifests;
- JSON schemas;
- small Python reference implementation;
- unit tests;
- demo scripts;
- diagrams expressed in Markdown or Mermaid.

---

## Out of Scope

The repository should not claim to provide:

- an operational aviation maintenance system;
- an airworthiness decision system;
- a dispatch or flight safety system;
- certified safety-critical software;
- validated risk scoring;
- legal, regulatory, or compliance certification;
- proprietary operational data.

AAI and RIA examples are research and demonstration examples only.

---

## Near-Term Next Steps

Recommended next steps:

1. Add `docs/model-spec.md` as the formal non-narrative specification.
2. Add `schemas/governing-manifest.schema.json`.
3. Add `schemas/posture-envelope.schema.json`.
4. Add minimal reference implementation under `src/xyz_dsm/`.
5. Add basic tests for posture derivation, scoring, and gateway wrapping.
6. Add sample AAI input elements and sample posture envelopes.
7. Add a simple demo script showing how a raw AAI source record becomes a governed posture envelope.

---

## Review Checklist

Before publishing or sharing a major revision, check:

- Does the repo still clearly state that AAI is the capstone application?
- Is RIA described as concept driver, not capstone?
- Do all examples use X, Y, and Z consistently?
- Do posture labels match the decision rules?
- Are raw source records preserved as evidence?
- Are inferred values labeled as inferred?
- Are health scores treated as agent-generated analytical products?
- Are disclaimers present?
- Are AAS, ARS, and GP described as heuristics?
- Does the repo avoid operational aviation claims?

---

## One-Sentence Summary

The XYZ Decision Space Model is a policy-as-code governance layer for agentic AI systems that declares how each agent may treat each input before reasoning begins, with AAI serving as the primary MIT capstone application and RIA preserved as the original route-intelligence concept driver.
