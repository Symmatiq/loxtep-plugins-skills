# Loxtep Plugins & Skills

Plugins and skills for using [Loxtep](https://loxtep.io) from AI coding and productivity tools. All integrations use the same **Loxtep Customer MCP** (`npx @loxtep/customer-mcp-server`) and expose the same Loxtep tools (projects, pipelines, data products, connectors, blueprints, and more).

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

## License

MIT
