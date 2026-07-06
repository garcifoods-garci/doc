# Report Generator Agent

## Purpose

Convert the outputs of all upstream agents into a clear, structured, human-readable and machine-friendly final report.

This agent is responsible for presentation, traceability, and completeness of the final deliverable.

---

## Inputs

### Required
- Structured goal interpretation from `goal_interpreter`
- Execution plan from `planner`
- Ingestion summary from `document_ingestion`
- Analysis outputs from `analysis_agent`
- Decision package from `decision_agent`

### Optional
- Comparison package from `comparison_agent`
- Reporting template
- Output format preferences
- Audience type such as executive, analyst, legal reviewer, or general user

---

## Outputs

A structured report containing:

- `goal`
- `detected_domain`
- `scope_and_coverage`
- `method_summary`
- `key_insights`
- `decision_or_recommendation`
- `justification`
- `confidence_score`
- `confidence_explanation`
- `assumptions`
- `limitations`
- `evidence_trace`
- `next_actions`
- optional appendices

The report may be emitted as:
- Markdown
- JSON
- HTML
- PDF-ready structured content

---

## Responsibilities

1. **Assemble the final narrative**
   - Present a coherent answer from modular upstream outputs.

2. **Preserve explainability**
   - Ensure every major conclusion is backed by evidence and confidence explanation.

3. **Represent uncertainty honestly**
   - Highlight assumptions, missing data, and unresolved contradictions.

4. **Support different audiences**
   - Keep a stable core structure while allowing summary depth to vary.

5. **Maintain structured sections**
   - Make output easy to review, compare, and audit.

---

## Recommended Report Structure

### 1. Goal
Restate the user objective in clear terms.

### 2. Detected Domain
State the inferred domain and confidence.

### 3. Scope and Coverage
Summarize folders scanned, documents analyzed, file types covered, and any missing or unreadable items.

### 4. Method Summary
Briefly describe how the system interpreted the goal, ingested documents, analyzed evidence, and reached a decision.

### 5. Key Insights
List the most important findings in concise bullets.

### 6. Comparative Analysis
When applicable, present rankings, strengths, weaknesses, and notable trade-offs across entities or options.

### 7. Decision or Recommendation
Provide the direct answer to the user goal.

### 8. Justification
Explain why the recommendation was made, tied to evidence and decision factors.

### 9. Confidence Score
Provide a score and explain what influenced it.

### 10. Risks, Limitations, and Assumptions
Surface uncertainty, data gaps, and interpretation constraints.

### 11. Evidence Trace
Include source references for major claims.

### 12. Next Actions
Suggest follow-up actions or verification steps if appropriate.

---

## Example

### Input
Decision package and supporting analysis for a multi-company evaluation.

### Output
```markdown
# Analysis Report

## Goal
Determine whether the companies in the provided folders appear worth investing in.

## Detected Domain
Finance, confidence 0.89.

## Scope and Coverage
- Folders scanned: 4
- Documents analyzed: 28
- Unreadable files: 2

## Key Insights
- Company A shows stronger consistency in performance indicators.
- Company B has stronger liquidity but weaker margin trajectory.
- Evidence quality is good overall but external market context is absent.

## Recommendation
Company A is the strongest candidate in the available corpus.

## Justification
This recommendation is based on stronger trend consistency, fewer severe risk indicators, and better alignment between narrative claims and document evidence.

## Confidence
0.76 — reduced by missing external context and some document gaps.
```

---

## Handoff and Consumption

The report generator is typically the terminal stage, but its output can also feed:
- dashboards,
- audit workflows,
- approval pipelines,
- case management systems,
- retrieval systems for later review.

---

## Quality Rules

The generated report should:
- answer the goal directly,
- avoid unsupported claims,
- distinguish facts from interpretations,
- include confidence and uncertainty,
- remain readable for non-technical users,
- retain enough structure for machine consumption.

---

## Scalability Considerations

For large analyses, the report generator should support:
- layered summaries,
- per-entity appendices,
- collapsible evidence sections,
- chunked report generation,
- standardized report schemas for downstream systems.

---

## Failure and Recovery Behavior

If upstream outputs are incomplete:
- generate the best possible partial report,
- include an explicit incompleteness notice,
- show which sections are low-confidence or unavailable,
- recommend next steps rather than suppressing issues.

---

## Implementation Notes

This agent should treat report composition as deterministic formatting over structured inputs wherever possible. That improves consistency, auditability, and integration with external systems.