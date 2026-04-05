---
name: create-connector
description:
  Guides adding connectors via Customer MCP grouped tools loxtep_connectors,
  loxtep_connections, and loxtep_templates. Use for OAuth/API-key connectors,
  project connections, or applying connector templates.
---

# Create Connector (Customer MCP)

This skill covers **org-level connectors** and **project connection nodes** using the Customer MCP **facade** tools.

## How calls work

The client lists tools like **`loxtep_connectors`**, **`loxtep_connections`**, **`loxtep_templates`**. Every call must include:

- **`operation`** — one of the allowed flat names for that tool (see table below).
- **Other fields** — parameters for that action (e.g. `connector_type`, `project_id`).

**Examples**

- List connector types: tool **`loxtep_connectors`**, arguments `{ "operation": "list_connector_types" }`.
- Create API-key connector: **`loxtep_connectors`**, `{ "operation": "create_connector", "connector_type": "...", ... }`.
- Add a connection node to a project: **`loxtep_connections`**, `{ "operation": "create_connection", "project_id": "...", ... }`.
- Apply a connector template: **`loxtep_templates`**, `{ "operation": "apply_template", "project_id": "...", "template_type": "...", "template_slug": "..." }`.

## When to Use

- User wants an integration (Salesforce, QuickBooks, Slack, Shopify, custom API)
- User says “create connector”, “add connector”, “OAuth for connector”
- User needs a connector instance or a **connection** on a workflow graph

## Section A — Add connector to project (existing types)

| MCP tool            | `operation`             | Purpose                                                                                                                                 |
| ------------------- | ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `loxtep_connectors` | `list_connector_types`  | Available types and `auth_type` (api_key, oauth2, basic).                                                                               |
| `loxtep_connectors` | `list_connectors`       | Connectors in the org (optional filter by `connector_type`).                                                                          |
| `loxtep_connectors` | `create_connector`      | Create connector with **API key** (or basic) auth; pass `connector_type` and credentials/metadata.                                     |
| `loxtep_connectors` | `get_connector_oauth_url` | OAuth URL; often `connector_type` + `connection_config` (e.g. Shopify shop slug). User completes OAuth in the browser.                 |
| `loxtep_connections` | `create_connection`    | Create a **connection** node in a project (ties a connector into a flow).                                                             |
| `loxtep_templates`  | `apply_template`        | Apply a catalog template into a project: `project_id`, `template_type`, `template_slug`.                                                |

To change the workflow graph (nodes/edges), use **`loxtep_workflows`** with `operation: "patch_workflow_graph"` (see **data-workflows** skill).

### Flow: API key connector

1. `loxtep_connectors` + `list_connector_types`.
2. `loxtep_connectors` + `create_connector`.
3. Use returned `connector_id` in `loxtep_connections` + `create_connection`, or in `loxtep_workflows` + `patch_workflow_graph`.

### Flow: OAuth (e.g. Shopify)

1. `loxtep_connectors` + `list_connector_types`.
2. `loxtep_connectors` + `get_connector_oauth_url` — user finishes OAuth in the browser.
3. Use `connector_id` in `create_connection` or graph patch as above.

### Flow: Apply connector template

1. Optional: `loxtep_templates` + `list_templates` / `get_template`.
2. `loxtep_templates` + `apply_template` with `project_id`, `template_type`, `template_slug`.

## Section B — Add new connector type (platform partners)

Adding a **new connector type** (new provider in the platform catalog) is not available via Customer MCP.

- **Platform partners / Loxtep contributors:** Internal dev MCP or guides `ADDING_A_NEW_CONNECTOR_TYPE` / `ADDING_A_COMPOSIO_CONNECTOR` in the Loxtep monorepo.
- **Customers:** Use `list_connector_types` and Section A; contact Loxtep to request new types.

## Auth

If any tool returns "No valid authentication token" or "RBAC requires JWT", run:

```bash
npx @loxtep/customer-mcp-server login
```

Then retry.

## Skill attribution (optional)

Include `_metadata` alongside `operation` and other args, for example:

```json
{
  "operation": "create_connector",
  "connector_type": "shopify",
  "_metadata": { "skill_name": "create-connector" }
}
```

Fully optional; ignored for tool logic.
