---
name: create-connector
description:
  Use when the user wants to connect Shopify, Salesforce, QuickBooks, Slack, or another SaaS/API
  into Loxtep — OAuth, API keys, list connector types, create org connector, get OAuth URL,
  add connection nodes to a project, or apply connector templates. Customer MCP loxtep_connectors,
  loxtep_connections, loxtep_templates. User story S1; then use data-workflows for graph and
  data products. See docs/skills-user-stories.md.
---

# Connect systems and ingest (Customer MCP)

**Story S1:** Bring external systems (e.g. **Shopify**) into the mesh: org-level **connector**, then **project connection** (and optionally **connector template**). After connections exist, use **`data-workflows`** for workflow graph and data products.

## When to use

- “Connect **Shopify** / **Salesforce** / …”
- “**OAuth** for a connector” or “**API key** connector”
- `list_connector_types`, `create_connector`, `get_connector_oauth_url`
- Add a **connection** node to a **project** (`create_connection`)
- Apply a **connector** catalog template (`apply_template` with connector template)

## Prerequisites

- MCP auth (`loxtep-auth`).
- **`project_id`** for `loxtep_connections` and for `apply_template` on project templates.

## Happy-path flows

### Flow — OAuth (e.g. Shopify)

| Step | Action | Tool | `operation` | Scope |
|------|--------|------|-------------|-------|
| 1 | Discover types | `loxtep_connectors` | `list_connector_types` | **global** |
| 2 | Start OAuth | `loxtep_connectors` | `get_connector_oauth_url` | organization |
| 3 | User completes browser OAuth | — | — | — |
| 4 | Attach to project | `loxtep_connections` | `create_connection` | **project** (`project_id`) |
| 5 | Wire into flow | `loxtep_workflows` | `patch_workflow_graph` | **project** — see **data-workflows** |

### Flow — API key connector

| Step | Action | Tool | `operation` |
|------|--------|------|-------------|
| 1 | `list_connector_types` | `loxtep_connectors` | `list_connector_types` |
| 2 | `create_connector` with `connector_type` + credentials/metadata | `loxtep_connectors` | `create_connector` |
| 3 | `create_connection` with `connector_id` + `project_id` | `loxtep_connections` | `create_connection` |

### Flow — Connector template from catalog

1. `loxtep_templates` → `list_templates` / `get_template` (optional).
2. `loxtep_templates` → `apply_template` with `project_id`, `template_type`, `template_slug`.

## MCP mapping

| User intent | Tool | `operation` | Scope | Notes |
|-------------|------|-------------|-------|-------|
| List types | `loxtep_connectors` | `list_connector_types` | global | No org id in scope metadata |
| List org connectors | `loxtep_connectors` | `list_connectors` | organization | |
| Create connector | `loxtep_connectors` | `create_connector` | organization | |
| OAuth URL | `loxtep_connectors` | `get_connector_oauth_url` | organization | |
| CRUD connection node | `loxtep_connections` | `create_connection`, `update_connection`, `delete_connection`, `list_connections`, `get_connection`, `test_connection` | **project** | Always `project_id` |
| Apply template | `loxtep_templates` | `apply_template` | **project** | `project_id`, `template_type`, `template_slug` |

## Pitfalls

- **`list_connector_types`** is **global** — do not assume org context for discovery.
- **`create_connection`** without **`project_id`** fails for project-scoped tool.
- **`test_connection`** — See **data-workflows** / connections mapping: optional HTTP GET when the stored connection config includes a probe URL; otherwise config-only success.
- **New connector types** (new provider in platform code) are **not** creatable via MCP — use `list_connector_types`; platform work is internal.

## New connector types (platform partners)

Not available via Customer MCP — new provider types are shipped with the platform (engineering / Composio integration), not created through these tools.

## Optional attribution

```json
{
  "operation": "create_connector",
  "connector_type": "shopify",
  "_metadata": { "skill_name": "create-connector" }
}
```

## Auth

`npx @loxtep/customer-mcp-server login` — **`loxtep-auth`**.

## References

- [User story catalog](../../../docs/skills-user-stories.md)
