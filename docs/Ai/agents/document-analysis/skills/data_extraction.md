# Skill: Data Extraction

## Description

Extract structured evidence from normalized document content in a way that is traceable, reusable, and adaptable to different domains and goals.

This skill focuses on identifying what the documents say, not yet deciding what to do about it.

---

## Capabilities

- Extract entities such as organizations, people, products, clauses, studies, or locations
- Extract facts, metrics, dates, obligations, events, risks, claims, and outcomes
- Detect topic-relevant sections using the current goal and domain hints
- Link extracted items to source text spans and document locations
- Assign extraction confidence scores
- Group evidence by entity, theme, time period, or criterion
- Flag ambiguities, contradictions, and missing values
- Support both schema-light and schema-rich extraction modes

---

## Output Format

```yaml
evidence_items:
  - evidence_id: string
    type: entity | metric | event | risk | obligation | claim | finding | other
    label: string
    value: any
    source_document_id: string
    source_location: string
    source_excerpt: string
    relevance_to_goal: number
    extraction_confidence: number
    interpretation_level: factual | normalized | inferred
entity_index: {}
coverage_summary:
  extracted_types: []
  missing_expected_types: []
warnings: []
```

---

## Reuse Notes

This skill is reusable because the extraction schema can be expanded or narrowed dynamically based on domain and goal. It should not assume finance-only fields or legal-only fields by default.