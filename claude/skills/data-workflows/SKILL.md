---
name: data-workflows
description:
  Use when the user wants to create or manage Loxtep projects, workflows, data
  products, templates, or connections via Customer MCP. Covers loxtep_projects,
  loxtep_workflows, loxtep_templates, loxtep_data_products, loxtep_connections,
  loxtep_connectors, and related grouped tools.
---

# Data workflows

Guide for common Loxtep workflows using the **Loxtep Customer MCP** (`@loxtep/customer-mcp-server`).

## How Customer MCP tools are grouped

The MCP server exposes **16 tools** named `loxtep_session`, `loxtep_projects`, `loxtep_workflows`, `loxtep_templates`, `loxtep_connectors`, `loxtep_connections`, `loxtep_data_products`, and so on (one tool per area).

**Each call uses:**

1. **MCP tool name** — the `loxtep_*` name shown in the client’s tool list.
2. **`operation`** (string) — the flat action id (e.g. `list_workflows`, `create_project`).
3. **Other arguments** — whatever that action needs (`project_id`, `name`, etc.), same shape as the Loxtep tool API.

**Example — list workflows**

- Tool: `loxtep_workflows`
- Arguments: `{ "operation": "list_workflows", "project_id": "<uuid>" }`

**Example — create a project**

- Tool: `loxtep_projects`
- Arguments: `{ "operation": "create_project", "name": "My project" }`

Full list of facades and allowed `operation` values: [Customer MCP Server README](https://github.com/symmatiq/loxtep/blob/main/platform-backend/_customer-mcp-server/README.md) or Loxtep app **Docs → Customer MCP Server**.

## When to Use

- User wants to create a project, add or edit a **workflow** (flow), or create a data product
- User asks to apply a **template** from the catalog into a project
- User needs to list or manage projects, workflows, or connections
- User is setting up a new data flow

## Facades and operations this skill focuses on

| MCP tool           | `operation` values (selection)                                                                                                                                                                                                                                                                         | Purpose                                |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------- |
| `loxtep_projects`  | `list_projects`, `get_project`, `create_project`, `update_project`, `delete_project`                                                                                                                                                                                                                   | Organization projects                  |
| `loxtep_templates` | `list_templates`, `get_template`, `apply_template`                                                                                                                                                                                                                                                   | Catalog templates (`apply_template`: `project_id`, `template_type`, `template_slug`) |
| `loxtep_workflows` | `create_workflow`, `update_workflow`, `delete_workflow`, `list_workflows`, `get_workflow`, `get_workflow_graph`, `patch_workflow_graph`, `preview_transform`, `create_transformation`, `create_validation`                                                                                              | Flows, graph, transforms               |
| `loxtep_connections` | `create_connection`, `update_connection`, `delete_connection`, `list_connections`, `get_connection`, `test_connection`                                                                                                                                                                            | Connection nodes (project-scoped)      |
| `loxtep_data_products` | `create_data_product`, `update_data_product`, `delete_data_product`, `list_data_products`, `get_data_product`, `get_data_product_lexicon`, `list_consumptions`, `create_consumption`                                                                                                                | Data products & consumptions           |
| `loxtep_connectors` | `list_connectors`, `list_connector_types`, `create_connector`, `get_connector_oauth_url`                                                                                                                                                                                                              | Org-level connector definitions        |

Other facades (`loxtep_session`, `loxtep_instances`, `loxtep_schemas`, `loxtep_quality`, `loxtep_catalog`, `loxtep_analytics`, `loxtep_workspace`, `loxtep_process_intel`, `loxtep_procedures`, `loxtep_agent_workspace`) cover session, instances, definitions, health, discovery, analytics, snapshots/queues, process intel, procedures, and agent workspace — see the README for their `operation` lists.

**Skill attribution (optional):** Add `_metadata: { skill_name: 'data-workflows' }` next to `operation` and the other arguments.

## Typical flows

1. **New project from template:** `loxtep_projects` + `operation: "create_project"` with `template_slug`, or create the project then `loxtep_templates` + `operation: "apply_template"` with `project_id`, `template_type`, `template_slug`.
2. **Connect data source to a flow:** `loxtep_connectors` + `list_connector_types` → `create_connector` or `get_connector_oauth_url` → `loxtep_connections` + `create_connection` with `project_id` / `connector_id`, or update the graph with `loxtep_workflows` + `patch_workflow_graph`.
3. **Apply a catalog template:** `loxtep_templates` + `list_templates` or `get_template` (optional) → `apply_template` with `project_id`, `template_type`, `template_slug`.

## Auth

If any tool returns "No valid authentication token" or "RBAC requires JWT", run `npx @loxtep/customer-mcp-server login` and retry.
