# MIT Applied Agentic AI Capstone Summary

## Project Title

Aging Aircraft Index Agentic System with XYZ Decision Space Governance

## Repository Context

This repository documents the **XYZ Decision Space Model**, a policy-as-code framework for governing semantic input treatment in agentic AI pipelines.

For the MIT Applied Agentic AI certificate capstone, the primary application is the **Aging Aircraft Index (AAI)** agentic system. AAI is an aviation reliability-intelligence workflow designed to ingest, normalize, cluster, score, and report aircraft health signals from multiple operational data sources.

The Route Intelligence Agent (RIA) was an earlier concept driver for the XYZ model, especially around the problem of preserving fixed route constraints across agent handoffs. AAI is the capstone implementation context.

---

## Problem Statement

Aircraft reliability data is distributed across multiple sources and formats:

- machine-generated Top of Descent reports;
- pilot end-of-day reports;
- maintenance end-of-day reports;
- service issue notes;
- MEL references;
- component and inspection reference data;
- free-text operational observations.

These sources contain useful signals, but they are difficult to synthesize manually. Important patterns may emerge across time, across systems, or across report types before they become obvious as formal discrepancies.

The challenge is not only retrieving and summarizing the data. The larger challenge is ensuring that each agent treats each input correctly.

For example:

- a raw machine log should be preserved as source evidence;
- a pilot report may be interpreted, but not silently rewritten;
- a maintenance resolution note may carry more authority than an inferred agent conclusion;
- a cluster assignment may be agent-generated and revisable;
- an aircraft health score is an analytical product requiring auditability.

Without an explicit governance layer, agents can blur the distinction between raw evidence, interpreted context, inferred structure, and final conclusions.

---

## Proposed Solution

The capstone proposes an **Aging Aircraft Index agentic system** governed by the **XYZ Decision Space Model**.

AAI uses specialized agents to:

1. retrieve raw reliability and operations data;
2. normalize records into structured event objects;
3. deduplicate overlapping reports;
4. cluster recurring or related issues;
5. assess aircraft health;
6. generate a daily or periodic reliability report.

XYZ adds a governance layer that declares how each agent may treat each input before reasoning begins.

Each data element receives a per-agent coordinate binding:

- **X — Treatment Authority:** who may change the element’s meaning?
- **Y — Validity Stability:** how stable and current is the value?
- **Z — Consequence:** what happens if the element is mishandled?

These coordinates produce a treatment posture:

- `IMMUTABLE`
- `ANCHORED`
- `NEGOTIABLE`
- `AGENT_OWNED`
- `EPHEMERAL`

The posture is delivered to agents through a governed payload called a posture envelope.

---

## Agentic System Architecture

The AAI system is designed as a multi-agent workflow.

### 1. Retrieval Agents

Retrieve data from source systems, including machine reports, pilot reports, maintenance reports, and reference documents.

### 2. Normalization Agent

Transforms raw records into structured event objects while preserving links to source evidence.

### 3. Deduplication Agent

Identifies overlapping reports that likely refer to the same underlying event.

### 4. Clustering Agent

Groups related events by system, ATA, message type, failure mode, or recurrence pattern.

### 5. Health Scoring Agent

Generates an aircraft health assessment based on active clusters, recurrence, discrepancy status, severity, and resolution evidence.

### 6. Report Generation Agent

Produces a human-readable aircraft health report with evidence, conclusions, and monitoring recommendations.

### 7. Verification Gate

Checks that source data has not been altered, that high-consequence findings include evidence, and that agent-generated conclusions are labeled appropriately.

---

## Why Agentic AI Is Appropriate

AAI requires more than a simple chatbot or static report. It involves:

- multiple data types;
- multiple levels of authority;
- temporal reasoning;
- recurrence detection;
- conflict handling;
- summarization;
- structured output generation;
- auditability;
- human review.

A multi-agent system allows each task to be separated into a specialized role while preserving the ability to combine the results into a coherent aircraft health assessment.

---

## Role of XYZ Governance

XYZ is used to prevent a common agentic failure mode: treating all inputs as equally revisable context.

In AAI:

- raw machine logs should not be rewritten;
- pilot and maintenance reports should remain traceable to the original source;
- agent-normalized records should be labeled as derived outputs;
- cluster assignments should be revisable but auditable;
- health scores should be treated as agent-generated analytical products;
- published reports should preserve source references and confidence levels.

The XYZ model makes these distinctions explicit through the governing manifest and posture envelope.

---

## Capstone Deliverables

The repository is intended to demonstrate:

- a clear problem statement;
- a multi-agent architecture;
- a governance model for input treatment;
- an AAI example manifest;
- a mapping between AAI agents and XYZ postures;
- sample governed elements;
- reference implementation logic;
- documentation suitable for review and reuse.

---

## Demonstration Scenario

A demonstration scenario may include:

1. a raw Top of Descent machine log showing repeated avionics messages;
2. a pilot end-of-day report mentioning related symptoms;
3. a maintenance report indicating troubleshooting or resolution status;
4. a normalization step that creates structured event records;
5. a clustering step that links related events over a 60-day window;
6. a health scoring step that identifies an emerging reliability issue;
7. a final report that distinguishes raw evidence from agent-generated conclusions.

XYZ governance ensures that each step preserves the authority and treatment rules of the underlying data.

---

## Expected Value

The AAI system is intended to improve:

- aircraft reliability monitoring;
- early detection of recurring issues;
- maintenance and operations communication;
- auditability of analytical conclusions;
- structured safety assurance;
- human decision support.

The capstone demonstrates how agentic AI can support operational intelligence while maintaining explicit governance over source data, derived reasoning, and final outputs.

---

## Limitations

This capstone is a research and demonstration project. It is not certified for operational aviation use and should not be used as a sole basis for maintenance, safety, dispatch, or airworthiness decisions.

The governance model and scoring outputs are design aids. Human review, domain expertise, and validated operational controls remain required in any high-consequence environment.
