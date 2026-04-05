# Loxtep Plugins & Skills

Plugins and skills for using [Loxtep](https://loxtep.io) from AI coding and productivity tools. All integrations use the same **Loxtep Customer MCP** (`npx @loxtep/customer-mcp-server`).

The MCP server registers **16 grouped tools** named `loxtep_projects`, `loxtep_workflows`, `loxtep_connectors`, and so on. Each call sets **`operation`** to the flat action name (e.g. `list_projects`, `create_connection`) plus that action’s arguments. See the [Customer MCP README](https://github.com/symmatiq/loxtep/blob/main/platform-backend/_customer-mcp-server/README.md) for the full map. Under the hood that still covers projects, workflows, data products, connectors, templates, catalog, and the rest of the customer tool surface.

## Plugins (siblings)

| Plugin | Platform | Path | Description |
|--------|----------|------|-------------|
| **Cursor** | Cursor IDE | [cursor/](cursor/) | Cursor Marketplace plugin: MCP, rules, skills. Install from Git or [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish). |
| **Claude** | Claude Code & Claude Cowork | [claude/](claude/) | Claude plugin: MCP, skills. Install from Git or Claude plugin discovery. |
| **Kiro** | Kiro IDE | [kiro/](kiro/) | MCP config + README. Copy `mcp.json` into `.kiro/settings/mcp.json` or `~/.kiro/settings/mcp.json`. |
| **Antigravity** | Google Antigravity IDE | [antigravity/](antigravity/) | MCP config + README. Add Loxtep server via Manage MCP Servers → View raw config. |
| **Codex** | OpenAI Codex | [codex/](codex/) | MCP config + README. Run `codex mcp add loxtep -- npx @loxtep/customer-mcp-server` or edit `~/.codex/config.toml`. |

## One-time setup (all platforms)

After installing your plugin or integration, log in once to save your Loxtep tokens:

```bash
npx @loxtep/customer-mcp-server login
```

Open the printed URL in your browser, sign in to Loxtep, and complete the OAuth flow. Tokens are stored at `~/.loxtep/customer-mcp.json` and used by the MCP server.

## Repository layout

- **cursor/** — Cursor plugin (`.cursor-plugin/`, rules, skills, assets).
- **claude/** — Claude Code / Cowork plugin (`.claude-plugin/`, skills).
- **kiro/** — Kiro IDE: MCP config and README.
- **antigravity/** — Antigravity IDE: MCP config and README.
- **codex/** — Codex CLI/IDE: TOML snippet and README.

See each directory’s `README.md` for install and usage instructions.

**Skills roadmap** (current vs candidate future skills): [docs/skills-roadmap.md](docs/skills-roadmap.md).

## Skill attribution (optional)

When invoking Loxtep MCP tools, agents may pass `_metadata` in tool arguments for
attribution and eval scoring. This is **fully optional** and backward-compatible.

**Convention:** Include `_metadata: { skill_name: 'skill-slug' }` in the tool
arguments. The `skill_name` should match the skill's `name` from its frontmatter
(e.g. `create-connector`, `data-workflows`). The Loxtep platform uses this
for per-skill eval and analytics when available.

```json
{
  "operation": "create_connector",
  "connector_type": "shopify",
  "metadata": { "api_key": "..." },
  "_metadata": { "skill_name": "create-connector" }
}
```

(Call the MCP tool **`loxtep_connectors`** with the JSON above as arguments.)

Tools ignore `_metadata` for tool logic; it is used only for request attribution.

## License

MIT
