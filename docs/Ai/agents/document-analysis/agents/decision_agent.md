# Decision Agent

## Purpose

Transform analyzed evidence into a direct recommendation, decision, ranking, or judgment that answers the user's goal while making uncertainty and trade-offs explicit.

This agent is responsible for conclusion formation, not raw extraction.

---

## Inputs

### Required
- Structured goal interpretation from `goal_interpreter`
- Execution plan from `planner`
- Evidence and insights from `analysis_agent`

### Optional
- Comparison package from `comparison_agent` when the goal involves multiple entities or alternatives
- User constraints such as risk tolerance or preferred criteria weighting
- Organizational policy thresholds
- Historical benchmark data

---

## Outputs

A structured decision package containing:

- `decision_statement`: concise answer to the user goal
- `recommendation_set`: one or more recommended actions or ranked options
- `justification`: evidence-backed explanation
- `decision_factors`: major criteria that drove the outcome
- `trade_offs`: explicit benefits, risks, and compromises
- `confidence_score`: numeric or categorical score
- `confidence_drivers`: why confidence is high or low
- `assumptions`: inferred but unverified premises
- `limitations`: data gaps, parsing issues, domain ambiguity, conflicting evidence
- `alternative_outcomes`: plausible alternatives if assumptions change
- `next_actions`: optional follow-up steps

---

## Responsibilities

1. **Answer the goal directly**
   - Produce a usable decision rather than a generic summary.

2. **Apply evidence-based judgment**
   - Ground recommendations in extracted findings and, when available, structured comparative analysis from the `comparison_agent`.

3. **Balance multiple criteria**
   - Handle competing signals such as opportunity versus risk or cost versus compliance.

4. **Quantify confidence**
   - Estimate confidence based on evidence quality, consistency, completeness, and domain certainty.

5. **Expose assumptions and limitations**
   - Make hidden dependencies explicit.

6. **Support nuanced outcomes**
   - Allow outputs such as approve, reject, conditional approve, rank with caveats, or insufficient evidence.

---

## Operating Logic

### Step 1: Align to Success Definition
Use `success_definition` from the goal interpretation to determine what counts as a complete answer.

### Step 2: Evaluate Evidence Against Criteria
Map findings to decision criteria. Where a comparison package exists, incorporate normalized rankings, strengths, weaknesses, and relative risks. Where criteria weights are unknown, use transparent default balancing and state this in assumptions.

### Step 3: Resolve Conflicts Conservatively
If evidence conflicts:
- prefer well-supported and traceable evidence,
- reduce confidence,
- present alternative interpretations when needed.

### Step 4: Form Recommendation
Generate one of the following patterns depending on goal type:
- binary decision,
- ranked list,
- scored options,
- conditional recommendation,
- defer due to insufficient evidence.

### Step 5: Compute Confidence
Confidence should consider:
- coverage of required evidence,
- consistency of findings,
- parsing quality,
- domain certainty,
- strength of reasoning chain,
- number of unresolved ambiguities.

---

## Example

### Input
Evidence package comparing multiple entities for a user decision goal.

### Output
```yaml
decision_statement: Company A appears to be the strongest candidate based on the available document set.
recommendation_set:
  - entity: Company A
    recommendation: favorable
  - entity: Company B
    recommendation: caution
justification:
  - Company A shows stronger trend consistency and fewer material risk indicators.
  - Company B has liquidity strength but weaker profitability trajectory.
decision_factors:
  - performance trend
  - risk exposure
  - evidence consistency
trade_offs:
  - Company A offers stronger growth but some concentration risk.
  - Company B appears safer on cash but weaker on earnings quality.
confidence_score: 0.76
confidence_drivers:
  positive:
    - strong document coverage
    - consistent evidence across multiple files
  negative:
    - no external market data in corpus
assumptions:
  - decision based only on supplied documents
limitations:
  - investment horizon unspecified
alternative_outcomes:
  - With a low-risk preference, Company B may become more acceptable despite lower upside.
next_actions:
  - review latest non-document market signals before final execution
```

---

## Handoff to Other Agents

- Sends complete decision package to `report_generator`.
- May incorporate rankings and comparative trade-offs from `comparison_agent` into the final recommendation set.
- Optionally sends low-confidence reasons back to `planner` if another analysis loop is warranted.

---

## Explainability Requirements

The decision agent must clearly separate:
- evidence,
- interpretation,
- recommendation,
- uncertainty.

A final decision without cited supporting factors is invalid under this framework.

---

## Confidence Scoring Guidance

A practical scoring model may combine:
- evidence coverage,
- evidence consistency,
- source reliability,
- parse quality,
- domain confidence,
- goal-criteria match.

Implementations may use a weighted formula, rubric, or model-based estimator, but the contributing factors must be visible in the output.

---

## Failure and Recovery Behavior

If evidence is insufficient for a reliable decision:
- return `insufficient_evidence` or a conditional recommendation,
- lower confidence score,
- specify missing information,
- recommend targeted next analysis steps.

---

## Implementation Notes

The decision agent should remain policy-light and domain-flexible. Any organization-specific thresholds or risk tolerances should be passed in as inputs rather than embedded in its default logic.