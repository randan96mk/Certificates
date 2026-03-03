# Architecture Deep Dives — Cross-Cutting Topics

[Back to Main README](../README.md)

---

## Topics

1. [Enterprise Architecture Patterns](#enterprise-architecture-patterns)
2. [Security Architecture Across Products](#security-architecture)
3. [Performance Engineering](#performance-engineering)
4. [Migration & Modernization](#migration--modernization)

---

## Enterprise Architecture Patterns

### Pattern 1: Centralized Content Hub

```
┌──────────────────────────────────────────────────────────┐
│                    AEM (Content Hub)                       │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐               │
│  │  Sites    │  │   DAM    │  │   CFs    │               │
│  │ (Pages)   │  │ (Assets) │  │ (Headless)│               │
│  └─────┬─────┘  └─────┬────┘  └─────┬────┘               │
│        │              │              │                    │
│        ▼              ▼              ▼                    │
│  ┌──────────────────────────────────────────────┐        │
│  │              Delivery Layer                   │        │
│  │  CDN + Dispatcher + Dynamic Media + GraphQL  │        │
│  └──────────────────────────────────────────────┘        │
└──────────────────────────────────────────────────────────┘
         │              │              │
    ┌────▼───┐    ┌────▼───┐    ┌────▼───┐
    │  Web   │    │ Mobile │    │ Kiosk  │
    │  Site  │    │  App   │    │  App   │
    └────────┘    └────────┘    └────────┘

Workfront: Manages the production pipeline
Fusion: Orchestrates cross-system workflows
```

### Pattern 2: Federated Content Architecture

```
Multi-Brand / Multi-Region:
┌───────────────────┐  ┌───────────────────┐
│ Region A (EMEA)    │  │ Region B (APAC)    │
│ ┌───────────────┐ │  │ ┌───────────────┐ │
│ │ AEM Author    │ │  │ │ AEM Author    │ │
│ │ (Content Shard)│ │  │ │ (Content Shard)│ │
│ └───────┬───────┘ │  │ └───────┬───────┘ │
│         ▼         │  │         ▼         │
│ ┌───────────────┐ │  │ ┌───────────────┐ │
│ │ AEM Publish   │ │  │ │ AEM Publish   │ │
│ └───────┬───────┘ │  │ └───────┬───────┘ │
│         ▼         │  │         ▼         │
│ ┌───────────────┐ │  │ ┌───────────────┐ │
│ │ Dispatcher    │ │  │ │ Dispatcher    │ │
│ └───────────────┘ │  │ └───────────────┘ │
└───────────────────┘  └───────────────────┘
         │                       │
         ▼                       ▼
┌──────────────────────────────────────────┐
│           Global CDN (Fastly)            │
│  Geographic routing to nearest region     │
└──────────────────────────────────────────┘

Workfront: Regional PMOs with global oversight
Fusion: Cross-region data synchronization
```

### Pattern 3: Composable Architecture

```
Microservices + AEM:
┌─────────────────────────────────────────────┐
│           Experience Layer (AEM)             │
│  Pages | SPAs | Experience Fragments         │
└──────────────┬──────────────────────────────┘
               │ API calls
┌──────────────▼──────────────────────────────┐
│           API Gateway                        │
└──┬───────┬───────┬───────┬───────┬──────────┘
   │       │       │       │       │
┌──▼──┐ ┌──▼──┐ ┌──▼──┐ ┌──▼──┐ ┌──▼──┐
│Auth │ │Cart │ │Prod │ │User │ │Search│
│Svc  │ │Svc  │ │Svc  │ │Svc  │ │Svc  │
└─────┘ └─────┘ └─────┘ └─────┘ └─────┘

AEM: Experience/content layer
Microservices: Business logic + data
Fusion: Service orchestration
Workfront: Project management for all services
```

---

## Security Architecture

### Cross-Product Security Model

```
Identity & Access:
┌──────────────────────────────────────────────────┐
│                Identity Provider (IdP)             │
│          (Okta / Azure AD / ADFS)                 │
└─────┬──────────────┬────────────────┬─────────────┘
      │ SAML 2.0     │ OAuth 2.0      │ API Key
      ▼              ▼                ▼
┌──────────┐   ┌──────────┐    ┌──────────┐
│ AEM       │   │ Workfront│    │ Fusion   │
│ (Publish) │   │          │    │          │
│ SAML SSO  │   │ SSO      │    │ API Auth │
└──────────┘   └──────────┘    └──────────┘

AEM Author: Adobe IMS
AEM Publish: SAML 2.0 (or generic SSO)
Workfront: SAML 2.0 SSO
Fusion: OAuth 2.0 connections per service
```

### Defense in Depth

```
Layer 1: Network (Firewall, WAF, DDoS protection)
    │
Layer 2: CDN (SSL termination, edge rules)
    │
Layer 3: Dispatcher (URL filtering, header management)
    │
Layer 4: AEM Application (ACLs, CUG, CSRF, XSS protection)
    │
Layer 5: Data (Encryption at rest, access controls)
    │
Layer 6: Monitoring (Audit logs, Security Health Check)
```

---

## Performance Engineering

### Performance Budget Template

| Metric | Target | Measurement |
|---|---|---|
| **TTFB** | < 500ms | Server-side time to first byte |
| **FCP** | < 1.5s | First Contentful Paint |
| **LCP** | < 2.5s | Largest Contentful Paint |
| **CLS** | < 0.1 | Cumulative Layout Shift |
| **FID** | < 100ms | First Input Delay |
| **Cache Hit Ratio** | > 95% | CDN + Dispatcher cache |
| **Error Rate** | < 0.1% | Server-side errors |
| **Uptime** | > 99.9% | Availability |

### Performance Troubleshooting Decision Tree

```
Page loads slowly?
├── Is page cached at CDN/Dispatcher?
│   ├── NO → Fix caching config (most common fix)
│   └── YES → Is it a cache MISS?
│       ├── YES → Check invalidation frequency, statfileslevel
│       └── NO → Continue...
├── Is the uncached request slow from AEM?
│   ├── Check query performance (traversal queries?)
│   ├── Check component rendering time
│   ├── Check external service calls (blocking?)
│   └── Check JVM heap/GC pressure
├── Is it client-side slowness?
│   ├── Large unoptimized images
│   ├── Render-blocking JS/CSS
│   ├── Too many HTTP requests
│   └── Third-party scripts blocking
└── Network issues?
    ├── CDN misconfiguration
    ├── SSL negotiation overhead
    └── Geographic distance without CDN
```

---

## Migration & Modernization

### Migration Decision Matrix

| Scenario | Recommendation |
|---|---|
| New greenfield project | AEM as a Cloud Service |
| AEM 6.4 → need latest features | Upgrade to AEM 6.5, then plan AEMaaCS migration |
| AEM 6.5 stable, no cloud urgency | AEM 6.5 LTS (extended support) |
| AEM 6.5 + heavy customization | Phased migration to AEMaaCS (refactor gradually) |
| Heavy reverse replication usage | Redesign UGC architecture before migrating |
| Heavy Forms/Screens usage | Evaluate separate cloud products |
| Multi-region data sovereignty | Verify AEMaaCS region availability |

### Modernization Path

```
Phase 1: Assessment
├── Run Best Practices Analyzer
├── Audit custom code
├── Document integration dependencies
└── Calculate migration effort

Phase 2: Preparation
├── Refactor deprecated APIs
├── Replace admin sessions with service users
├── Move OSGi configs to codebase
├── Replace reverse replication patterns
└── Create custom Oak indexes in code

Phase 3: Migration
├── Set up Cloud Manager environments
├── Deploy code via CI/CD pipeline
├── Migrate content via Content Transfer Tool
├── Configure integrations (SAML, third-party)
└── Run top-up transfer for delta content

Phase 4: Validation
├── Functional testing
├── Performance testing (load + stress)
├── Security testing (penetration)
├── UAT with content authors
└── Parallel running period

Phase 5: Cutover
├── DNS switch
├── CDN warm-up
├── Monitor post-cutover
├── Decommission old environment
└── Documentation and training
```

---

*[Back to Main README](../README.md)*
