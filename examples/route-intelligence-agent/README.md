# Route Intelligence Agent Concept Driver

## Overview

This folder preserves the Route Intelligence Agent (RIA) context that originally motivated parts of the XYZ Decision Space Model.

RIA is not the primary MIT capstone application. The capstone application is the Aging Aircraft Index (AAI) agentic system. RIA remains useful as a secondary aviation proof-of-concept because it clearly illustrates the need for semantic immutability across multi-agent handoffs.

---

## Why RIA Matters to XYZ

The route-intelligence use case exposed a specific failure mode:

A downstream agent may receive a filed ATC route, FIR sequence, or route structure as plain text and treat it as something it can optimize, substitute, expand, or reinterpret.

In aviation route analysis, some elements must remain fixed:

- filed ATC route;
- flight level;
- destination;
- FIR sequence;
- transit FIR classification;
- route structure.

Other elements may be contextual, negotiable, or agent-generated:

- adjacent FIR risk context;
- diversion candidates;
- airport suitability;
- medical facility ranking;
- security risk scoring;
- policy recommendations.

XYZ was developed in part to declare these differences before reasoning begins.

---

## Example Failure Mode

A route-analysis agent receives a FIR sequence and identifies nearby adjacent FIRs with relevant security concerns. Without explicit treatment rules, the agent may promote adjacent FIRs into the transit FIR sequence or treat the filed route as negotiable.

This is not necessarily poor reasoning. The agent may be trying to improve the analysis.

The failure is that the system never declared that the original route structure was semantically fixed.

---

## XYZ Treatment

In RIA, a filed ATC route may receive the following coordinate binding:

| Element | X | Y | Z | Posture |
|---|---:|---:|---:|---|
| Filed ATC route | -1.0 | +1.0 | 1.0 | IMMUTABLE |
| FIR sequence | -1.0 | +1.0 | 1.0 | IMMUTABLE |
| Adjacent FIR context | -0.2 | +0.5 | 0.6 | NEGOTIABLE |
| Diversion candidates | +0.6 | -0.4 | 0.7 | AGENT_OWNED |
| Airport medical capability | +0.4 | -0.3 | 0.9 | NEGOTIABLE |

The immutable route elements may be used as fixed inputs, but they may not be reinterpreted, substituted, expanded, or optimized by the agent.

---

## Compliance Checks

RIA illustrates several semantic compliance checks that are useful for XYZ:

| Check | Purpose |
|---|---|
| `no_substitution` | Prevent replacing the filed route with another route |
| `no_expansion` | Prevent expanding a FIR sequence by inference |
| `no_promotion` | Prevent adjacent FIRs from being promoted into transit FIRs |
| `no_optimization` | Prevent treating a fixed route as an efficiency preference |
| `preserve_route_structure` | Preserve the original route structure through handoffs |

---

## Relationship to AAI

RIA and AAI both involve multi-agent aviation workflows, but they emphasize different governance problems.

| System | Primary Governance Challenge |
|---|---|
| RIA | Preserve fixed route constraints across agents |
| AAI | Preserve source evidence while allowing structured inference, clustering, scoring, and reporting |

RIA helped reveal the original problem. AAI is the broader capstone application where XYZ governs a complete data lifecycle.

---

## Repository Role

This folder should be treated as supporting context for the XYZ model. It demonstrates how the same framework can apply to another aviation agentic system, while the AAI folder remains the primary capstone example.

---

## Disclaimer

This example is for research and conceptual demonstration only. It is not an operational route-planning, dispatch, safety, or security system.
