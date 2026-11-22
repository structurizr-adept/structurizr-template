---
name: structurizr-workspace-generation
description: Turn page-referenced evidence from PDFs or stable-locator evidence from text documents into Structurizr DSL, coordinate read-only review, validate through Structurizr MCP, and save each view as a raw Mermaid .mmd file.
---

# Structurizr workspace generation

Follow the complete project policy in `AGENTS.md`. This skill owns evidence-to-DSL modeling, independent review, validation, and Mermaid export.

## 1. Plan before editing

Use the evidence index in working context. List the target system, people/external systems, containers, supported relationships, and candidate container views. Check material claims have source locations (PDF page or document heading/section/table/paragraph). Keep unknown protocols and boundaries unknown. Choose clear identifiers and stable view keys. Do not create a planning or evidence file.

## 2. Author the DSL

- Write/maintain `./structurizr/workspace.dsl` using valid Structurizr DSL syntax.
- Include system context only when useful surrounding people/systems are established; make container views primary.
- Only system-context and container views are allowed. Component/code, deployment, dynamic, and other views are prohibited unless the user explicitly changes project policy.
- Add additional container views only for distinct, evidence-supported behaviors or subsystem focus.
- Model relationships at supported detail. Include purpose, technology, protocol, or transport only when stated in source. Never infer HTTP/REST, a queue, database, cloud service, or other implementation from convention.
- Match human-readable DSL content to the source language. When the source document is Russian, write element names, descriptions, relationship labels, and view titles in Russian wherever the DSL allows. Keep DSL keywords/syntax valid and identifiers/view keys syntactically valid and stable.
- Use readable descriptions, tags, and styles consistently where valid and useful. Do not imply unsupported facts.
- Do not author ADRs or ADR files.

## 3. Independent evidence review

Ask the orchestrator to invoke `.agents/agents/architecture-reviewer.md` after the first complete DSL exists. Pass the reviewer the exact DSL, project constraints, compact page/section-locator evidence index, and accessible source or relevant source sections. The reviewer reports only and never edits. If no native subagent facility is available, perform a separate read-only review pass. The orchestrator alone resolves findings and edits the DSL.

## 4. Validate

Use Structurizr MCP `parse`/`inspect` where useful, then `validate` the final DSL. Read the full result, repair errors, and repeat. Confirm all model content and views follow evidence and project policy. MCP validity and evidence fidelity are separate checks. If MCP validation is unavailable, report that and never claim validation success.

## 5. Export Mermaid as `.mmd`

For every final view key, call Structurizr MCP `export_mermaid` with the exact final DSL and view key. Write the returned Mermaid source, without Markdown fences or prose, to `./structurizr/<view-key>.mmd`. These are raw Mermaid files, not `.md` documents. Use a collision-safe view-key-based name only if needed. Confirm file contents match the export. Re-export every view after any DSL change. If an export fails, report it and do not claim all views were exported.

## 6. Final check

For ordinary architecture generation, generated architecture deliverables are `./structurizr/workspace.dsl` and one `.mmd` per view in `./structurizr/`. Docker PNG/SVG exports are created only when the user explicitly requests them and are governed by their respective skills. Do not save scratch files, evidence, review reports, logs, or ADRs. Preserve unrelated files. In the final response, summarize validation/export status and any unresolved evidence gaps.
