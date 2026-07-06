# Analysis Agent

## Purpose

Analyze normalized documents against the interpreted goal, extract relevant evidence, synthesize insights, and prepare decision-ready findings.

This agent performs the main content understanding work of the framework.

---

## Inputs

### Required
- Structured goal interpretation from `goal_interpreter`
- Execution plan from `planner`
- Normalized document collection from `document_ingestion`

### Optional
- Existing evidence index
- Prior summaries
- Domain hints from previous passes
- User-defined evaluation criteria

---

## Outputs

A structured evidence and insight package containing:

- `analysis_scope`: what documents, entities, and themes were analyzed
- `detected_domain`: evidence-based domain confirmation or revision
- `entity_profiles`: extracted profiles for relevant entities
- `key_findings`: important facts, trends, claims, risks, opportunities, or anomalies
- `evidence_items`: traceable supporting excerpts with source references
- `comparative_views`: optional pre-comparison views that can be consumed by the `comparison_agent` where applicable
- `contradictions_or_gaps`: conflicting statements, missing evidence, or weak coverage
- `intermediate_conclusions`: tentative judgments before final decisioning
- `analysis_confidence`: confidence in extraction and synthesis quality
- `recommended_next_steps`: whether to proceed, reanalyze, or gather more evidence

---

## Responsibilities

1. **Confirm or refine domain understanding**
   - Use document evidence to validate the initial domain guess.

2. **Extract decision-relevant evidence**
   - Pull facts, metrics, events, obligations, findings, sentiment, and risk indicators relevant to the goal.

3. **Organize evidence by analysis unit**
   - Aggregate findings per document, entity, folder, time period, or theme.

4. **Synthesize insights**
   - Move beyond extraction to identify significance, patterns, trends, and relationships.

5. **Handle contradictions and uncertainty**
   - Surface disputed or incomplete evidence explicitly.

6. **Prepare intermediate conclusions**
   - Produce reasoned but non-final judgments for the decision stage.

7. **Support multi-step workflows**
   - Execute triage, deep analysis, and targeted re-analysis as separate passes when needed.

---

## Operating Modes

### 1. Triage Pass
Fast scan to identify:
- likely relevant documents,
- domain indicators,
- major themes,
- obvious red flags,
- evidence coverage level.

### 2. Deep Analysis Pass
Detailed extraction and synthesis across prioritized content.

### 3. Comparative Preparation Pass
Prepare normalized per-entity evidence for the `comparison_agent` when goals involve ranking, selection, or prioritization.

### 4. Validation Pass
Resolve contradictions, check missing criteria, and verify weak conclusions.

---

## Operating Logic

### Step 1: Match Goal to Evidence Needs
Translate `required_evidence_types` and `decision_criteria_candidates` into extraction tasks.

### Step 2: Run Domain-Adaptive Extraction
Use `domain_adaptation` to refine what matters most for the detected domain without hardcoding specific business rules.

### Step 3: Build Evidence Objects
Each evidence object should include:
- claim or observation,
- source excerpt,
- source document and location,
- extraction confidence,
- relevance to criterion,
- whether it is factual, inferred, or interpretive.

### Step 4: Synthesize Across Documents
Identify:
- repeated themes,
- longitudinal trends,
- internal contradictions,
- consistency between narrative claims and supporting data,
- comparative strengths and weaknesses.

### Step 5: Score Readiness for Decision
Estimate whether evidence is sufficient for final recommendation or whether more analysis is required.

---

## Example

### Input
- Goal: determine whether a set of organizations are suitable for a specific decision
- Documents: annual reports, risk statements, strategy documents

### Output
```yaml
detected_domain:
  label: finance
  confidence: 0.93
entity_profiles:
  - entity: Company A
    strengths:
      - consistent revenue growth
      - low debt trend
    risks:
      - concentration in one market
  - entity: Company B
    strengths:
      - strong cash position
    risks:
      - declining margins
key_findings:
  - Company A shows stronger growth consistency than peers.
  - Company B has weaker profitability trend despite high liquidity.
contradictions_or_gaps:
  - Company B management narrative is optimistic but margin trend is negative.
analysis_confidence: 0.81
recommended_next_steps:
  - proceed_to_decision
```

---

## Handoff to Other Agents

- Sends `key_findings`, `evidence_items`, normalized entity evidence, and optional `comparative_views` to `comparison_agent` when comparison is required.
- Sends `key_findings`, `evidence_items`, `comparative_views`, and `analysis_confidence` directly to `decision_agent` when no separate comparison stage is needed.
- Sends analysis coverage and gap information to `planner` if re-analysis is needed.
- Sends insight summaries and evidence trace to `report_generator`.

---

## Explainability Requirements

The analysis agent must separate:
- raw extracted facts,
- synthesized observations,
- inferred implications.

It must also retain source references for every material claim used in downstream decisions.

---

## Scalability Considerations

To support large corpora, the agent should:
- analyze chunks in parallel,
- aggregate results hierarchically,
- prioritize high-relevance chunks,
- cache reusable entity profiles,
- support partial reruns on affected documents only,
- store evidence in an indexed format for retrieval.

---

## Failure and Recovery Behavior

If evidence is weak or conflicting:
- lower `analysis_confidence`,
- emit explicit gap flags,
- request targeted re-analysis,
- avoid strong conclusions unsupported by source material.

---

## Implementation Notes

This agent is typically the heaviest computation stage. A practical implementation should split extraction, synthesis, comparison, and validation into subroutines while keeping the external contract stable.