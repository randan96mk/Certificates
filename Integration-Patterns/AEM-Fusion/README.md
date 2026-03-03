# AEM + Fusion Integration Patterns

[Back to Integration Patterns](../README.md) | [Back to Main README](../../README.md)

---

## Overview

While AEM and Workfront have a native connector, AEM and Fusion integrations are typically built via AEM's REST APIs and Adobe I/O Events.

## Integration Architecture

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│    Fusion     │     │   AEM API    │     │   AEM        │
│   Scenario    │────▶│  (REST/      │────▶│  Author/     │
│               │     │   GraphQL)   │     │  Publish     │
└──────────────┘     └──────────────┘     └──────────────┘
       │
       │  Also connects to:
       ├──▶ Adobe I/O Events (event-driven triggers)
       ├──▶ AEM Assets HTTP API (/api/assets)
       ├──▶ AEM GraphQL API (/graphql/execute.json)
       └──▶ AEM Workflow API (/etc/workflow)
```

## Common Patterns

### Pattern 1: Asset Metadata Enrichment

```
Trigger: Fusion scheduled (every 15 min)
  → Search AEM Assets: new assets without metadata
    (GET /api/assets/content/dam/inbox?orderby=jcr:created)
  → For each new asset:
    → Get PIM data (external system API call)
    → Update AEM metadata:
      (PUT /api/assets/{path} with metadata JSON)
    → Move asset to appropriate folder
    → Trigger AEM processing workflow
```

### Pattern 2: Content Fragment Management

```
Trigger: External system webhook (e.g., PIM product update)
  → Transform product data to CF model format
  → Check if CF exists:
    (GET /api/assets/content/dam/products/{sku}.json)
  → Router:
    ├── CF exists → Update CF
    │   (PUT /api/assets/content/dam/products/{sku})
    └── CF doesn't exist → Create CF
        (POST /api/assets/content/dam/products
         with Content-Type: application/json)
  → Trigger AEM page regeneration if needed
```

### Pattern 3: AEM Workflow Trigger via Fusion

```
Trigger: Workfront proof approved (via WF event subscription)
  → Get proof details and asset path
  → Call AEM Workflow API:
    POST /etc/workflow/instances
    Body: {
      "model": "/var/workflow/models/dam/update_asset",
      "payloadType": "JCR_PATH",
      "payload": "/content/dam/approved/{asset-name}"
    }
  → Monitor workflow completion
  → Update Workfront task status on completion
```

### Pattern 4: Scheduled Content Publishing

```
Trigger: Fusion scheduled (from Workfront launch dates)
  → Search WF projects: Launch date = today
  → For each project:
    → Get AEM page paths from WF custom field
    → Activate AEM pages:
      POST /bin/replicate
      Body: {
        "cmd": "Activate",
        "path": "/content/mysite/campaigns/{page}"
      }
  → Update WF project status to "Published"
  → Send Slack notification
```

## AEM API Reference for Fusion

| API | Endpoint | Use |
|---|---|---|
| Assets HTTP API | `/api/assets/*` | CRUD assets and metadata |
| GraphQL | `/graphql/execute.json/{endpoint}` | Query Content Fragments |
| Workflow | `/etc/workflow/*` | Trigger and monitor workflows |
| Replication | `/bin/replicate` | Activate/deactivate content |
| User/Group | `/libs/granite/security/*` | User management |
| Query Builder | `/bin/querybuilder.json` | Search content |
| Package Manager | `/crx/packmgr/*` | Package operations |

### Authentication for Fusion → AEM

```
AEM 6.5:
├── Basic Auth (development only)
├── OAuth 2.0 token
└── Service account credentials

AEM as a Cloud Service:
├── Adobe IMS Service Account (JWT/OAuth)
├── Technical Account via Adobe Developer Console
└── Access token refreshed automatically by Fusion
```

---

*[Back to Integration Patterns](../README.md)*
