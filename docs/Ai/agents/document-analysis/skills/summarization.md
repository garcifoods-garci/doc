# Skill: Summarization

## Description

Generate concise, accurate summaries at multiple levels of granularity, from document chunks to final reports, while retaining the most goal-relevant information.

---

## Capabilities

- Summarize individual chunks, full documents, folders, entities, themes, or final system outputs
- Create executive summaries and detailed analytical summaries
- Prioritize information based on the current goal
- Preserve important caveats and uncertainty markers
- Compress repeated findings across multiple documents
- Produce bullet, paragraph, or section-based summaries
- Support layered summarization for large corpora

---

## Output Format

```yaml
summary:
  level: chunk | document | folder | entity | corpus | final_report
  audience: analyst | executive | general | custom
  text: string
  key_points: []
  caveats: []
  source_coverage:
    documents_used: []
    omitted_or_low_quality: []
```

---

## Reuse Notes

This skill is reusable across domains because it depends on relevance and evidence quality rather than domain-specific templates. Domain adaptation may influence what is emphasized, but not the core output contract.