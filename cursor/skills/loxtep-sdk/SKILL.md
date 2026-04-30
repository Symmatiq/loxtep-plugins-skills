---
name: loxtep-sdk
description: Bootstrap @loxtep/sdk in Node (auth, env, REST vs Loxtep streams), map MCP tools to SDK methods, and avoid mixing JWT with IAM. Use when adding Loxtep SDK usage, queue readers/writers, or pairing MCP with runtime code.
---

# Loxtep Node SDK (agent skill)

## Auth (single mental model)

1. `LOXTEP_AUTH_TOKEN` env var (CI/ephemeral), else  
2. `~/.loxtep/credentials.json` (`loxtep login`).

CLI/SDK do **not** auto-merge until one of these exists plus `api_url`.

## Copy-paste: client bootstrap

```ts
import { LoxtepClient } from '@loxtep/sdk';

export function createRuntimeClient() {
  return new LoxtepClient({
    api_url: process.env.LOXTEP_API_URL!.replace(/\/$/, ''),
    auth: { type: 'jwt', token: process.env.LOXTEP_AUTH_TOKEN! },
    organization_id: process.env.LOXTEP_ORGANIZATION_ID,
    project_id: process.env.LOXTEP_PROJECT_ID,
    instance_id: process.env.LOXTEP_INSTANCE_ID,
    default_bot_id: process.env.LOXTEP_BOT_ID,
    credentials: {
      accessKeyId: process.env.AWS_ACCESS_KEY_ID ?? 'cli',
      secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY ?? 'cli',
    },
  });
}
```

## Bootstrapping from an SDK Connector

If an SDK connector already exists (created via UI, API, or MCP — see the **`create-connector`** skill), export its `sdk_config` directly:

```bash
# Export as shell exports (source-able)
loxtep config export --from-connector "<connector_id>" --format sh

# Export as JSON (for programmatic use)
loxtep config export --from-connector "<connector_id>" --format json

# Export as .env file
loxtep config export --from-connector "<connector_id>" --format env
```

The `--from-connector` flag fetches the connector's `sdk_config` (containing `api_url`, `organization_id`, `project_id`, `instance_id`, `region`) and outputs it in the requested format.

**Quick start from a connector:**

```bash
# 1. Source the connector config into your shell
eval "$(loxtep config export --from-connector "<connector_id>" --format sh)"

# 2. SDK client picks up env vars automatically
node -e "
  const { LoxtepClient } = require('@loxtep/sdk');
  const client = new LoxtepClient();
  // client is configured from LOXTEP_* env vars
"
```

> To create an SDK connector in the first place, see the **`create-connector`** skill (Flow — SDK Connector).

## Shell exports from an existing data product

```bash
loxtep config export --from-data-product "<uuid>" --format sh
# or JSON for apps:
loxtep config export --from-data-product "<uuid>" --format json
```

## MCP vs SDK

- **MCP:** provisioning, catalog, agent tool calls over HTTP.  
- **SDK:** typed REST + **Loxtep streams** (live queues / bus).  
- See shipped docs in `@loxtep/sdk`: `docs/sdk-pairing.md`, `docs/sdk-mcp-mapping.md`.

## Stream vs replay

- **`data_products.stream`:** live bus when `bot_id` + stream bus config exists; otherwise error/missing queue.  
- **`data_products.replay`:** historical path via platform HTTP (observe/trace semantics).

## Bus credentials

JWT is **not** bus IAM. Use your instance’s stream resource env + AWS principal; future `loxtep bus login` will wrap RBAC issuance — until then see `docs/sdk-bus-rbac-threat-model.md`.
