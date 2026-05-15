# XYZ Decision Space Model

Policy-as-code for semantic input treatment in agentic AI pipelines.

## Summary

The XYZ Decision Space Model is a lightweight governance framework for multi-agent AI systems. It addresses a common failure mode: agents receive unannotated inputs and implicitly decide whether those inputs are constraints, preferences, context, or working variables.

XYZ solves this by assigning each input element a per-agent coordinate binding:

- X — Treatment Authority
- Y — Validity Stability
- Z — Consequence

The coordinates produce a posture envelope that tells the receiving agent how it may treat the input before reasoning begins.

## Why This Matters

In agentic systems, prompts often describe the task but do not formally declare the semantic treatment of each input. This can cause agents to reinterpret fixed constraints, optimize around required values, or promote contextual data into authoritative data.

XYZ provides a design-time manifest and runtime gateway pattern to reduce that risk.

## Core Concepts

### X — Treatment Authority

Who has authority to change the element’s meaning?

### Y — Validity Stability

How temporally stable and reliable is the value?

### Z — Consequence

What happens if the element is mishandled?

## Postures

- IMMUTABLE
- ANCHORED
- NEGOTIABLE
- AGENT_OWNED
- EPHEMERAL

## Architecture

1. Governing Manifest
2. Gateway Layer
3. Posture Envelope
4. Agent Reasoning Protocol
5. Integrity and Semantic Compliance Checks

## Repository Contents

- `docs/` — whitepaper, model specification, proof of concept
- `schemas/` — JSON schemas for manifests and posture envelopes
- `examples/` — sample manifests and aviation proof-of-concept
- `src/` — reference Python implementation
- `tests/` — basic test coverage
- `capstone/` — MIT capstone-facing summary materials

## Aviation Proof of Concept

The initial proof of concept is based on an aviation route-analysis multi-agent system. Filed ATC routes and FIR sequences are examples of inputs that must be treated as semantically immutable across agent handoffs.

## Status

Version: 0.4-draft  
Status: Pre-publication, internal research / capstone reference  
Author: Luke Haider

## Disclaimer

This repository is a research and reference implementation. It is not operationally certified and should not be used as a sole safety mechanism in aviation or other high-consequence environments.
