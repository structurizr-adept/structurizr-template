---
name: structurizr-local-site
description: Run the local Structurizr site for the workspace in a Docker container and serve it at localhost:5555. Use when the user explicitly asks to open, run, or preview the local Structurizr workspace.
---

# Run the local Structurizr site

Only run this workflow when the user asks for a local preview/site. Follow `AGENTS.md`; do not edit the DSL as part of serving it.

## Preconditions

- Run from the repository root in a POSIX-compatible shell (Linux, macOS, or WSL) with Docker available.
- The workspace file must be `./structurizr/workspace.dsl`. If it is missing, stop and report what is missing; do not create a second DSL elsewhere.
- Run from the repository root so the supplied `./structurizr` mount resolves correctly.

## Start

Run the local server:

```sh
docker run -it --rm -p 5555:8080 -v ./structurizr:/usr/local/structurizr structurizr/structurizr:2026.10.26-playwright local
```

Use the `-playwright` image for local preview too. Keep the foreground process running while the user previews the site. Tell the user to open `http://localhost:5555`. Do not stop the running server unless the user asks or the task is complete and the client requires cleanup; preserve the terminal/session control for shutdown.

If the command runner has no interactive TTY and rejects `-it`, run detached instead:

```sh
docker run -d --rm --name structurizr-local -p 5555:8080 -v ./structurizr:/usr/local/structurizr structurizr/structurizr:2026.10.26-playwright local
```

Report that the server is running and provide `http://localhost:5555`. Leave it running for the user; they can stop it with `docker stop structurizr-local`. If port 5555 is already occupied, report the conflict and ask the user to free the port before retrying; do not silently change the requested port.

## Troubleshoot data-directory permissions

If startup reports `Data directory /usr/local/structurizr is not writable`, try these in order, from the repository root:

1. Run as the current host UID/GID so files remain owned by the user:

   ```sh
   docker run -it --rm -p 5555:8080 -u "$(id -u):$(id -g)" -v ./structurizr:/usr/local/structurizr structurizr/structurizr:2026.10.26-playwright local
   ```

2. If the image requires root inside the container, try:

   ```sh
   docker run -it --rm -p 5555:8080 -u 0:0 -v ./structurizr:/usr/local/structurizr structurizr/structurizr:2026.10.26-playwright local
   ```

   Files created this way may be root-owned on Linux. If that happens, correct host ownership before editing them.

3. Last resort from the supplied notes: `chmod 646 structurizr/workspace.*`, then retry. This grants write permission to other users on the host; use only on a trusted machine and restore narrower permissions afterward. Prefer UID/GID mapping where supported.

The `-noble` image tag was suggested but is unverified; do not switch to it automatically. If the listed options fail, report the exact Docker error and host environment instead of repeatedly broadening permissions. On native Windows PowerShell, `$(id -u)` and `chmod` are not available as written; use WSL or Docker Desktop's documented file-sharing behavior and do not guess a UID.
