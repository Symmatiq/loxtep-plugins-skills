# Loxtep for Kiro

Use the [Loxtep](https://loxtep.io) Customer MCP from [Kiro](https://kiro.dev) to create projects, pipelines, data products, connectors, and more (50+ tools).

This directory lives in the [loxtep-plugins-skills](https://github.com/symmatiq/loxtep-plugins-skills) repo under `kiro/`.

## Prerequisites

- **Node.js** 18+
- **Loxtep account** with `owner`, `org_admin`, or `developer` role (for MCP tool access)

## Install

1. **Add the Loxtep MCP server** to Kiro:
   - **Workspace:** Copy the contents of `mcp.json` into `.kiro/settings/mcp.json` in your project (create the file if needed), or merge the `loxtep` entry into your existing `mcpServers` object.
   - **User (global):** Copy into `~/.kiro/settings/mcp.json` so Loxtep is available in all workspaces.
   - See [Kiro MCP configuration](https://kiro.dev/docs/mcp/configuration) for locations and full options (e.g. `env`, `autoApprove`, `disabledTools`).

2. **Log in once** to save your Loxtep tokens:
   ```bash
   npx @loxtep/customer-mcp-server login
   ```
   Open the printed URL in your browser, sign in to Loxtep, and complete the OAuth flow.

3. **Use the tools** — Loxtep tools appear in Kiro’s MCP panel (e.g. `create_project`, `list_projects`, `apply_blueprint`, `create_connector`).

## What you get

- **Loxtep Customer MCP** — `npx @loxtep/customer-mcp-server` (projects, pipelines, data products, connectors, connections, schemas, catalog, blueprints, and more). Kiro uses MCP tools only; there is no separate “skills” bundle.

## Environment variables (optional)

- `LOXTEP_ENV` or `NODE_ENV` — Set to `dev` / `development` for dev app/API (`appdev.loxtep.io`, `apidev.loxtep.io`). Default is production.
- `LOXTEP_APP_URL` — Override app base URL for login.
- `LOXTEP_API_BASE_URL` — Override API endpoint.
- `LOXTEP_TOKEN_FILE` — Custom path to token file (default `~/.loxtep/customer-mcp.json`).

Add these under the `loxtep` server’s `env` object in your MCP config if needed. See the [Customer MCP Server README](https://github.com/symmatiq/loxtep/blob/main/platform-backend/_customer-mcp-server/README.md) for full details.

## License

MIT
