# Comparison Agent

## Purpose

Compare multiple entities based on analyzed data so the system can support ranking, selection, prioritization, or trade-off evaluation.

This agent is used when the user goal involves choosing among alternatives, identifying leaders and laggards, or understanding relative strengths and weaknesses across entities.

---

## Inputs

### Required
- Structured insights from multiple entities
- Metrics and extracted data

### Optional
- Comparison criteria from `goal_interpreter`
- Comparison strategy from `planner`
- Domain profile from `analysis_agent`
- User weighting preferences or constraints

---

## Outputs

A structured comparison package containing:

- `comparison_scope`: entities, groups, or document sets included
- `normalized_comparison_frame`: normalized criteria and comparable values
- `comparative_analysis`: cross-entity findings and patterns
- `rankings`: ranked entities or grouped tiers when ranking is appropriate
- `strengths_and_weaknesses`: strengths and weaknesses per entity
- `relative_risks`: notable relative risks, limitations, or caveats
- `comparison_confidence`: confidence in the comparison quality
- `comparison_notes`: assumptions, normalization choices, and unresolved comparability issues

---

## Responsibilities

1. **Normalize data across entities**
   - Align extracted evidence into a comparable structure even when documents differ in format, terminology, or completeness.

2. **Compare key metrics and signals**
   - Evaluate entities across the most relevant dimensions for the current goal.

3. **Highlight top performers and underperformers**
   - Identify where one entity outperforms, underperforms, or differs materially from peers.

4. **Support domain-agnostic comparisons**
   - Compare financial, legal, operational, research, compliance, or general document outcomes without assuming finance-only logic.

5. **Surface comparability limits**
   - Flag when entities are difficult to compare due to missing data, different reporting styles, inconsistent time windows, or weak evidence.

6. **Prepare decision-ready outputs**
   - Deliver ranked and justified comparisons that downstream decisioning can use directly.

---

## Operating Logic

### Step 1: Build a Comparison Frame
Create a normalized set of criteria using:
- goal-driven criteria,
- domain-adapted evidence types,
- comparable time periods or scopes where possible.

### Step 2: Normalize Evidence
Standardize metrics, claims, and risk indicators into comparable units or categorical bands.

### Step 3: Compare Across Entities
Identify:
- leaders,
- underperformers,
- outliers,
- clusters,
- trade-offs,
- consistency differences.

### Step 4: Produce Ranking or Tiering
If a strict ranking is appropriate, order entities. If not, produce tiers or scenario-based recommendations.

### Step 5: Estimate Comparison Confidence
Confidence should reflect:
- data completeness,
- consistency of definitions,
- evidence quality,
- domain certainty,
- fairness of normalization.

---

## Example (Finance)

### Compare companies based on:
- Revenue growth
- Profit margins
- Debt levels
- Cash flow stability

### Example Output
```yaml
comparison_scope:
  entities:
    - Company A
    - Company B
    - Company C
comparative_analysis:
  - Company A leads on growth consistency.
  - Company B has the strongest cash position but weaker margins.
  - Company C carries the highest leverage risk.
rankings:
  - rank: 1
    entity: Company A
  - rank: 2
    entity: Company B
  - rank: 3
    entity: Company C
strengths_and_weaknesses:
  Company A:
    strengths:
      - strong revenue growth
      - stable cash flow
    weaknesses:
      - moderate concentration risk
  Company B:
    strengths:
      - strong liquidity
    weaknesses:
      - declining profitability
comparison_confidence: 0.79
comparison_notes:
  - Ranking is based only on supplied documents.
  - Some debt disclosures were less detailed for Company C.
```

---

## Notes

- Must work across domains such as finance, legal, research, operations, procurement, or compliance.
- Should not assume finance-only logic.
- Should support both quantitative and qualitative comparisons.
- Should prefer tiering or scenario-based comparison when strict ranking would be misleading.

---

## Handoff to Other Agents

- Receives multi-entity evidence from `analysis_agent`.
- Receives comparison strategy from `planner`.
- Sends comparison outputs to `decision_agent` for recommendation synthesis.
- Sends rankings, strengths, weaknesses, and comparison notes to `report_generator`.

---

## Failure and Recovery Behavior

If comparison quality is weak:
- lower `comparison_confidence`,
- explain why entities are not fully comparable,
- avoid forced rankings,
- recommend additional analysis or normalized criteria refinement.

---

## Implementation Notes

This agent should remain modular and optional. It should be invoked only when the goal or plan requires comparison, ranking, prioritization, or cross-entity evaluation.