# Skill: Document Parsing

## Description

Convert raw files of varying types into normalized text and structural representations that downstream agents can analyze consistently.

This skill is domain-agnostic and focuses on reliable content access rather than interpretation.

---

## Capabilities

- Detect supported file types and encoding
- Extract text from common formats such as PDF, DOCX, TXT, HTML, spreadsheets, and scanned images when OCR is available
- Preserve structural elements such as headings, sections, tables, pages, lists, and footnotes when possible
- Identify document boundaries and section labels
- Normalize whitespace, encoding artifacts, and duplicated content
- Produce chunkable content units for large-document analysis
- Capture parse quality indicators and warnings
- Maintain provenance from parsed output back to source location

---

## Output Format

```yaml
document_id: string
source_path: string
file_type: string
document_text: string
document_structure:
  headings: []
  sections: []
  tables: []
  pages: []
chunk_candidates: []
parse_quality:
  score: number
  status: success | partial | failed
warnings: []
provenance_map: {}
```

---

## Reuse Notes

This skill should be reusable across legal, financial, scientific, operational, and general-purpose document collections because it avoids domain assumptions and exposes a uniform content model.