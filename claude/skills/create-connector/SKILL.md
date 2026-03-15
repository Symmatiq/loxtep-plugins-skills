---
name: create-connector
description:
  Guides adding connectors to Loxtep projects using the Customer MCP. Use when
  the user wants to add a connector, create a connector instance, connect an app
  (Salesforce, QuickBooks, Slack, Shopify, etc.), get OAuth URL, or apply a
  connector blueprint.
---

# Create Connector (Customer MCP)

This skill guides **adding connectors to your Loxtep project** using the Loxtep
Customer MCP tools: listing types, creating connector instances (API key or
OAuth), and applying connector blueprints.

## When to Use

- User wants to add an integration (Salesforce, QuickBooks, Slack, Shopify,
  custom API)
- User says "create connector", "add connector", "connect Shopify", "get OAuth
  for connector"
- User needs to create a connector instance or add a connection to a pipeline

## Section A — Add Connector to Project (Existing Types)

Use these **Customer MCP tools** to add an existing connector type to your
project:

| Tool                      | Purpose                                                                                                                                                                                                              |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `list_connector_types`    | List available connector types and auth_type (api_key, oauth2, basic). Use to see what's available.                                                                                                                  |
| `list_connectors`         | List connectors in the organization (filter by connector_type).                                                                                                                                                      |
| `create_connector`        | Create a connector with **API key auth**. Pass connector_type and metadata (e.g. api_key). Optionally include `_metadata: { skill_name: 'create-connector' }` for eval attribution.                                                                                                                             |
| `get_connector_oauth_url` | Get OAuth URL for a connector. Pass connector_id (existing) OR connector_type + connection_config (e.g. Shopify: connector_type "shopify", connection_config { shop: "store-name" }). Creates connector after OAuth. |
| `create_connection`       | Create a connection in a project (pipeline node).                                                                                                                                                                     |
| `apply_blueprint`         | Apply a connector blueprint into a project. Params: project_id, blueprint_type "connectors", blueprint_slug.                                                                                                          |

### Flow: API key connector

1. `list_connector_types` — confirm the type exists and auth_type is api_key (or basic).
2. `create_connector` — pass connector_type and metadata (e.g. api_key).
3. Use the returned connector_id in `create_pipeline` (connector_id) or `patch_pipeline_graph` (add connection node).

### Flow: OAuth connector (e.g. Shopify)

1. `list_connector_types` — confirm the type (e.g. shopify) and auth_type oauth2.
2. `get_connector_oauth_url` — pass connector_type and connection_config (e.g. shop: "store-name" from store.myshopify.com). User opens oauth_url in browser and completes flow.
3. After OAuth, connector is created; use connector_id in `create_pipeline` or add connection via `patch_pipeline_graph`.

### Flow: Apply connector blueprint

1. `list_blueprints` — optional; category "connectors" to see connector blueprints.
2. `apply_blueprint` — project_id, blueprint_type "connectors", blueprint_slug.

## Section B — Add New Connector Type (Platform Partners)

Adding a **new connector type** (new provider + blueprint to the platform catalog) is platform-level work. It is not available via the Customer MCP.

- **Platform partners / Loxtep contributors:** Use the internal Loxtep MCP (scaffold_connector_provider, scaffold_connector_blueprint) in the platform monorepo, or follow the guides ADDING_A_NEW_CONNECTOR_TYPE and ADDING_A_COMPOSIO_CONNECTOR.
- **Customers:** Use the existing connector types from `list_connector_types` and the flows in Section A. To request a new connector type, contact Loxtep.

## Auth

If any tool returns "No valid authentication token" or "RBAC requires JWT", run:

```bash
npx @loxtep/customer-mcp-server login
```

Then retry the tool call.

## Skill attribution (optional)

When calling MCP tools, you may pass `_metadata: { skill_name: 'create-connector' }` in tool args for per-skill eval attribution. Fully optional.
