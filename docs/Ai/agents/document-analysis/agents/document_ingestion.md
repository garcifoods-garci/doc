# Document Ingestion Agent

## Purpose

Load documents from a root folder and any nested subfolders, convert them into normalized analysis-ready units, and preserve provenance for all downstream reasoning.

This agent is responsible for discovery, parsing coordination, metadata capture, and content normalization.

---

## Inputs

### Required
- Execution plan from `planner`
- Root folder path or repository location

### Optional
- File inclusion and exclusion rules
- Parser preferences
- Size or time limits
- Existing document index

---

## Outputs

A normalized document collection containing:

- `document_id`
- `source_path`
- `folder_context`
- `file_type`
- `file_metadata`: size, timestamps, author if available
- `document_text`: extracted text
- `document_structure`: sections, headings, tables, pages, paragraphs, attachments if available
- `chunked_units`: manageable analysis segments with offsets
- `parse_quality`: score or status
- `language`: detected language if relevant
- `provenance_map`: trace from extracted text back to source location
- `ingestion_warnings`: OCR issues, corruption, truncation, missing pages, or unsupported content

Optionally, it may also output:
- file inventory
- folder summary
- document grouping suggestions

---

## Responsibilities

1. **Discover documents recursively**
   - Scan root folders and nested subfolders.

2. **Filter and prioritize files**
   - Apply file-type rules and priority heuristics from the plan.

3. **Coordinate parsing**
   - Use reusable parsing skill to convert each file into normalized text and structure.

4. **Preserve provenance**
   - Track where every text span came from so later evidence can be audited.

5. **Chunk content for scale**
   - Split long documents into stable logical segments for parallel analysis.

6. **Assess parse quality**
   - Flag low-quality OCR, malformed files, missing text, or unsupported layouts.

7. **Prepare analysis-ready outputs**
   - Emit a consistent structure regardless of document type.

---

## Supported Document Patterns

The framework should support mixed repositories such as:
- one folder per company,
- one folder per contract,
- one folder per case,
- nested archives by year, region, or business unit,
- loose files in a shared directory.

The ingestion agent must not assume one folder convention. It should capture folder context as metadata rather than embedding logic in code.

---

## Operating Logic

### Step 1: Recursive Discovery
Scan all included directories and collect candidate files.

### Step 2: File Triage
Rank files by likely usefulness using signals such as:
- file type,
- file name,
- recency,
- size,
- folder naming patterns,
- user plan priorities.

### Step 3: Parse and Normalize
Convert each file into a standard content model using `document_parsing` skill.

### Step 4: Chunk and Index
Chunk long text by logical boundaries when possible:
- heading,
- section,
- page,
- table,
- paragraph window.

### Step 5: Emit Quality Signals
Expose parse confidence, missing text, duplicated pages, OCR noise, and unsupported formats.

---

## Example

### Input
- Root folder: `/reports/`
- Plan: recursive financial comparison workflow

### Output
```yaml
documents:
  - document_id: doc-001
    source_path: /reports/company_a/annual_report_2025.pdf
    folder_context:
      - reports
      - company_a
    file_type: pdf
    parse_quality: 0.91
    language: en
    chunked_units:
      - chunk_id: doc-001-sec-01
        section_title: Chairman's Letter
        text_offset_start: 0
        text_offset_end: 5400
      - chunk_id: doc-001-sec-02
        section_title: Risk Factors
        text_offset_start: 5401
        text_offset_end: 13100
    ingestion_warnings: []
```

---

## Handoff to Other Agents

- Sends normalized documents and chunk metadata to `analysis_agent`.
- Sends file coverage, parse quality, and ingestion warnings to `planner` and `decision_agent` for confidence calibration.
- Sends document inventory and folder context to `report_generator` for scope reporting.

---

## Scalability Considerations

A production ingestion layer should support:
- parallel file parsing,
- incremental indexing,
- resumable ingestion,
- duplicate detection,
- chunk caching,
- large-file streaming,
- OCR fallback for scans,
- content hashing for change detection.

---

## Failure and Recovery Behavior

When parsing fails or quality is low:
- continue processing other files,
- record the failure in `ingestion_warnings`,
- reduce confidence contribution from that document,
- optionally queue the file for alternate parsing or OCR retry.

---

## Implementation Notes

This agent should treat parsing as a reusable service boundary. It should avoid domain-specific interpretation and focus only on getting reliable, structured, traceable content into the system.