# Loxtep Claude Plugin

Claude Code and Claude Cowork plugin for the [Loxtep](https://loxtep.io) data mesh platform. Use 50+ MCP tools from Claude to create projects, pipelines, data products, connections, and schemas.

This plugin lives in the [loxtep-plugins-skills](https://github.com/symmatiq/loxtep-plugins-skills) repo under `claude/`.

## Prerequisites

- **Node.js** 18+
- **Loxtep account** with `owner`, `org_admin`, or `developer` role (for MCP tool access)

## Quick start

1. **Install the plugin** in Claude Code or Claude Cowork (e.g. from Git: clone or install from `https://github.com/symmatiq/loxtep-plugins-skills` and select the `claude/` directory as the plugin path, per your client’s instructions).
2. **Log in once** to save your Loxtep tokens:
   ```bash
   npx @loxtep/customer-mcp-server login
   ```
   Open the printed URL in your browser, sign in to Loxtep, and complete the OAuth flow.
3. **Use the tools** — Loxtep tools appear in Claude’s toolkit (e.g. `create_project`, `list_projects`, `apply_blueprint`, `create_connector`).

## What's included

- **Loxtep Customer MCP** — `npx @loxtep/customer-mcp-server` (projects, pipelines, data products, connectors, connections, schemas, catalog, blueprints, and more).
- **Skills** — `create-connector` (add connectors to projects), `data-mesh-workflows` (common project/pipeline/data product flows), and `loxtep-auth` (recovery when auth fails).

## Environment variables (optional)

- `LOXTEP_ENV` or `NODE_ENV` — Set to `dev` / `development` for dev app/API (`appdev.loxtep.io`, `apidev.loxtep.io`). Default is production.
- `LOXTEP_APP_URL` — Override app base URL for login.
- `LOXTEP_API_BASE_URL` — Override API endpoint.
- `LOXTEP_TOKEN_FILE` — Custom path to token file (default `~/.loxtep/customer-mcp.json`).

See the [Customer MCP Server README](https://github.com/symmatiq/loxtep/blob/main/platform-backend/_customer-mcp-server/README.md) for full details.

## Submission (Claude plugin discovery)

If you submit this plugin to Anthropic’s plugin directory or marketplace, use the repository URL and the `claude/` path as the plugin root. Follow [Claude Code plugins](https://code.claude.com/docs/en/plugins-reference) and [Claude plugin discovery](https://code.claude.com/en/discover-plugins) for current requirements.

## License

MIT
