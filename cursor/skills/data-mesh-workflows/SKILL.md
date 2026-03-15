---
name: data-mesh-workflows
description:
  Use when the user wants to create or manage Loxtep projects, pipelines, data
  products, blueprints, or connections. Covers create_project, apply_blueprint,
  create_pipeline, create_data_product, list_projects, and related Customer MCP
  tools.
---

# Data Mesh Workflows

Guide for common Loxtep workflows using the Loxtep Customer MCP tools.

## When to Use

- User wants to create a project, add a pipeline, or create a data product
- User asks to apply a blueprint or template
- User needs to list or manage projects, pipelines, or connections
- User is setting up a new data flow or data asset

## Customer MCP Tools (use these)

| Tool                      | Purpose                                                                                |
| ------------------------- | -------------------------------------------------------------------------------------- |
| `list_projects`           | List projects in the organization                                                      |
| `create_project`          | Create a project (name required; optional template_slug, description, domain_id)       |
| `get_project`             | Get a single project by project_id                                                     |
| `list_blueprints`         | List blueprints (category: pipelines, connectors, projects, etc.)                     |
| `get_blueprint`           | Get a blueprint by template_id                                                         |
| `apply_blueprint`         | Apply a pipeline/connector/transform/data_product/domain blueprint into a project     |
| `create_pipeline`         | Create a pipeline (connector_id or connection_id; pipeline_type defaults to ingestion) |
| `list_pipelines`          | List pipelines in the project                                                         |
| `create_data_product`     | Create a data product                                                                  |
| `list_data_products`      | List data products in the project                                                     |
| `list_connector_types`    | List available connector types (auth_type: api_key, oauth2, etc.)                     |
| `create_connector`        | Create a connector (API key auth)                                                      |
| `get_connector_oauth_url` | Get OAuth URL for a connector                                                          |
| `create_connection`       | Create a connection in a project                                                       |
| `patch_pipeline_graph`    | Add/update/remove nodes and edges in a pipeline graph                                  |

**Skill attribution (optional):** When calling MCP tools, pass `_metadata: { skill_name: 'data-mesh-workflows' }` in tool args for eval attribution.

## Typical Flows

1. **New project from template:** `create_project` with `template_slug`, or
   `create_project` then `apply_blueprint` for pipelines/connectors.
2. **Add connector to project:** `list_connector_types` → `create_connector`
   (API key) or `get_connector_oauth_url` (OAuth) → use connector_id in
   `create_pipeline` or add connection node via `patch_pipeline_graph`.
3. **Apply existing blueprint:** `list_blueprints` (optional) →
   `apply_blueprint` with project_id, blueprint_type, blueprint_slug.

## Auth

If any tool returns "No valid authentication token" or "RBAC requires JWT", run
`npx @loxtep/customer-mcp-server login` and retry.
