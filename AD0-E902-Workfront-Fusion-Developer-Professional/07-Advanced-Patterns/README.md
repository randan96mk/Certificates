# Advanced Fusion Patterns & Best Practices

[Back to AD0-E902 Index](../README.md)

---

## Architecture Patterns for Architects

### Pattern 1: Hub-and-Spoke Integration

```
                    ┌──────────┐
                    │ Workfront │
                    │  (Hub)    │
                    └─────┬────┘
                          │
           ┌──────────────┼──────────────┐
           │              │              │
     ┌─────▼────┐  ┌─────▼────┐  ┌─────▼────┐
     │  Jira    │  │  Slack   │  │  AEM     │
     │ (Spoke)  │  │ (Spoke)  │  │ (Spoke)  │
     └──────────┘  └──────────┘  └──────────┘

Hub Design:
├── Workfront is the system of record
├── Fusion scenarios radiate outward (hub → spokes)
├── Each spoke has dedicated scenarios
├── Data Store tracks sync state per spoke
└── Central error notification for all scenarios
```

### Pattern 2: Event-Driven Architecture

```
Event Sources → Event Bus (Fusion) → Event Consumers

Source Scenarios (Producers):
├── Watch WF Events → Parse → Data Store: Add Event
├── Webhook from Jira → Parse → Data Store: Add Event
└── Schedule: Poll Salesforce → Data Store: Add Event

Consumer Scenarios (Processors):
├── Schedule: Get Unprocessed Events →
│   Iterator → Router → Process by type →
│   Mark as processed
└── Separate consumers for different event types

Benefits:
├── Decoupled systems
├── Resilient (events stored, retried)
├── Scalable (add consumers independently)
└── Auditable (event log in data store)
```

### Pattern 3: Chain Scenarios (Parent-Child)

```
Parent Scenario:
  Trigger → Prepare Data → Call Child Scenario → Process Result

Child Scenario:
  Webhook Trigger → Business Logic → Webhook Response

How to Chain:
├── Child has Custom Webhook trigger
├── Parent uses HTTP module to call child's webhook URL
├── Child returns result via Webhook Response
├── Parent receives and continues processing

Benefits:
├── Reuse child logic across multiple parent scenarios
├── Simpler maintenance (update child, all parents benefit)
├── Independent error handling per scenario
└── Better organization for complex workflows
```

### Pattern 4: Idempotent Processing

```
Problem: Webhook fires twice for same event
Solution: Idempotent design with Data Store

Webhook → Data Store: Get Record (key = event.id)
  ├── Exists → Skip (already processed)
  └── Not Exists →
      Data Store: Add Record (key = event.id, value = "processing")
      → Process Event
      → Data Store: Update Record (value = "completed")

Key Rules:
├── Use unique event ID as data store key
├── Check before processing, not after
├── Handle race conditions (lock mechanism)
└── Periodically clean old records from data store
```

### Pattern 5: Graceful Degradation

```
Primary Path with Fallback:
  HTTP: Call Primary API
    │
    ├── Success (2xx) → Process response
    │
    └── Error handler:
        HTTP: Call Fallback API
          │
          ├── Success → Process fallback response
          │
          └── Error handler:
              Use Cached Data (from Data Store)
              → Process cached response
              → Send alert: "Using stale data"
```

## Operations & Cost Optimization

```
Reducing Operations:
├── Use webhooks instead of polling (trigger is free)
├── Aggregate before acting (10 items → 1 email, not 10 emails)
├── Filter early (before expensive modules)
├── Cache with Data Stores (avoid repeated API lookups)
├── Set appropriate polling intervals (not too frequent)
├── Use search filters in trigger (don't fetch everything)
└── Batch operations where API supports (bulk create)

Reducing Execution Time:
├── Parallel paths (Router) for independent actions
├── Avoid unnecessary Sleep modules
├── Use aggregator to reduce downstream operations
├── Limit result sets in searches
└── Optimize JSON payloads (don't send unnecessary data)
```

## Security Best Practices

```
Scenario Security:
├── Never hardcode credentials (use Connections)
├── Use OAuth 2.0 over API keys when available
├── Restrict webhook access (IP whitelist)
├── Don't log sensitive data in notes/notifications
├── Use encrypted data stores for sensitive values
├── Regularly rotate connection credentials
├── Review scenario access per team
└── Audit execution history for anomalies
```

---

*[Back to AD0-E902 Index](../README.md)*
