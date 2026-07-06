# Multi-Agent Document Analysis Framework

## Overview

This framework is a production-ready blueprint for analyzing large document collections from folder structures using a loosely coupled, multi-agent system. It accepts only a high-level user goal and autonomously:

1. interprets the goal,
2. detects the likely domain,
3. builds an execution plan,
4. loads and parses documents from nested folders,
5. extracts evidence and insights,
6. compares entities or alternatives when needed,
7. makes a recommendation or decision,
8. generates a structured report with justification and confidence.

The design is domain-agnostic. It can analyze financial reports, contracts, policy documents, research papers, operational documents, compliance records, customer feedback archives, or mixed collections.

---

## Example Goal

> Decide if these companies are worth investing in.

The system should infer that the likely domain is finance, identify relevant document sets such as annual reports or earnings materials, extract decision-relevant evidence, compare alternatives if needed, and produce a recommendation with rationale and confidence.

---

## Design Principles

- **Goal-first operation**: the only required input is a natural-language goal.
- **Domain adaptation**: domain assumptions are inferred dynamically from the goal and document evidence.
- **Loose coupling**: agents communicate via structured inputs and outputs rather than direct internal dependencies.
- **Scalable execution**: document ingestion, parsing, extraction, and analysis can run in parallel across folders and files.
- **Explainable decisions**: every recommendation must be linked to explicit evidence and reasoning.
- **Reusable skills**: parsing, extraction, reasoning, summarization, and adaptation skills are shared across agents.
- **Production-oriented**: supports large datasets, partial failures, iterative refinement, and auditable outputs.

---

## Folder Structure

```text
/
├─ README.md
├─ agents/
│  ├─ goal_interpreter.md
│  ├─ planner.md
│  ├─ document_ingestion.md
│  ├─ analysis_agent.md
│  ├─ comparison_agent.md
│  ├─ decision_agent.md
│  └─ report_generator.md
└─ skills/
   ├─ document_parsing.md
   ├─ data_extraction.md
   ├─ reasoning.md
   ├─ summarization.md
   └─ domain_adaptation.md
```

---

## End-to-End Workflow

### 1. Goal Intake
The user provides a high-level objective such as:
- Decide which vendors are safest to onboard.
- Identify legal risks across these contracts.
- Determine whether these companies are worth investing in.
- Summarize the key scientific findings and recommend next actions.

### 2. Goal Interpretation
The **Goal Interpreter** converts the raw goal into:
- decision type,
- analysis intent,
- success criteria,
- likely domain,
- required evidence types,
- constraints and ambiguities.

### 3. Planning
The **Planner** creates a dynamic workflow based on:
- goal complexity,
- domain confidence,
- folder structure,
- document volume,
- required output format,
- comparison or ranking needs.

### 4. Document Loading and Normalization
The **Document Ingestion Agent** recursively scans folders and subfolders, identifies processable files, extracts text and metadata, and produces normalized document units for downstream analysis.

### 5. Analysis
The **Analysis Agent** applies reusable skills to:
- parse documents,
- extract entities, facts, metrics, events, claims, and risks,
- summarize findings,
- identify contradictions or gaps,
- connect evidence to the goal.

### 6. Comparison
The **Comparison Agent** compares entities, options, or document groups when the goal requires ranking, prioritization, or selection. It produces:
- comparative analysis,
- rankings or tiers,
- strengths and weaknesses per entity,
- normalized cross-entity views,
- comparison confidence.

### 7. Decisioning
The **Decision Agent** synthesizes evidence into:
- recommendations,
- alternatives,
- trade-offs,
- confidence score,
- justification linked to evidence.

### 8. Report Generation
The **Report Generator** creates a final structured report containing:
- executive summary,
- key insights,
- decision or recommendation,
- justification,
- confidence,
- assumptions,
- evidence trace,
- optional appendix by document or folder.

---

## Core Inputs

### Required
- **Goal**: a natural-language statement of what the system should decide, assess, compare, summarize, or recommend.

### Optional but Useful
- Root document folder path
- Output preferences
- Time or compute budget
- Required confidence threshold
- User constraints such as risk tolerance or ranking criteria

The framework remains functional with only the goal and a document repository.

---

## Standard Outputs

Every full run should produce the following result categories:

### Key Insights
A concise list of the most decision-relevant findings.

### Decision or Recommendation
A direct answer to the user goal.

### Justification
Explicit reasoning supported by extracted evidence.

### Confidence Score
A numeric or categorical estimate based on evidence coverage, consistency, document quality, and domain certainty.

### Structured Report
A machine-readable and human-readable report suitable for review, audit, or downstream automation.

---

## Orchestration Model

The system should use an orchestration layer that manages agents as modular workers.

### Recommended Execution Pattern
1. Run `goal_interpreter`.
2. Run `planner`.
3. Run `document_ingestion` according to plan.
4. Run one or more `analysis_agent` passes:
   - per document,
   - per folder,
   - per topic,
   - per entity,
   - per comparison set.
5. Run `comparison_agent` when the goal involves ranking, selection, prioritization, or cross-entity evaluation.
6. Run `decision_agent` once enough evidence is assembled.
7. Run `report_generator` to produce final output.

### Dynamic Behavior
The orchestrator may:
- branch workflows for different document groups,
- retry low-confidence extraction,
- escalate ambiguity back to planning,
- run comparative analyses across subfolders,
- stop early if evidence is insufficient and report limitations.

---

## Scalability Guidance

To support large datasets, implementations should:
- scan folders incrementally,
- store document metadata and parsing outputs in an index,
- chunk long documents,
- parallelize ingestion and analysis,
- cache repeated extraction results,
- use confidence thresholds to prioritize deeper analysis,
- separate lightweight triage from heavy reasoning,
- preserve provenance for every extracted claim.

---

## Explainability Requirements

A production implementation should ensure that:
- every major conclusion maps to one or more evidence items,
- inferred domain labels are explicit and revisable,
- uncertainty is surfaced rather than hidden,
- assumptions are recorded,
- confidence is computed from transparent factors,
- the final report distinguishes facts, inferences, and recommendations.

---

## Failure Handling

The framework should degrade gracefully when:
- documents are missing or unreadable,
- the domain is unclear,
- evidence is conflicting,
- folder structure is inconsistent,
- extraction confidence is low.

In such cases, the system should still produce:
- partial findings,
- an uncertainty summary,
- recommended next steps,
- a lower confidence score.

---

## Implementation Notes

This blueprint is intentionally tool-agnostic. It can be implemented using:
- language models,
- OCR and parsing services,
- vector or keyword retrieval,
- workflow engines,
- metadata stores,
- batch or streaming pipelines.

The recommended contract between components is structured JSON-like data, even when agents are represented as prompts or services.

---

## File Guide

### Agents
- `agents/goal_interpreter.md` — converts raw goals into actionable analysis intent.
- `agents/planner.md` — creates the workflow and allocates analysis steps.
- `agents/document_ingestion.md` — loads, parses, normalizes, and indexes documents.
- `agents/analysis_agent.md` — extracts insights and evidence relevant to the goal.
- `agents/comparison_agent.md` — compares entities, alternatives, and trade-offs when needed.
- `agents/decision_agent.md` — converts evidence into recommendations and confidence.
- `agents/report_generator.md` — composes the final structured output.

### Skills
- `skills/document_parsing.md` — file-to-text normalization and structural parsing.
- `skills/data_extraction.md` — reusable fact, entity, metric, and risk extraction.
- `skills/reasoning.md` — evidence synthesis, comparison, and decision logic.
- `skills/summarization.md` — concise multi-level summaries for documents and results.
- `skills/domain_adaptation.md` — dynamic domain detection and criteria adaptation.

---

## Recommended Final Report Sections

1. Goal
2. Detected Domain
3. Scope and Coverage
4. Method Summary
5. Key Insights
6. Decision or Recommendation
7. Justification
8. Confidence Score
9. Risks and Uncertainties
10. Evidence Trace
11. Assumptions
12. Next Actions

---

## Success Criteria

The framework is successful if it can:
- operate from only a high-level goal,
- work across domains without hardcoded business logic,
- scale to multi-folder document sets,
- produce explainable decisions,
- remain modular enough to swap tools, prompts, or models without redesign.
