# Aging Aircraft Index Example

## Overview

This folder contains an example implementation context for applying the XYZ Decision Space Model to the **Aging Aircraft Index (AAI)** agentic system.

AAI is the primary application for the MIT Applied Agentic AI certificate capstone. It is an aviation reliability-intelligence workflow that uses multiple agents to retrieve, normalize, deduplicate, cluster, score, and report aircraft health signals.

---

## Purpose of This Example

This example shows how XYZ can be used to govern semantic input treatment in a multi-agent aircraft reliability workflow.

The goal is to define:

- which data elements must be preserved as source evidence;
- which elements may be interpreted but not rewritten;
- which elements are agent-generated analytical products;
- which elements require staleness checks;
- which elements require audit trails or verification gates.

---

## AAI Workflow

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

---

## Key Agents

| Agent | Role |
|---|---|
| Retrieval Agent | Fetches source records and preserves raw data |
| Normalization Agent | Converts raw reports into structured event records |
| Deduplication Agent | Links records that may describe the same event |
| Clustering Agent | Groups related events into reliability clusters |
| Health Scoring Agent | Produces aircraft health assessment |
| Report Generation Agent | Produces human-readable AAI report |
| Verification Gate | Checks source preservation, evidence support, and governance compliance |

---

## Key Data Elements

| Element | Treatment Concern |
|---|---|
| Raw TOD machine log | Must preserve source value |
| Pilot EOD report | May be interpreted but not silently rewritten |
| Maintenance EOD report | High-authority source for action and resolution status |
| Source timestamp | Required for recurrence analysis |
| Aircraft identifier | Must not be altered |
| Message ID | Must remain traceable |
| Normalized event record | Agent-generated structured output |
| Cluster ID | Agent-generated grouping |
| Discrepancy status | Must distinguish maintenance-confirmed vs inferred |
| Health score | Agent-generated analytical product requiring evidence |
| Final report | Must preserve source references and confidence boundaries |

---

## Files

| File | Description |
|---|---|
| `manifest.yaml` | Example governing manifest for AAI |
| `sample-elements.json` | Optional future sample input elements |
| `sample-posture-envelopes.json` | Optional future sample governed payloads |

---

## Capstone Relevance

This example supports the MIT capstone by demonstrating:

- a real multi-agent workflow;
- operationally meaningful data;
- governance over source evidence and derived analysis;
- use of posture envelopes;
- explicit distinction between raw data, anchored context, and agent-generated conclusions;
- placement of verification gates based on governance pressure.

---

## Disclaimer

This example is for research and demonstration only. It is not certified for aviation maintenance, airworthiness, dispatch, or safety decision-making.
