# XYZ Decision Space Model

## Executive Summary

**Policy-as-Code for Agentic AI Input Treatment**  
**Version:** 0.4-draft  
**Author:** Luke Haider  
**Status:** Research and reference implementation  

---

## The Problem

Agentic AI systems increasingly rely on multiple specialized agents working together. One agent may retrieve information, another may normalize it, another may cluster related signals, another may generate a score, and another may produce a final report.

A quiet failure mode appears when these agents pass information between each other: the receiving agent often has no formal way to know how each input is allowed to be treated.

Is the input a fixed source record?  
Is it context?  
Is it a preference?  
Is it a working hypothesis?  
Can the agent modify it, summarize it, optimize it, infer from it, or substitute a better value?

If that treatment is not declared, the agent decides implicitly during reasoning. In many cases that may be harmless. In high-consequence workflows, it can be a serious governance gap.

For example, in an aircraft reliability workflow, a raw machine-generated log, a pilot narrative report, a maintenance resolution note, an agent-inferred cluster, and an aircraft health score all require different treatment. If an agent silently rewrites source evidence, treats an inferred cluster as confirmed fact, or marks a discrepancy resolved without maintenance evidence, the system may produce a polished but poorly governed output.

The failure is not only a reasoning failure. It is an input-treatment governance failure.

---

## The Concept

The **XYZ Decision Space Model** is a lightweight policy-as-code framework for governing how agents treat inputs before reasoning begins.

Each data element is assigned a per-agent coordinate binding:

- **X — Treatment Authority:** Who has authority to change the element’s meaning?
- **Y — Validity Stability:** How temporally stable and reliable is the value?
- **Z — Consequence:** What happens if the element is mishandled?

Together, these coordinates determine how an agent is allowed to treat the input.

The model does not try to control the internal reasoning of a language model directly. Instead, it controls the structure, metadata, permissions, verification, and audit trail around the inputs that enter the agent’s context.

---

## Capstone Context

This repository supports an MIT Applied Agentic AI certificate capstone focused on the **Aging Aircraft Index (AAI)** agentic system.

AAI is an aviation reliability-intelligence workflow that uses multiple agents to retrieve, normalize, deduplicate, cluster, score, and report aircraft health signals from machine-generated reports, pilot end-of-day reports, maintenance reports, and reference data.

The Route Intelligence Agent (RIA) was an earlier concept driver for the XYZ model, especially around the need to preserve filed ATC routes and FIR sequences as semantically immutable across multi-agent handoffs. In this repository, AAI is the primary capstone application and RIA is a secondary aviation proof-of-concept.

---

## Why This Matters

Most agentic systems focus on what agents should produce. Fewer systems explicitly govern what agents are allowed to do with each input they receive.

This creates several risks:

- fixed source evidence can be silently rewritten;
- contextual information can be promoted into authority;
- authoritative text can be summarized in a way that changes meaning;
- dynamic inputs can be used after they become stale;
- inferred outputs can be treated as confirmed facts;
- downstream agents can inherit upstream outputs without knowing which parts were fixed, derived, or temporary.

The XYZ model makes input treatment explicit before reasoning begins.

---

## How the Model Works

The model has three main implementation artifacts.

### 1. Governing Manifest

The **Governing Manifest** is a YAML or JSON policy file that declares:

- governed elements;
- agents;
- per-agent X/Y/Z coordinate bindings;
- treatment posture for each element;
- semantic permissions;
- verification requirements;
- override conditions;
- audit requirements;
- semantic compliance checks.

This manifest is the policy-as-code layer.

### 2. Gateway Layer

The **Gateway Layer** sits between raw data and agent reasoning.

Before an agent receives an input, the gateway:

1. looks up the element’s per-agent binding;
2. checks whether the receiving agent is allowed to receive the element;
3. applies freshness or staleness rules based on Y;
4. computes an integrity hash;
5. wraps the value in a governed payload called a posture envelope;
6. delivers the governed payload to the agent.

### 3. Posture Envelope

A **Posture Envelope** carries the value plus its governance metadata.

It includes:

- element ID;
- value;
- X/Y/Z coordinates;
- posture label;
- semantic permission statement;
- override conditions;
- integrity hash;
- timestamp;
- verification and audit metadata as needed.

Agents receive governed payloads rather than unannotated raw values.

---

## Treatment Postures

Each element receives a semantic permission class called a **posture**.

### IMMUTABLE

The agent may use the element as a fixed input, but may not reinterpret, modify, substitute, summarize in a meaning-changing way, omit, or optimize it.

The agent may reason **with** the element, but may not reason **over** its meaning.

### ANCHORED

The agent may use the element for context and reasoning, but may not change its operative meaning or substitute an alternative. Conflicts must be escalated.

### NEGOTIABLE

The agent may reason over the element and modify, weigh, or replace it within declared override conditions.

### AGENT_OWNED

The agent has full discretion over the element. It may generate, revise, discard, or replace it.

### EPHEMERAL

Temporary internal working state. It is not authoritative and is not passed downstream unless explicitly promoted to a governed element.

---

## Per-Agent Binding

A key feature of the model is that the same element can have different treatment rules for different agents.

In AAI, a maintenance resolution note may be:

| Agent | Treatment |
|---|---|
| Retrieval Agent | IMMUTABLE — preserve exact source text and metadata |
| Normalization Agent | ANCHORED — extract structure while preserving source meaning |
| Health Scoring Agent | ANCHORED — use as authoritative context for resolution status |
| Report Generation Agent | ANCHORED — summarize without changing operative meaning |

This is not inconsistency. It reflects that each agent has a different authority relationship to the same data.

---

## Integrity and Semantic Compliance

The model separates two kinds of enforcement.

### Integrity Checks

Integrity checks detect whether a value was structurally changed.

Example: if an immutable raw machine log or source evidence link has a different hash at handoff, the source evidence was modified.

### Semantic Compliance Checks

Semantic compliance checks detect whether the value was misused even if it was not structurally changed.

Examples include:

- no source rewriting;
- no unsupported resolution claim;
- no treating inferred status as maintenance-confirmed status;
- no health score without evidence;
- no silent cluster merge;
- no treating a fixed route constraint as a preference.

This distinction is important. A hash can prove a value did not change. It cannot prove the agent used the value correctly.

---

## Governance Design Scores

The model also produces design heuristics for each agent.

### Agent Autonomy Score

Measures how much treatment authority an agent has over the elements it handles, weighted by consequence.

### Agent Risk Score

Measures how much temporal instability the agent operates under, weighted by consequence.

### Governance Pressure

Combines autonomy and risk to identify where stronger oversight may be needed.

These scores are not validated operational risk metrics. They are design heuristics for deciding where to place verification gates, human review, stronger audit trails, or stricter compliance checks.

---

## Primary Capstone Application: Aging Aircraft Index

The primary application in this repository is the **Aging Aircraft Index** agentic system.

In AAI, different elements require different treatment rules:

- raw Top of Descent machine logs should be preserved as source evidence;
- pilot and maintenance reports may be interpreted but not silently rewritten;
- source timestamps and aircraft identifiers must remain fixed;
- normalized event records are agent-generated structured outputs;
- deduplication links and cluster IDs are agent-generated analytical structures;
- discrepancy status must distinguish maintenance-confirmed status from inferred status;
- health scores are agent-generated analytical products requiring evidence and auditability;
- final reports must preserve source references and confidence boundaries.

XYZ provides a way to declare these distinctions as policy before each agent reasons over the data.

---

## Secondary Concept Driver: Route Intelligence Agent

The model was originally motivated by route-intelligence examples where a filed ATC route or FIR sequence could be incorrectly treated as negotiable by a downstream agent.

Those examples remain useful because they clearly illustrate semantic immutability, gateway enforcement, hash integrity checks, and semantic compliance checks such as:

- `no_substitution`;
- `no_expansion`;
- `no_promotion`;
- `no_optimization`.

RIA demonstrates preservation of fixed route constraints. AAI demonstrates a broader lifecycle: source evidence, derived structure, clustering, scoring, reporting, and verification.

---

## Differentiation

The XYZ Decision Space Model does not replace existing governance approaches. It addresses a specific gap.

| Approach | Primary Focus | Gap XYZ Addresses |
|---|---|---|
| Constitutional AI | Output behavior and refusal rules | Input treatment before reasoning begins |
| RAG metadata filtering | What content is retrieved | What agents may do with retrieved content |
| Tool/function schemas | API structure and required fields | Semantic authority and mutability of values |
| Data contracts | Data format and quality | Reasoning permissions over data |
| Formal verification | Deterministic state-machine correctness | Probabilistic agents handling natural language inputs |

The contribution of XYZ is the combination of:

- per-agent input-treatment binding;
- design-time semantic permission declaration;
- governed payloads;
- gateway enforcement;
- integrity plus semantic compliance checks;
- governance design scoring.

---

## Repository Purpose

This repository is intended to serve as a concept evidence package and reference implementation.

It includes or is expected to include:

- whitepaper;
- executive summary;
- formal model specification;
- governing manifest schema;
- posture envelope schema;
- AAI capstone documentation;
- AAI example governing manifest;
- RIA concept-driver materials;
- reference Python implementation;
- basic tests.

The goal is to show that the XYZ Decision Space Model is not just an abstract idea. It can be documented, represented as policy, applied to a real multi-agent workflow, and implemented in lightweight runtime logic.

---

## Status and Disclaimer

This project is a research and reference implementation. It is not certified for operational aviation use, safety-critical use, legal compliance, or regulatory decision-making.

The aviation examples are simplified demonstrations intended to illustrate agentic AI governance concepts. They should not be used as operational procedures.

---

## Summary

The XYZ Decision Space Model is a policy-as-code framework for governing semantic input treatment in agentic AI pipelines.

It answers a question that many multi-agent systems leave implicit:

**What is this agent allowed to do with this input?**

By assigning each element a per-agent X/Y/Z coordinate binding, deriving a treatment posture, wrapping the element in a governed payload, and validating handoffs through a gateway layer, the model makes input-treatment authority explicit, auditable, and enforceable.
