# Loxtep Cursor Plugin

Cursor plugin for the [Loxtep](https://loxtep.io) data mesh platform. Use 50+ MCP tools from Cursor to create projects, pipelines, data products, connections, and schemas.

This plugin lives in the [loxtep-plugins-skills](https://github.com/symmatiq/loxtep-plugins-skills) repo under `cursor/`. For Cursor Marketplace or “install from Git”, use the repo URL and select the `cursor/` path if your client supports subpaths.

## Prerequisites

- **Node.js** 18+
- **Loxtep account** with `owner`, `org_admin`, or `developer` role (for MCP tool access)

## Quick start

1. **Install the plugin** in Cursor (Settings → Plugins → Install from Git; use `https://github.com/symmatiq/loxtep-plugins-skills` and `cursor/` as the plugin path if supported).
2. **Log in once** to save your Loxtep tokens:
   ```bash
   npx @loxtep/customer-mcp-server login
   ```
   Open the printed URL in your browser, sign in to Loxtep, and complete the OAuth flow.
3. **Use the tools** — Loxtep tools appear in the MCP palette (e.g. `create_project`, `list_projects`, `apply_blueprint`, `create_connector`).

## What's included

- **Loxtep Customer MCP** — `npx @loxtep/customer-mcp-server` (projects, pipelines, data products, connectors, connections, schemas, catalog, blueprints, and more).
- **Auth rule** — If a tool fails with "No valid authentication token", the agent is guided to run the login command.
- **Skills** — `create-connector` (add connectors to projects or create connector instances) and `data-mesh-workflows` (common project/pipeline/data product flows).

## Environment variables (optional)

- `LOXTEP_ENV` or `NODE_ENV` — Set to `dev` / `development` for dev app/API (`appdev.loxtep.io`, `apidev.loxtep.io`). Default is production.
- `LOXTEP_APP_URL` — Override app base URL for login.
- `LOXTEP_API_BASE_URL` — Override API endpoint.
- `LOXTEP_TOKEN_FILE` — Custom path to token file (default `~/.loxtep/customer-mcp.json`).

See the [Customer MCP Server README](https://github.com/symmatiq/loxtep/blob/main/platform-backend/_customer-mcp-server/README.md) for full details.

## Submission (Cursor Marketplace)

Before submitting at [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish): ensure valid manifest, relative paths, README, logo, and frontmatter on rules/skills. Submit the repository URL; if the marketplace supports a plugin subpath, specify `cursor/`. Plugins are manually reviewed.

## License

MIT
