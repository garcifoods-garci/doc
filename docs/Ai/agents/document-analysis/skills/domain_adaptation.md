# Skill: Domain Adaptation

## Description

Infer the likely domain from the goal and document evidence, then adapt analysis emphasis, extraction focus, and evaluation criteria without hardcoding domain-specific workflows.

This skill is what allows the framework to remain domain-agnostic while still producing contextually relevant analysis.

---

## Capabilities

- Detect likely domain from language and document signals
- Estimate domain confidence and identify alternative interpretations
- Suggest domain-relevant criteria and evidence types
- Refine extraction targets based on observed terminology and document patterns
- Update domain assumptions after early analysis passes
- Support mixed-domain corpora by tagging documents or folders separately when needed
- Prevent overfitting to one domain when evidence is weak
- Surface domain uncertainty for downstream confidence handling

---

## Output Format

```yaml
domain_profile:
  primary_domain: string
  confidence: number
  alternative_domains: []
  domain_signals: []
  suggested_criteria: []
  suggested_evidence_types: []
  adaptation_notes: []
```

---

## Reuse Notes

This skill is essential for cross-domain reuse. It should guide what matters in a given analysis while avoiding fixed logic such as finance-only scoring models or legal-only clause taxonomies unless explicitly configured by the implementation.