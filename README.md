# XYZ Decision Space Model

Policy-as-code for semantic input treatment in agentic AI pipelines.

## Summary

The XYZ Decision Space Model is a lightweight governance framework for multi-agent AI systems. It addresses a common failure mode: agents receive unannotated inputs and implicitly decide whether those inputs are constraints, preferences, context, or working variables.

XYZ solves this by assigning each input element a per-agent coordinate binding:

- **X — Treatment Authority:** who has authority to change the element’s meaning?
- **Y — Validity Stability:** how temporally stable and reliable is the value?
- **Z — Consequence:** what happens if the element is mishandled?

The coordinates produce a posture envelope that tells the receiving agent how it may treat the input before reasoning begins.

## Why This Matters

In agentic systems, prompts often describe the task but do not formally declare the semantic treatment of each input. This can cause agents to reinterpret fixed constraints, optimize around required values, promote contextual data into authoritative data, or use dynamic data after it has become stale.

XYZ provides a design-time manifest and runtime gateway pattern to reduce that risk.

## Capstone Context

This repository supports an MIT Applied Agentic AI certificate capstone focused on the **Aging Aircraft Index (AAI)** agentic system.

The AAI system is an aviation reliability-intelligence workflow that uses multiple agents to retrieve, normalize, deduplicate, cluster, score, and report aircraft health signals from machine-generated reports, pilot end-of-day reports, maintenance reports, and reference data. XYZ provides the governance layer for declaring how each agent may treat each input before reasoning begins.

The model was initially motivated by related route-intelligence failure modes, where filed ATC routes and FIR sequences must remain semantically immutable across multi-agent handoffs. In this repository, Route Intelligence Agent (RIA) material is treated as an original concept driver and secondary aviation proof-of-concept, while AAI is the primary capstone application.

## Core Concepts

### X — Treatment Authority

Who has authority to change the element’s meaning?

### Y — Validity Stability

How temporally stable and reliable is the value?

### Z — Consequence

What happens if the element is mishandled?

## Postures

- `IMMUTABLE` — use as fixed input; do not reinterpret, modify, substitute, omit, or optimize.
- `ANCHORED` — use for context and reasoning; do not change operative meaning or substitute.
- `NEGOTIABLE` — reason over and modify within declared override conditions.
- `AGENT_OWNED` — agent has full discretion over the element.
- `EPHEMERAL` — temporary working state; not passed downstream unless promoted.

## Architecture

1. Governing Manifest
2. Gateway Layer
3. Posture Envelope
4. Agent Reasoning Protocol
5. Integrity and Semantic Compliance Checks
6. Governance Design Scores

## Repository Contents

- `docs/` — whitepaper, executive summary, and model specification
- `examples/aging-aircraft-index/` — AAI capstone example manifest and notes
- `examples/route-intelligence-agent/` — RIA concept-driver notes and route-analysis proof-of-concept material
- `schemas/` — JSON schemas for manifests and posture envelopes
- `src/` — reference Python implementation
- `tests/` — basic test coverage
- `capstone/` — MIT capstone-facing summary materials

## AAI Capstone Application

The primary capstone application is the Aging Aircraft Index agentic system. In AAI, different elements require different treatment rules:

- raw machine logs should be preserved as source evidence;
- pilot and maintenance reports may be interpreted but not silently rewritten;
- normalized event records are agent-generated structured outputs;
- cluster assignments and aircraft health scores are agent-owned analytical products;
- maintenance references and MEL references may require anchored or immutable treatment;
- published reports should preserve conclusions and evidence trails.

XYZ provides a way to declare these distinctions as policy before each agent reasons over the data.

## Route Intelligence Concept Driver

The model was originally motivated by route-intelligence examples where a filed ATC route or FIR sequence could be incorrectly treated as negotiable by a downstream agent. Those examples remain useful because they clearly illustrate semantic immutability, gateway enforcement, hash integrity checks, and semantic compliance checks such as `no_substitution`, `no_expansion`, `no_promotion`, and `no_optimization`.

## Status

Version: 0.4-draft  
Status: Pre-publication, research / capstone reference  
Author: Luke Haider

## Disclaimer

This repository is a research and reference implementation. It is not operationally certified and should not be used as a sole safety mechanism in aviation or other high-consequence environments. The aviation examples are simplified demonstrations intended to illustrate agentic AI governance concepts and should not be used as operational procedures.
