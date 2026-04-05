# Loxtep plugins — skills roadmap

Canonical MCP grouping: **16** `loxtep_*` tools, each call sets **`operation`** to a flat tool name plus arguments. See the [Customer MCP Server README](https://github.com/symmatiq/loxtep/blob/main/platform-backend/_customer-mcp-server/README.md).

## Current skills

| Skill slug        | Role |
| ----------------- | ---- |
| `loxtep-auth`     | Recover when MCP returns missing JWT / auth errors (`npx @loxtep/customer-mcp-server login`). |
| `create-connector`| Connectors (`loxtep_connectors`), connections (`loxtep_connections`), connector-related templates (`loxtep_templates`). |
| `data-workflows`  | Breadth: projects, templates, workflows graph, connections, data products, connectors — common Studio-style flows. |

## Candidate skills (future)

Add focused skills when agents repeatedly mis-route or need deeper playbooks. Prefer **one or two** new skills per iteration to avoid skill-list noise.

| Candidate slug        | Primary MCP facade(s)      | Typical user intent |
| --------------------- | -------------------------- | ------------------- |
| `catalog-discovery`   | `loxtep_catalog`           | Catalog search, evidence, lineage, domains/tags, `run_discovery` sequencing. |
| `analytics-query`     | `loxtep_analytics`         | Execute query, list tables, schema, poll results. |
| `definitions-quality`| `loxtep_schemas`, `loxtep_quality` | Definitions, PII tagging, quality rules. |
| `workspace-snapshots` | `loxtep_workspace`       | Versions, snapshots, reindex, queue info, replay. |
| `process-intelligence` | `loxtep_process_intel`  | Entity context, ontology, thesaurus. |
| `procedures`          | `loxtep_procedures`        | Procedure CRUD / process graph procedures. |
| `agent-workspace`     | `loxtep_agent_workspace`   | `agent_orchestration_*` — distinct from data **workflow** projects. |
| `session-org`         | `loxtep_session`, `loxtep_instances` | Current user/org context; careful guidance for `create_instance`. |

## Recommended next additions

1. **`catalog-discovery`** — high frequency for Discover / “find data” flows.  
2. **`agent-workspace`** — disambiguates agent orchestration from `loxtep_workflows`.

Scaffold new skills like [create-connector](../cursor/skills/create-connector/SKILL.md): frontmatter `name`, triggers, `loxtep_*` + `operation` tables, optional `_metadata.skill_name`, auth pointer.
