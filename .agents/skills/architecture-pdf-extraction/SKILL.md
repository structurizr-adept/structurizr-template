---
name: architecture-pdf-extraction
description: Extract page-referenced architecture evidence from a supplied software architecture PDF. Use only for PDF inputs, including long, scanned, or diagram-heavy PDFs; prefer selectable text and inspect relevant images only when needed.
---

# Architecture PDF extraction

Produce an evidence index for the orchestrator, not a file. Keep it in working context and compact enough to pass to the reviewer.

## Procedure

1. Identify the supplied PDF path or attachment and determine page count. Do not search unrelated private directories. If no PDF is accessible, ask for its path or upload location.
2. Prefer selectable text extraction, in manageable page ranges (for example, 10–20 pages). Preserve actual PDF page numbers and headings. Use available PDF tools without assuming a particular binary is installed.
3. If extraction is empty, garbled, or misses important architecture content, inspect only relevant pages visually when a PDF/image tool is available. Prioritize architecture diagrams, legends, tables, and labels. Do not OCR or inspect every page by default. If the image is unreadable or visual tools unavailable, record the gap.
4. Cover all page ranges; search terms can locate candidate passages but must not replace reading coverage. Consolidate repeated facts while retaining all useful page references.
5. Record claims as evidence, separate explicit facts from labeled inference, and retain contradictions and unknowns. Do not resolve missing facts by convention.

## Evidence index (working context only)

Capture people/roles, target system and boundary, external systems, containers and responsibilities/technology, directed interactions and their purpose/protocol when stated, behaviors relevant to distinct container views, contradictions/uncertainties, and PDF page numbers for every material claim. Quote only short phrases when needed. Make the index concise enough for a separate reviewer to spot-check source pages.

## Restrictions

- Do not create extracted text, OCR images, evidence indexes, temporary Markdown, or any other intermediate files.
- Do not infer architecture from product familiarity or technology names alone; do not treat decorative or unreadable images as evidence.
- Do not model components or code-level detail.
- Do not produce DSL or Mermaid. Return only the working-context evidence index and unresolved source questions to the orchestrator.
