---
name: data-workflows
description:
  Use when the user wants to build or operate the data mesh studio — projects, workflows,
  templates, connections, data products, omnichannel unified products, webhook consumptions,
  session context, or "ingest / unify / expose sales data". Customer MCP tools loxtep_session,
  loxtep_projects, loxtep_templates, loxtep_workflows, loxtep_connections, loxtep_data_products.
  Stories S0, S2, S3; orchestrates with create-connector for S1. See docs/skills-user-stories.md.
---

# Data mesh studio (Customer MCP)

End-to-end playbooks for **projects**, **workflow graphs**, **connections**, **data products**, and **webhook consumptions**, plus **session** context. Pair with **`create-connector`** for SaaS/API ingest (Shopify, etc.) and **`loxtep-instances`** for runtime provisioning.

## When to use

- **S0:** Confirm user and org (`get_current_user`, `get_current_organization`).
- **S2:** Create an **omnichannel** or unified **data product** across multiple sources in a project.
- **S3:** Register an **external webhook** for data product updates (`create_consumption`).
- User asks for **projects**, **flows**, **templates**, **connections** (project nodes), **data products**, **consumptions**, or **patch workflow graph**.

## Prerequisites

- MCP login (`loxtep-auth` if JWT errors). For **permissions / 403**, see **`loxtep-mcp-session`** (`get_current_user` → `permissions`).
- **Project-scoped** calls require `project_id` in the same payload as `operation`.
- **Org-scoped** data products / consumptions: use `data_product_id` as required by each operation.

## How MCP calls work

1. **Tool name** — one of `loxtep_session`, `loxtep_projects`, `loxtep_templates`, `loxtep_workflows`, `loxtep_connections`, `loxtep_data_products`.
2. **`operation`** — flat tool id (e.g. `list_workflows`).
3. **Other fields** — API args at top level next to `operation`.

## Happy-path flows

### Flow A — Session before work (S0)

1. `loxtep_session` → `{ "operation": "get_current_user" }`
2. `loxtep_session` → `{ "operation": "get_current_organization" }`

### Flow B — New project then template (studio bootstrap)

1. `loxtep_projects` → `create_project` (`name`, optional `template_slug`, …).
2. `loxtep_templates` → `list_templates` / `get_template` (optional).
3. `loxtep_templates` → `apply_template` with `project_id`, `template_type`, `template_slug`.

### Flow C — Omnichannel data product (S2)

1. Ensure **project** exists; add **connections** per channel (see **`create-connector`** skill).
2. `loxtep_workflows` → `create_workflow` / `patch_workflow_graph` to model transforms and joins.
3. `loxtep_data_products` → `create_data_product` (project-scoped where applicable).
4. `get_data_product` / `get_data_product_lexicon` to verify; `update_data_product` as needed.

### Flow D — Webhook for data product updates (S3)

1. Obtain `data_product_id` (`list_data_products` / `get_data_product`).
2. `loxtep_data_products` → `create_consumption` with `data_product_id`, `endpoint_url`, optional `headers`, `secret_token`, `filters`, `delivery_method` (default webhook per platform).
3. Optional: `list_consumptions` to audit active subscriptions.

## MCP mapping (operations and scope)

| Step | User intent | Tool | `operation` | Scope | Key args |
|------|-------------|------|---------------|-------|----------|
| 1 | Who am I? | `loxtep_session` | `get_current_user` | organization | — |
| 2 | Which org? | `loxtep_session` | `get_current_organization` | organization | — |
| 3 | List/create/update/delete projects | `loxtep_projects` | `list_projects`, `get_project`, `create_project`, `update_project`, `delete_project` | organization | `name`, ids |
| 4 | Templates | `loxtep_templates` | `list_templates`, `get_template`, `apply_template` | organization / **project** for `apply_template` | `apply_template`: `project_id`, `template_type`, `template_slug` |
| 5 | Workflows | `loxtep_workflows` | `create_workflow`, `update_workflow`, `delete_workflow`, `list_workflows`, `get_workflow`, `get_workflow_graph`, `patch_workflow_graph`, `preview_transform`, `create_transformation`, `create_validation` | **project** | `project_id` |
| 6 | Connection nodes | `loxtep_connections` | `create_connection`, `update_connection`, `delete_connection`, `list_connections`, `get_connection`, `test_connection` | **project** | `project_id` |
| 7 | Data products | `loxtep_data_products` | `create_data_product`, `update_data_product`, `delete_data_product`, `list_data_products`, `get_data_product`, `get_data_product_lexicon` | **project** or org per op | `project_id` where required |
| 8 | Webhook consumptions | `loxtep_data_products` | `list_consumptions`, `create_consumption` | **organization** | `data_product_id`, `endpoint_url`, … |

## Pitfalls

- Missing **`project_id`** on project-scoped workflow/connection ops.
- **`apply_template`** requires `project_id` — not the same as org-only template list.
- **`test_connection`** — Loads connection from workspace storage; runs an optional **HTTP GET** probe only when the saved configuration includes a URL-like field (`base_url`, `url`, `host`, …). Otherwise it confirms the entity exists without a live probe.
- **`create_transformation`** / **`create_validation`** — Require an **existing workflow graph** (`get_workflow_graph` / prior `patch_workflow_graph`). If the graph is missing, the tool fails with a not-found style error.
- **Paid plans vs shared instance** — provisioning is **`loxtep-instances`**, not this skill.
- **Agent issues/goals** — use **`loxtep-agent-workspace`**, not `loxtep_projects`.

## Optional attribution

```json
{
  "operation": "create_data_product",
  "project_id": "...",
  "_metadata": { "skill_name": "data-workflows" }
}
```

## Auth

`npx @loxtep/customer-mcp-server login` — see **`loxtep-auth`**.

## References

- [User story catalog](../../../docs/skills-user-stories.md) (this repo)
