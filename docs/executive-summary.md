# XYZ Decision Space Model

## Executive Summary

**Policy-as-Code for Agentic AI Input Treatment**
**Version:** 0.4-draft
**Author:** Luke Haider
**Status:** Research and reference implementation

---

## The Problem

Agentic AI systems increasingly rely on multiple specialized agents working together. One agent may retrieve information, another may analyze risk, another may generate recommendations, and another may verify the final output.

A quiet failure mode appears when these agents pass information between each other: the receiving agent often has no formal way to know how each input is allowed to be treated.

Is the input a fixed constraint?
Is it context?
Is it a preference?
Is it a working hypothesis?
Can the agent modify it, summarize it, optimize it, or substitute a better value?

If that treatment is not declared, the agent decides implicitly during reasoning. In many cases that may be harmless. In high-consequence workflows, it can be a serious governance gap.

For example, in an aviation route-analysis system, a filed ATC route may arrive as plain text. Without governance metadata, an agent may treat the route as something it can optimize for efficiency, even though the route should be treated as a fixed operational constraint. The agent may not be reasoning badly. It was never told what kind of reasoning was permitted.

The failure is not only a reasoning failure. It is an input-treatment governance failure.

---

## The Concept

The **XYZ Decision Space Model** is a lightweight policy-as-code framework for governing how agents treat inputs before reasoning begins.

Each data element is assigned a per-agent coordinate binding:

* **X — Treatment Authority:** Who has authority to change the element’s meaning?
* **Y — Validity Stability:** How temporally stable and reliable is the value?
* **Z — Consequence:** What happens if the element is mishandled?

Together, these coordinates determine how an agent is allowed to treat the input.

The model does not try to control the internal reasoning of a language model directly. Instead, it controls the structure, metadata, permissions, verification, and audit trail around the inputs that enter the agent’s context.

---

## Why This Matters

Most agentic systems focus on what agents should produce. Fewer systems explicitly govern what agents are allowed to do with each input they receive.

This creates several risks:

* fixed constraints can drift into preferences;
* contextual information can be promoted into authority;
* authoritative text can be summarized in a way that changes meaning;
* dynamic inputs can be used after they become stale;
* downstream agents can inherit upstream outputs without knowing which parts were fixed, derived, or temporary.

The XYZ model makes input treatment explicit before reasoning begins.

---

## How the Model Works

The model has three main implementation artifacts.

### 1. Governing Manifest

The **Governing Manifest** is a YAML or JSON policy file that declares:

* governed elements;
* agents;
* per-agent X/Y/Z coordinate bindings;
* treatment posture for each element;
* semantic permissions;
* verification requirements;
* override conditions;
* audit requirements;
* semantic compliance checks.

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

* element ID;
* value;
* X/Y/Z coordinates;
* posture label;
* semantic permission statement;
* override conditions;
* integrity hash;
* timestamp;
* verification and audit metadata as needed.

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

For example, the destination airport `KSEA` may be:

| Agent                     | Treatment                                                      |
| ------------------------- | -------------------------------------------------------------- |
| Route Decomposition Agent | IMMUTABLE — use as fixed mission destination                   |
| Airspace Security Agent   | ANCHORED — use as stable context for risk assessment           |
| Diversion Analysis Agent  | NEGOTIABLE — evaluate alternatives only within diversion logic |

This is not inconsistency. It reflects that each agent has a different authority relationship to the same data.

---

## Integrity and Semantic Compliance

The model separates two kinds of enforcement.

### Integrity Checks

Integrity checks detect whether a value was structurally changed.

Example: if an immutable ATC route has a different hash at handoff, the route was modified.

### Semantic Compliance Checks

Semantic compliance checks detect whether the value was misused even if it was not structurally changed.

Examples include:

* no substitution;
* no route expansion;
* no promotion of adjacent FIRs into transit FIRs;
* no treating a constraint as a preference;
* no inferred optimization.

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

## Aviation Proof of Concept

The initial proof of concept is based on an aviation route-analysis multi-agent system.

In this use case, certain elements must remain semantically fixed across agent handoffs:

* filed ATC route;
* flight level;
* destination;
* FIR sequence;
* route structure.

Other elements are dynamic or agent-assessed:

* SIGMET data;
* FIR risk scores;
* diversion candidates;
* medical capability assessments;
* airport suitability rankings.

The XYZ model allows these elements to be governed differently. A filed route can be marked `IMMUTABLE`, while diversion candidates can be `AGENT_OWNED`, and SIGMET data can be `ANCHORED` but subject to staleness checks.

This prevents an agent from treating a fixed ATC route as a negotiable fuel-efficiency preference while still allowing appropriate reasoning over diversion options and risk assessments.

---

## Differentiation

The XYZ Decision Space Model does not replace existing governance approaches. It addresses a specific gap.

| Approach               | Primary Focus                           | Gap XYZ Addresses                                     |
| ---------------------- | --------------------------------------- | ----------------------------------------------------- |
| Constitutional AI      | Output behavior and refusal rules       | Input treatment before reasoning begins               |
| RAG metadata filtering | What content is retrieved               | What agents may do with retrieved content             |
| Tool/function schemas  | API structure and required fields       | Semantic authority and mutability of values           |
| Data contracts         | Data format and quality                 | Reasoning permissions over data                       |
| Formal verification    | Deterministic state-machine correctness | Probabilistic agents handling natural language inputs |

The contribution of XYZ is the combination of:

* per-agent input-treatment binding;
* design-time semantic permission declaration;
* governed payloads;
* gateway enforcement;
* integrity plus semantic compliance checks;
* governance design scoring.

---

## Repository Purpose

This repository is intended to serve as a concept evidence package and reference implementation.

It includes or is expected to include:

* whitepaper;
* executive summary;
* formal model specification;
* governing manifest schema;
* posture envelope schema;
* aviation proof-of-concept manifest;
* reference Python implementation;
* basic tests;
* MIT capstone support materials.

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
