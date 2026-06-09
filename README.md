# XYZ Decision Space Model

**Policy-as-code for semantic input treatment in agentic AI pipelines.**

The XYZ Decision Space Model is a lightweight governance framework for multi-agent AI systems. It defines how each agent is allowed to treat each input before reasoning begins.

In many agentic workflows, agents receive inputs without knowing whether those inputs are fixed constraints, contextual evidence, preferences, temporary working variables, or agent-owned analytical outputs. XYZ addresses that gap by assigning each governed input a per-agent treatment posture.

## What This Project Demonstrates

This repository demonstrates:

- agentic AI system design
- AI governance for multi-agent workflows
- policy-as-code input treatment
- semantic permission modeling
- aviation reliability intelligence use cases
- JSON/YAML manifest design
- reference implementation patterns for governed agent handoffs

## Core Idea

Each governed input receives a per-agent XYZ binding:

- **X — Treatment Authority:** who has authority to change the element’s meaning?
- **Y — Validity Stability:** how temporally stable and reliable is the value?
- **Z — Consequence:** what happens if the element is mishandled?

The XYZ binding produces a **posture envelope** that tells the receiving agent how it may treat the input.

## Example

A raw maintenance report and an aircraft health score should not be treated the same way.

A raw report may be source evidence that should be preserved. A health score may be an agent-generated analytical output that can be revised when new evidence appears.

XYZ makes those differences explicit before the agent reasons over the data.

## Architecture

```text
Raw / Source Inputs
        ↓
Governing Manifest
        ↓
Gateway Layer
        ↓
Posture Envelope
        ↓
Agent Reasoning Protocol
        ↓
Integrity + Semantic Compliance Checks
        ↓
Governed Agent Output
```

## Postures

- `IMMUTABLE` — use as fixed input; do not reinterpret, modify, substitute, omit, or optimize.
- `ANCHORED` — use for context and reasoning; do not change operative meaning or substitute.
- `NEGOTIABLE` — reason over and modify within declared override conditions.
- `AGENT_OWNED` — agent has full discretion over the element.
- `EPHEMERAL` — temporary working state; not passed downstream unless promoted.

## Capstone Context

This repository supports an MIT Applied Agentic AI certificate capstone focused on the **Aging Aircraft Index (AAI)** agentic system.

The AAI system is an aviation reliability-intelligence workflow that uses multiple agents to retrieve, normalize, deduplicate, cluster, score, and report aircraft health signals from machine-generated reports, pilot end-of-day reports, maintenance reports, and reference data.

XYZ provides the governance layer for declaring how each agent may treat each input before reasoning begins.

## Route Intelligence Concept Driver

The model was initially motivated by route-intelligence examples where a filed ATC route or FIR sequence could be incorrectly treated as negotiable by a downstream agent.

Those examples remain useful because they illustrate semantic immutability, gateway enforcement, hash integrity checks, and semantic compliance checks such as `no_substitution`, `no_expansion`, `no_promotion`, and `no_optimization`.

## Repository Contents

- `docs/` — whitepaper, executive summary, and model specification
- `examples/aging-aircraft-index/` — AAI capstone example manifest and notes
- `examples/route-intelligence-agent/` — RIA concept-driver notes and proof-of-concept material
- `schemas/` — JSON schemas for manifests and posture envelopes
- `src/` — reference Python implementation
- `tests/` — basic test coverage
- `capstone/` — MIT capstone-facing summary materials

## Status

| Field | Value |
|---|---|
| Version | 0.4-draft |
| Status | Research / capstone reference |
| Primary application | Aging Aircraft Index |
| Secondary concept driver | Route Intelligence Agent |
| Operational use | Not certified; demonstration only |

## Not Intended For

This repository is a research and reference implementation. It is not certified for operational aviation use, regulatory compliance, legal decision-making, or safety-critical deployment.

The aviation examples are simplified demonstrations for explaining agentic AI governance concepts.

## Author

Luke Haider
