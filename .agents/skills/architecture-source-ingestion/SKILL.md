---
name: architecture-source-ingestion
description: Extract architecture evidence from non-PDF sources such as Markdown, plain text, DOCX, and accessible Google Drive documents. Use for any text/document input that is not a PDF.
---

# Architecture source ingestion (non-PDF)

Produce a concise evidence index for the orchestrator in working context only. This skill is for non-PDF input. For PDFs, use `architecture-pdf-extraction`.

## Source handling

1. Locate the source explicitly supplied or referenced by the user. Do not search unrelated private locations.
2. For plain text and Markdown, read all relevant sections. For long documents, work through ordered chunks and preserve headings. Search may help locate topics but does not replace coverage.
3. For DOCX, use available document-reading tools to extract paragraphs, headings, tables, captions, and footnotes in source order. If text extraction misses meaningful architecture in embedded diagrams, inspect only those relevant images when available. Preserve page number when the reader provides one; otherwise use heading, table caption, or stable section/paragraph locator.
4. For Google Drive, use a connected Drive integration only if available and the user identifies/provides access to the document. Read the document content and stable title/heading or section locations. Do not claim to access Drive when no connector is available; ask the user for an upload or exported file instead.
5. For other document formats, use the best available reader and preserve stable source locators. If content cannot be read reliably, report the specific gap and use only readable material.
6. Keep document contents private to the active task. Never create local text dumps, downloads, OCR files, evidence tables, or other intermediate repository files unless the user explicitly requests them.

## Evidence index (working context only)

Record people/roles, target system and boundary, external systems, containers and responsibilities/technology, directed relationships and their purpose/protocol/transport when explicitly stated, distinct behaviors that may justify container views, contradictions and unknowns, plus a locator for each material claim. Use locators such as document title + heading, section, table caption, paragraph, or source page. Separate explicit facts from labeled inference. Do not resolve unknowns by convention.

Keep the index concise enough to pass to a separate reviewer. Do not produce DSL or Mermaid; return the evidence index and unresolved source questions to the orchestrator.
