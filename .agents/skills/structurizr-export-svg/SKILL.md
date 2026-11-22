---
name: structurizr-export-svg
description: Export SVG diagrams from each Structurizr view using the pinned Playwright Docker image. Use only when the user requests SVG exports.
---

# Export Structurizr views as SVG

Run only when requested. Follow `AGENTS.md` and use the final validated DSL. Do not modify the DSL or create exports for a stale version.

## Preconditions and command

Run from repository root in a POSIX-compatible shell (Linux, macOS, or WSL). Confirm `./structurizr/workspace.dsl` exists and inspect its final view keys. SVGs are explicitly requested derived outputs and go under `./structurizr/generated/svg/`. Create that directory if needed with `mkdir -p structurizr/generated/svg`.

Run:

```sh
docker run -it --rm -v ./structurizr:/usr/local/structurizr structurizr/structurizr:2026.10.26-playwright export -workspace workspace.dsl -format svg -output generated/svg
```

Afterwards inspect the exit status and confirm SVG files were written for the requested views. Do not claim success based on the command alone if it exited unsuccessfully or generated files are missing. Keep `workspace.dsl` and `.mmd` exports unchanged.

## Troubleshoot permissions

If the container cannot write under `/usr/local/structurizr` or output files are root-owned, retry with matching host identity:

```sh
docker run -it --rm -u "$(id -u):$(id -g)" -v ./structurizr:/usr/local/structurizr structurizr/structurizr:2026.10.26-playwright export -workspace workspace.dsl -format svg -output generated/svg
```

If that image needs root, retry with `-u 0:0`; files may then be root-owned. Last resort from the supplied notes is `chmod 646 structurizr/workspace.*`, which grants other host users write permission. Prefer UID/GID mapping; use the broad mode only on a trusted host and restore narrower permissions promptly. The proposed `-noble` image tag is unverified and must not be selected automatically. If permission errors remain, report the exact error and host environment. These `id`/`chmod` commands assume a POSIX shell; on Windows use WSL or documented Docker Desktop mount behavior rather than guessing.

Root retry command:

```sh
docker run -it --rm -u 0:0 -v ./structurizr:/usr/local/structurizr structurizr/structurizr:2026.10.26-playwright export -workspace workspace.dsl -format svg -output generated/svg
```

If the error specifically says the workspace files are not writable, the supplied last-resort command is `chmod 646 structurizr/workspace.*`. For a directory-creation error, correct ownership/write access on `structurizr/generated/svg/` instead; changing workspace file modes does not make a directory writable.
