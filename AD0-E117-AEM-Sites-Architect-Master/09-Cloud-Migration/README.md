# AEM as a Cloud Service — Migration & Architecture

[Back to AD0-E117 Index](../README.md)

---

## AEM 6.5 vs AEM as a Cloud Service — Complete Comparison

### Architecture Differences

| Aspect | AEM 6.5 (On-Prem/AMS) | AEM as a Cloud Service |
|---|---|---|
| **Infrastructure** | Self-managed or Adobe Managed | Cloud-native, fully Adobe-managed |
| **Scaling** | Manual vertical/horizontal | Auto-scaling based on demand |
| **Author instances** | Single (TarMK) or multi (MongoMK) | Multiple, auto-scaled, immutable |
| **Publish instances** | Manual configuration | Auto-scaled with subscription pattern |
| **CDN** | Customer-managed | Built-in (Fastly) |
| **Updates** | Manual service pack installation | Continuous zero-downtime updates |
| **Server access** | SSH, Felix Console, CRXDE | **No server access** (immutable) |

### Deployment Differences

| Aspect | AEM 6.5 | AEMaaCS |
|---|---|---|
| Server mutability | Mutable (runtime changes) | **Immutable** (no runtime changes) |
| OSGi config | Config files per run mode + Felix Console | Environment variables + codebase only |
| Code deployment | Manual or custom CI/CD | **Cloud Manager** CI/CD mandatory |
| Dispatcher config | Manual server config | In codebase, validated by SDK |
| Run modes | Custom run modes supported | **Fixed:** dev, stage, prod |
| CRXDE Lite | Available on author | **Author only** (read-mostly) |

### Content Distribution

| Aspect | AEM 6.5 | AEMaaCS |
|---|---|---|
| Replication | Traditional replication agents | **Sling Content Distribution** |
| Reverse replication | Supported | **Not supported** |
| Binary handling | Full binary replication | **Binaryless** (shared blob store) |
| Agent configuration | Manual per instance | Automatic (pipeline-based) |

### Feature Differences

| Feature | AEM 6.5 | AEMaaCS |
|---|---|---|
| Workflows | Full workflow engine | Limited; Adobe I/O Events + microservices |
| Customization depth | Deep (any bundle/config) | Restricted (shared infrastructure) |
| Available bundles | Full bundle set | 87 fewer bundles |
| Asset processing | Local processing | **Asset microservices** (serverless) |
| Forms | AEM Forms available | Separate Forms CS |
| Screens | AEM Screens available | Separate Screens CS |
| Communities | Available (deprecated) | **Removed** |

## Migration to AEM as a Cloud Service

### Content Transfer Tool (CTT)

```
Migration Flow:
┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│ Source AEM 6.x    │     │ Content Transfer  │     │ AEM as a Cloud   │
│ (On-Prem/AMS)    │ ──▶ │ Tool (CTT)        │ ──▶ │ Service          │
└──────────────────┘     └──────────────────┘     └──────────────────┘

Pre-Migration Checklist:
├── ✅ Disable all replication agents
├── ✅ Run datastore consistency check (oak-run)
├── ✅ Run offline compaction (oak-run)
├── ✅ Ensure free disk space ≥ 1.5x JCR size
├── ✅ Stop unnecessary background processes
├── ✅ Back up the source instance
├── ✅ Validate content structure against AEMaaCS requirements
└── ✅ Run Best Practices Analyzer (BPA)

CTT Phases:
1. Extraction (from source)
   └── Creates migration set from source AEM
2. Ingestion (to target)
   └── Imports migration set into AEMaaCS
3. Top-up (incremental)
   └── Transfer only delta changes since last extraction
```

### Best Practices Analyzer (BPA)

```
BPA Reports:
├── Code compatibility issues
│   ├── Deprecated APIs used
│   ├── Unsupported OSGi configurations
│   └── Mutable content in /apps
├── Content issues
│   ├── Content structure violations
│   ├── Unsupported node types
│   └── Large binary content
├── AEMaaCS incompatibilities
│   ├── Features not available in cloud
│   ├── Configuration changes needed
│   └── Custom run mode usage
└── Recommendations
    └── Specific remediation steps for each finding
```

### Code Migration Considerations

```
Code Changes for AEMaaCS:
├── OSGi Configurations
│   ├── Move all configs to codebase
│   ├── Replace runtime configs with env variables
│   └── Remove custom run modes (use dev/stage/prod)
├── Admin Sessions
│   ├── Replace with Service Users
│   └── Map services via Service User Mapper
├── Replication Code
│   ├── Replace replication APIs with Distribution APIs
│   └── Remove reverse replication code
├── Workflow Code
│   ├── Replace heavy workflows with Adobe I/O Events
│   └── Use asset microservices for asset processing
├── Dispatcher
│   ├── Restructure configs for AEMaaCS format
│   ├── Validate with Dispatcher Validator SDK
│   └── Remove unsupported Apache modules
└── Search
    ├── Deploy custom indexes via CI/CD
    ├── Convert property indexes to Lucene where needed
    └── Validate with Cloud Readiness Analyzer
```

## Cloud Manager

```
Cloud Manager Architecture:
├── Environments
│   ├── Development (dev)
│   ├── Stage (stage)
│   └── Production (prod)
├── CI/CD Pipelines
│   ├── Full-Stack Pipeline (code + content)
│   ├── Front-End Pipeline (clientlibs/themes only)
│   ├── Config Pipeline (CDN/Dispatcher configs)
│   └── Web Tier Config Pipeline (Dispatcher-only)
├── Quality Gates
│   ├── Code Quality (SonarQube-based)
│   ├── Performance Testing (load test)
│   ├── Security Testing
│   ├── Custom Functional Tests
│   └── Experience Audit (Lighthouse)
├── Environment Variables
│   ├── Standard (non-secret)
│   └── Secret (encrypted)
└── Monitoring
    ├── New Relic integration
    ├── Log management
    └── Alerting
```

### Architect's Cloud Migration Decision Framework

```
Should you migrate to AEMaaCS?
├── New project (greenfield)?
│   └── YES → AEMaaCS (default for new projects)
├── Existing AEM 6.5?
│   ├── Heavy customization of JCR/bundles?
│   │   └── Significant refactoring needed — plan carefully
│   ├── Using Communities/Forms/Screens?
│   │   └── Separate cloud products needed
│   ├── Reverse replication required?
│   │   └── Architecture redesign needed
│   ├── Strict data sovereignty?
│   │   └── Check AEMaaCS region availability
│   └── Otherwise?
│       └── Migrate — benefits outweigh costs
└── AEM 6.5 LTS
    └── Available for organizations not yet ready for cloud
    └── Extended support timeline
```

---

### Practice Scenarios

**Scenario 1:** During implementation of a public-facing website on AEM as a Cloud Service, the customer needs commenting functionality for end users. What should the Architect recommend?

<details>
<summary>Answer</summary>

**Integrate a third-party solution to store comments externally.**

Reasoning:
- AEMaaCS does not support reverse replication (comments from publish → author)
- JCR on publish is not designed for user-generated content at scale
- MongoDB is NOT provided with AEMaaCS (it uses TarMK/Oak)
- Sling Distribution doesn't support publish-to-author content flow

Solution:
- Use a third-party commenting service (Disqus, Coral, custom)
- Store comments in external database (MongoDB, DynamoDB, etc.)
- Load comments via client-side AJAX
- Moderate through external admin interface or integration
</details>

**Scenario 2:** Moving content from AEM 6.5 on-premise to AEM as a Cloud Service. What actions must be performed before extraction with the Content Transfer Tool?

<details>
<summary>Answer</summary>

**Two critical pre-extraction actions:**

1. **Disable all replication agents** — Prevents content changes during migration that could cause inconsistencies

2. **Run datastore consistency check via oak-run** — Identifies and resolves missing or corrupt binary references in the datastore before transfer

Additional recommended steps:
- Run offline compaction (reduces transfer size)
- Verify sufficient disk space (≥ 1.5x JCR size)
- Run Best Practices Analyzer
- Create a backup of the source instance
</details>

---

*[Back to AD0-E117 Index](../README.md)*
