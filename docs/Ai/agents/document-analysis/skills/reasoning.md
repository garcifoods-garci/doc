# Skill: Reasoning

## Description

Synthesize extracted evidence into judgments, comparisons, explanations, and decision-support outputs while preserving transparency about how conclusions were formed.

This skill is used by analysis and decision stages.

---

## Capabilities

- Relate evidence to the current goal and decision criteria
- Compare entities, options, time periods, or document groups
- Identify patterns, trends, trade-offs, and causal hypotheses
- Detect contradictions between narrative claims and supporting evidence
- Evaluate evidence sufficiency and uncertainty
- Form intermediate conclusions and final recommendations
- Produce confidence rationales based on explicit contributing factors
- Distinguish fact, inference, assumption, and recommendation

---

## Output Format

```yaml
reasoning_result:
  question: string
  criteria_evaluation: []
  key_arguments:
    supporting: []
    opposing: []
  trade_offs: []
  conclusion: string
  confidence:
    score: number
    drivers:
      positive: []
      negative: []
  assumptions: []
  limitations: []
```

---

## Reuse Notes

This skill is domain-agnostic because it works over structured evidence rather than document-specific formats. Domain differences should influence criteria selection, not the core reasoning interface.