---
name: loxtep-sdk
description: Bootstrap @loxtep/sdk in Node (auth, env, REST vs Loxtep streams), map MCP tools to SDK methods, and avoid mixing JWT with IAM. Use when adding Loxtep SDK usage, queue readers/writers, or pairing MCP with runtime code.
---

# Loxtep Node SDK (agent skill)

## Auth (single mental model)

1. `LOXTEP_AUTH_TOKEN` (CI/ephemeral), else  
2. `~/.loxtep/credentials.json` (`loxtep login`), else  
3. `~/.loxtep/customer-mcp.json` (`npx @loxtep/customer-mcp-server login`) — map `api_base_url` → `api_url`.

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
