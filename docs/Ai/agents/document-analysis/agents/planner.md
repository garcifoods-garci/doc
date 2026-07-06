# Planner Agent

## Purpose

Convert the interpreted goal into an executable, adaptive workflow that selects the right analysis steps, skills, sequencing, and depth of review.

The planner is responsible for orchestration logic, not content analysis itself.

---

## Inputs

### Required
- Structured goal interpretation from `goal_interpreter`

### Optional
- Root folder path
- File inventory or folder metadata
- Compute budget
- Priority rules
- Previous run artifacts
- User-defined output format requirements

---

## Outputs

A structured execution plan containing:

- `plan_id`
- `workflow_type`: single-pass, iterative, comparative, risk-first, evidence-first, or hybrid
- `document_scope`: folders, subfolders, file types, and inclusion rules
- `processing_strategy`: batch, incremental, parallel, prioritized, or staged
- `analysis_units`: per document, per entity, per folder, per time period, per theme
- `selected_skills`: parsing, extraction, reasoning, summarization, domain adaptation
- `agent_sequence`: ordered or conditional agent execution steps
- `decision_checkpoints`: moments when the system decides whether more evidence is required
- `confidence_gates`: thresholds for re-analysis, escalation, or early stop
- `comparison_strategy`: if ranking or comparing entities is needed
- `failure_handling`: how to proceed on unreadable files, missing data, or weak confidence
- `final_output_spec`: sections and structure expected from the final report

---

## Responsibilities

1. **Choose workflow shape**
   - Decide whether the task needs simple summarization, comparative analysis, ranking, anomaly detection, or recommendation.

2. **Define scope**
   - Determine what folders, subfolders, and files should be included.

3. **Select analysis granularity**
   - Choose the best unit of work, such as document-level, entity-level, or folder-level analysis.

4. **Assign reusable skills**
   - Specify which skills are required at each stage.

5. **Set checkpoints and gates**
   - Introduce confidence-based branching and retry conditions.

6. **Optimize for scale**
   - Prioritize triage, batching, parallelization, and incremental processing when document collections are large.

7. **Prepare decision readiness criteria**
   - Define what minimum evidence is needed before decision synthesis begins.

---

## Operating Logic

### Step 1: Read Goal Complexity
Infer whether the goal requires:
- a single answer,
- per-entity decisions,
- cross-document comparison,
- temporal trend analysis,
- contradiction detection,
- or iterative evidence gathering.

### Step 2: Determine Analysis Strategy
Examples:
- **Summarization workflow** for “summarize key findings”.
- **Comparative workflow** for “choose the best option”.
- **Risk workflow** for “identify exposure or compliance failures”.
- **Decision workflow** for “decide whether to proceed”.

If comparison, ranking, or prioritization is required, the planner should explicitly schedule the `comparison_agent` between analysis and decisioning.

### Step 3: Define Scan and Prioritization Rules
Use folder structure, file names, metadata, and size to prioritize likely high-value files first.

### Step 4: Build Conditional Flow
Example conditions:
- If domain confidence is low, run early domain validation during analysis.
- If extraction coverage is poor, retry with deeper parsing.
- If evidence conflicts strongly, schedule contradiction resolution before decisioning.

---

## Example

### Input
Structured goal interpretation for: “Decide if these companies are worth investing in.”

### Output
```yaml
plan_id: plan-001
workflow_type: comparative_decision
document_scope:
  include_subfolders: true
  include_file_types: [pdf, docx, xlsx, txt, html]
processing_strategy:
  mode: staged_parallel
  triage_first: true
analysis_units:
  - per_company
  - per_document
selected_skills:
  - document_parsing
  - data_extraction
  - reasoning
  - summarization
  - domain_adaptation
agent_sequence:
  - goal_interpreter
  - document_ingestion
  - analysis_agent:triage_pass
  - analysis_agent:deep_pass
  - comparison_agent
  - decision_agent
  - report_generator
decision_checkpoints:
  - after triage
  - after deep pass
confidence_gates:
  minimum_extraction_coverage: 0.70
  minimum_decision_confidence: 0.65
comparison_strategy:
  compare_entities_on:
    - performance
    - risk
    - trend
    - uncertainty
failure_handling:
  unreadable_files: continue_and_log
  missing_evidence: lower_confidence_and_flag
final_output_spec:
  sections:
    - executive_summary
    - key_insights
    - company_comparison
    - recommendation
    - justification
    - confidence
    - evidence_trace
```

---

## Handoff to Other Agents

- Sends `document_scope` and `processing_strategy` to `document_ingestion`.
- Sends `analysis_units`, `selected_skills`, and checkpoints to `analysis_agent`.
- Sends `comparison_strategy` and normalization expectations to `comparison_agent`.
- Sends decision readiness criteria and comparison strategy to `decision_agent`.
- Sends `final_output_spec` to `report_generator`.

---

## Scalability Considerations

A production planner should support:
- shallow triage before deep analysis,
- parallel ingestion and per-document analysis,
- resumable workflows,
- adaptive chunk sizing,
- selective reprocessing,
- sampling for extremely large corpora,
- evidence indexing for later reuse.

---

## Failure and Recovery Behavior

If the system cannot build a high-confidence plan:
- produce a minimal safe default workflow,
- mark assumptions clearly,
- favor broad ingestion and conservative decision thresholds,
- allow refinement after early document evidence is observed.

---

## Implementation Notes

The planner should emit a machine-executable plan object. In implementation, this can map to workflow engine steps, task queues, or agent invocations.