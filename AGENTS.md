# Repository purpose

This repository turns architecture source material into a validated Structurizr DSL workspace and derived diagrams. Inputs may be PDFs, plain text, Markdown, DOCX, Google Drive files, or other accessible text documents. The architecture source of truth is `./structurizr/workspace.dsl`.

# Mandatory architecture workflow

When asked to generate or update architecture, use the skill that matches each input:

- PDFs: `.agents/skills/architecture-pdf-extraction/SKILL.md`.
- Non-PDF documents, Markdown, plain text, and accessible Google Drive content: `.agents/skills/architecture-source-ingestion/SKILL.md`.
- Modeling, review, validation, and Mermaid export: `.agents/skills/structurizr-workspace-generation/SKILL.md`.

In Claude Code, equivalent discoverable skills are under `.claude/skills/` and point to these canonical instructions.

1. Locate the user-provided source(s). For Google Drive or other connected sources, use the available connector only when the user supplied/referred to that source and the connection is available. If an input is inaccessible, request an accessible file or connection; never invent its content.
2. Extract text first and process long sources in manageable page/chunk ranges. Keep a concise evidence index in working context only. Inspect embedded or scanned images only when text misses relevant architecture and visual tools can read them. Do not generate OCR, text dumps, evidence files, or scratch files.
3. Preserve source locations: PDF page numbers; DOCX page/heading/table; Markdown heading or section; Google Drive document title and heading/section; or another stable locator. Record people, systems, containers, responsibilities, relationships, known protocols/technology, contradictions, and unresolved facts. Do not state assumptions as facts.
4. Author/maintain `./structurizr/workspace.dsl` and one Structurizr Mermaid export per view as `./structurizr/<view-key>.mmd` files. `.mmd` files must contain valid Mermaid source returned by the Structurizr MCP exporter, not Markdown fences or prose. Do not create other architecture outputs (including evidence files, JSON/YAML inventories, reports, ADRs, or other diagrams). Explicitly requested Docker PNG/SVG exports are additional derived outputs under `./structurizr/generated/png/` or `./structurizr/generated/svg/`.
5. Delegate a read-only evidence-to-DSL review to `.agents/agents/architecture-reviewer.md` when the runtime supports subagents. Give it the source(s) or accessible locators, compact evidence index, and complete DSL. If unavailable, perform a distinct second-pass review. Reviewer never edits; orchestrator alone resolves findings and edits.
6. Resolve review findings against source evidence and run Structurizr MCP validation on the final DSL. Fix errors and validate again. Do not claim validation succeeded unless MCP reports success. If unavailable, report the limitation and do not call the workspace validated.
7. Read view keys from `./structurizr/workspace.dsl` and use Structurizr MCP `export_mermaid` for every view, saving raw Mermaid to `./structurizr/<view-key>.mmd`. Re-export after every DSL change. Do not keep exports only in agent memory.
8. Check generated architecture files are only `workspace.dsl` and per-view `.mmd`; include PNG/SVG only when the user requests a Docker export. Preserve unrelated repository files and docs.

# Architecture constraints

- Prefer container views. Include a system context view when useful people/external systems are established. Add other container views only for meaningfully distinct behavior or subsystem focus.
- Component and code diagrams are prohibited. Deployment, dynamic, and all other view types are also prohibited unless the user explicitly changes this project policy.
- Model only evidence-supported people, systems, containers, and relationships. Preserve terminology where practical. Never invent protocols, transports, products, or technologies.
- Match the DSL's human-readable language to the source document. If the architecture source is in Russian, use Russian for element names, descriptions, relationship labels, and view titles where Structurizr DSL permits it. Keep DSL keywords and syntax valid; identifiers/view keys must remain syntactically valid and stable.
- Use readable relationship descriptions and consistent tags/styles where useful and valid.
- Never generate Structurizr ADRs, ASCIIDoc, or any other ADR artifact.
- Do not run Docker unless the user asks for a local site or PNG/SVG export. Use the matching skill under `.agents/skills/` and its pinned command, which mounts `./structurizr` at `/usr/local/structurizr`. Never mount the repository root as the workspace directory or create a second DSL elsewhere. Docker-generated PNG/SVG are allowed only for the explicitly requested outcome.

# Tool and output policy

- Structurizr MCP is configured in `.mcp.json` and `.codex/config.toml`. Use `inspect`, `parse`, `validate`, and `export_mermaid` as appropriate. Validate final DSL and export every final view.
- Keep all intermediate analysis in working context. Never save source text, OCR, evidence tables, reviewer reports, plans, logs, or scratch files.
- Do not overwrite unrelated files. When updating a workspace, inspect it and preserve user-authored content unless it conflicts with explicit constraints.
- If a source ambiguity materially changes boundaries or relationships, leave it unresolved in the model and mention it in the final response. Continue with supported content.
