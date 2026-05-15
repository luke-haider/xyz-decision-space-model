# Aging Aircraft Index Agentic System Overview

## Purpose

The **Aging Aircraft Index (AAI)** agentic system is designed to support aircraft reliability intelligence by collecting, structuring, clustering, and summarizing operational and maintenance signals.

The system is intended to help identify emerging aircraft health concerns before they become obvious through formal discrepancies alone.

AAI is the primary application used for the MIT Applied Agentic AI certificate capstone.

---

## Core Idea

Aircraft reliability information exists across multiple sources:

- machine-generated reports;
- pilot observations;
- maintenance notes;
- service issue records;
- component history;
- MEL references;
- inspection and maintenance program data;
- free-text operational reports.

Each source may contain partial evidence. A single item may not be meaningful by itself, but recurring patterns across time and sources may indicate an emerging reliability issue.

AAI uses an agentic workflow to identify and preserve these signals.

---

## Data Sources

### Top of Descent Reports

Machine-generated reports that capture standardized aircraft system messages at a specific reporting point.

Important governance note: the Top of Descent report is a reporting slice. It should not automatically be interpreted as the time or phase of flight when an issue originated.

### Pilot End-of-Day Reports

Pilot-submitted narrative reports describing operational issues, anomalies, observations, aircraft behavior, or crew experience.

These reports are source evidence and should not be silently rewritten by agents.

### Maintenance End-of-Day Reports

Maintenance-submitted notes describing troubleshooting, inspections, corrective actions, deferred items, or ongoing monitoring.

These reports often carry high authority when determining discrepancy status or resolution state.

### Reference Data

Reference material may include MEL guidance, maintenance program information, component history, service bulletins, vendor information, or internal policy references.

Reference data may have different treatment rules depending on source authority and recency.

---

## Agent Roles

### 1. Data Retrieval Agent

Retrieves source records from operational data stores.

Primary responsibility:

- fetch relevant records;
- preserve raw source values;
- attach source metadata;
- avoid interpreting or modifying source content.

### 2. Normalization Agent

Transforms raw records into a structured event format.

Primary responsibility:

- extract dates, systems, message IDs, ATA references, and source type;
- create normalized event records;
- preserve links back to source records;
- flag uncertainty when fields are inferred.

### 3. Deduplication Agent

Identifies records that may describe the same underlying event.

Primary responsibility:

- compare event date, aircraft, system, message ID, description, and source;
- merge or link duplicates only within declared rules;
- preserve all source references;
- avoid deleting evidence.

### 4. Clustering Agent

Groups related events into reliability clusters.

Primary responsibility:

- identify recurrence patterns;
- group related messages or symptoms;
- create cluster IDs;
- distinguish nuisance patterns from operationally meaningful trends;
- preserve uncertainty when relationships are inferred.

### 5. Health Scoring Agent

Assesses aircraft health based on active clusters, recurrence, discrepancy status, severity, and resolution evidence.

Primary responsibility:

- generate an aircraft health score;
- explain score drivers;
- identify active monitoring concerns;
- distinguish evidence from agent inference.

### 6. Report Generation Agent

Creates the final AAI report.

Primary responsibility:

- summarize aircraft health;
- list active clusters;
- provide evidence trails;
- identify unresolved or monitoring items;
- avoid overstating confidence;
- preserve source references.

### 7. Verification Gate

Reviews the output for governance compliance.

Primary responsibility:

- verify source preservation;
- check that high-consequence claims include evidence;
- flag unsupported conclusions;
- detect source mutation;
- distinguish raw data from derived analysis.

---

## Information Flow

```text
Raw Reports
   │
   ▼
Retrieval Agents
   │
   ▼
Normalized Event Records
   │
   ▼
Deduplication
   │
   ▼
Reliability Clusters
   │
   ▼
Aircraft Health Scoring
   │
   ▼
AAI Report
   │
   ▼
Verification Gate / Human Review
```

---

## Why This Is Agentic

AAI is not a single prompt or one-step summarization task. It is a multi-stage workflow in which different agents perform different reasoning tasks over different types of information.

The system is agentic because it involves:

- role-specialized agents;
- sequential handoffs;
- intermediate structured outputs;
- synthesis across multiple data sources;
- reasoning over time;
- audit and verification steps;
- governance over how each input is treated.

---

## Governance Problem

The main governance risk is that agents may confuse source evidence, interpretation, and conclusions.

Examples:

- A raw machine log may be rewritten as a cleaner description, losing original evidence.
- A pilot observation may be treated as a confirmed maintenance discrepancy.
- An agent-inferred cluster may be treated as a verified failure mode.
- A health score may be presented without evidence or confidence limits.
- Maintenance resolution status may be inferred without an authoritative maintenance record.

The XYZ Decision Space Model addresses this by assigning each element a treatment posture before agent reasoning begins.

---

## Relationship to XYZ Decision Space

AAI uses XYZ to define how each agent may treat each input.

Examples:

| AAI Element | Likely XYZ Treatment |
|---|---|
| Raw TOD machine log | IMMUTABLE or ANCHORED |
| Pilot EOD report | ANCHORED |
| Maintenance EOD report | ANCHORED |
| Normalized event record | NEGOTIABLE or AGENT_OWNED |
| Cluster assignment | AGENT_OWNED |
| ATA classification | NEGOTIABLE |
| Discrepancy status | ANCHORED if maintenance-sourced; NEGOTIABLE if inferred |
| Aircraft health score | AGENT_OWNED |
| Published AAI report | ANCHORED |

The goal is not to prevent agents from reasoning. The goal is to ensure that agents reason within the correct authority boundaries.

---

## Capstone Demonstration Value

AAI demonstrates applied agentic AI in a real operational context.

It shows:

- data ingestion;
- retrieval;
- normalization;
- deduplication;
- clustering;
- scoring;
- report generation;
- governance;
- verification;
- human oversight.

This makes it a strong capstone example because it combines technical architecture with operational relevance and governance discipline.

---

## Disclaimer

This system overview is for research, education, and reference implementation purposes. It is not a certified aviation maintenance, dispatch, safety, or airworthiness system.
