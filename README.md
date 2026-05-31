# homelab-mcp-template

Copier template for new per-service MCP server repos in the homelab.

It captures the shared repo shape from `homelab-reolink-mcp` — uv, ruff,
mypy, pytest, FastMCP scaffolding, CI, labels, issue/PR templates,
release-please, Docker, and Copilot instructions — without vendoring cockpit
ADRs or custom agents.

## Usage

```bash
uv tool install copier
copier copy gh:TheLeftMoose/homelab-mcp-template /path/to/new-repo
```

Example for repo #2:

```bash
copier copy gh:TheLeftMoose/homelab-mcp-template /home/desck/src/homelab-pihole-mcp \
  --data service_name=pihole \
  --data description="Slim MCP server for Pi-hole diagnostics"
```

## Variables

See [`copier.yml`](copier.yml) for the source of truth. Key variables:

- `service_name` — kebab-case service key, repo suffix, MCP key.
- `python_pkg` — import package, default `<service_name>_mcp` with `_`.
- `service_display_name` — human-readable name for docs.
- `vendor_library` — optional upstream Python client package.
- `default_port` — default service API port.
- `description` — one-line pyproject/README/GitHub description.
- `author_name`, `repo_owner`, `repo_name`, `service_env_prefix`.

## After `copier copy`

1. Inspect and replace the scaffold tool in `src/<python_pkg>/server.py` with
   service-specific read-only observations.
2. Run `uv sync && uv run pytest && uv run ruff check . && uv run mypy src`.
3. Initialize git, create the GitHub repo, commit, and push.
4. Wire the new MCP into the cockpit via `homelab-llm/scripts/render-mcp-config.sh`.
5. File the W1/tool-surface issue required by
   [ADR-0007](https://github.com/TheLeftMoose/homelab-llm/blob/main/docs/adr/0007-mcp-self-sufficiency.md).
6. Install shared agents via
   `copilot plugin install github.com/TheLeftMoose/homelab-copilot-plugin`.

## Decision context

- [ADR-0008 — MCP repo template strategy](https://github.com/TheLeftMoose/homelab-llm/blob/main/docs/adr/0008-mcp-repo-template-strategy.md)
- [ADR-0006 — cockpit-not-monorepo](https://github.com/TheLeftMoose/homelab-llm/blob/main/docs/adr/0006-cockpit-not-monorepo.md)
- [ADR-0007 — MCP self-sufficiency](https://github.com/TheLeftMoose/homelab-llm/blob/main/docs/adr/0007-mcp-self-sufficiency.md)

## License

MIT. See [`LICENSE`](LICENSE).
