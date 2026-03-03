# AEM Architecture Fundamentals

[Back to AD0-E117 Index](../README.md)

---

## AEM Technology Stack

```
┌───────────────────────────────────────────────┐
│                  AEM Application               │
│  (Sites, Assets, Forms, Commerce, Screens)     │
├───────────────────────────────────────────────┤
│              AEM Platform Layer                │
│  (Workflows, Search, Tagging, i18n, DAM)      │
├───────────────────────────────────────────────┤
│              Apache Sling                      │
│  (Resource Resolution, Servlets, Models)       │
├───────────────────────────────────────────────┤
│              Apache Jackrabbit Oak             │
│  (JCR Implementation, Query, Indexing)         │
├───────────────────────────────────────────────┤
│              OSGi Framework (Felix)            │
│  (Bundle Management, Services, Configurations) │
├───────────────────────────────────────────────┤
│              Java Virtual Machine (JVM)         │
│  (Memory Management, GC, Threading)            │
└───────────────────────────────────────────────┘
```

## Deployment Topologies

### TarMK (Recommended Default)

```
┌──────────────────┐     ┌──────────────────┐
│   Author (Primary)│     │  Author (Standby) │
│   TarMK           │────▶│  Cold Standby     │
│                    │sync │                    │
└────────┬───────────┘     └──────────────────┘
         │ Replication
         ▼
┌──────────────────┐     ┌──────────────────┐
│  Publish 1        │     │  Publish 2        │
│  TarMK            │     │  TarMK            │
└────────┬───────────┘     └────────┬──────────┘
         │                          │
         ▼                          ▼
┌──────────────────────────────────────────────┐
│              Dispatcher / CDN                 │
│         (Load Balanced, Cached)               │
└──────────────────────────────────────────────┘
```

**Use When:**
- Typical authoring loads (< 100 concurrent authors)
- Single-server author tier sufficient
- Simpler to operate and maintain
- Recommended for most deployments

### MongoMK (Horizontal Author Scaling)

```
┌──────────┐ ┌──────────┐ ┌──────────┐
│ Author 1  │ │ Author 2  │ │ Author 3  │
│ MongoMK   │ │ MongoMK   │ │ MongoMK   │
└─────┬─────┘ └─────┬─────┘ └─────┬─────┘
      │              │              │
      ▼              ▼              ▼
┌──────────────────────────────────────────┐
│          MongoDB Replica Set             │
│  (Primary + 2 Secondaries, odd count)    │
└──────────────────────────────────────────┘
```

**Use When:**
- 100+ concurrent authors around the clock
- 30+ simultaneous editing sessions
- Single server CPU/memory insufficient
- Geographic distribution of authoring required

### AEM as a Cloud Service

```
┌──────────────────────────────────────────────┐
│              Cloud Manager                    │
│  (CI/CD Pipelines, Environments, Monitoring) │
└──────────────┬───────────────────────────────┘
               │ Deploy
               ▼
┌──────────────────┐     ┌──────────────────────┐
│  Author Tier      │     │  Publish Tier         │
│  (Auto-scaled)    │     │  (Auto-scaled)        │
│  Immutable        │     │  Immutable             │
└────────┬──────────┘     └────────┬───────────────┘
         │ Sling Distribution       │
         ▼                          ▼
         └──────────────┬───────────┘
                        ▼
                ┌──────────────┐
                │  Fastly CDN  │
                │  (Built-in)  │
                └──────────────┘
```

**Key Differences:**
- Immutable infrastructure (no runtime Felix Console changes)
- Auto-scaling (no manual instance management)
- Sling Content Distribution (replaces traditional replication)
- Cloud Manager mandatory for all deployments
- Continuous updates from Adobe (zero-downtime)

### Content Sharding

```
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│ Author EMEA   │  │ Author APAC   │  │ Author AMER   │
│ (Regional)    │  │ (Regional)    │  │ (Regional)    │
└───────┬───────┘  └───────┬───────┘  └───────┬───────┘
        │                  │                   │
        ▼                  ▼                   ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│ Publish EMEA  │  │ Publish APAC  │  │ Publish AMER  │
└──────────────┘  └──────────────┘  └──────────────┘
```

**Use When:**
- Legal requirements prevent content sharing between regions
- Editors must work globally without cross-region maintenance impact
- Strict data sovereignty requirements

## Topology Selection Decision Tree

```
How many concurrent authors?
├── < 100 → TarMK (single author + cold standby)
│   └── Need HA? → Cold Standby for failover
└── 100+ → Is cloud an option?
    ├── YES → AEM as a Cloud Service (auto-scaled)
    └── NO → MongoMK with MongoDB Replica Set
        └── Regional content isolation needed?
            └── YES → Content Sharding
```

## AEM Instance Types

| Instance | Role | Access |
|---|---|---|
| **Author** | Content creation, editing, workflows | Internal users (authentication required) |
| **Publish** | Content delivery to end users | Public-facing (anonymous access) |
| **Dispatcher** | Cache, load balance, security | First point of contact for requests |
| **Preview** | Content preview before publish | Internal review (optional in AEMaaCS) |

## Design Patterns in AEM

### Model-Component-View (MCV)

```
Sling Model (Data + Logic)
    ↕ adapts from
Resource (JCR Node)
    ↕ resolved by
HTL Template (Presentation)
    = rendered as
HTML Output
```

### Component Architecture Patterns

| Pattern | Description | Use Case |
|---|---|---|
| **Proxy Pattern** | `/apps/myproject/components/text` extends Core Component | Standard component customization |
| **Composite Pattern** | Page contains components containing sub-components | Page structure |
| **Singleton Pattern** | OSGi service as singleton | Shared services |
| **Factory Pattern** | Component factory for service variants | Multiple service implementations |
| **Adapter Pattern** | Sling resource adaptation (`resource.adaptTo()`) | Data access |
| **Whiteboard Pattern** | OSGi service registration | Event handlers |
| **Observer Pattern** | Event listeners and handlers | Workflow triggers |

> **Architect Warning:** Never read the repository in an OSGi service's `activate()` method — it may not be available yet during bundle activation. Use lazy initialization instead.

---

*[Back to AD0-E117 Index](../README.md)*
