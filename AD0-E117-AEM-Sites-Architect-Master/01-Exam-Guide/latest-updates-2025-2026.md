# AD0-E117 — Latest Exam & Product Updates (2025-2026)

[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E117 Index](../README.md)

---

## Exam Administration Changes

| Change | Details |
|---|---|
| **Proctoring Platform** | Now **Meazure Learning / ProctorU** with Guardian browser (replaced Examity) |
| **Scheduling** | Up to 60 days in advance via [certification.adobe.com](https://certification.adobe.com/) |
| **Summit 2026** | Free exam with full conference pass (April 19-22, Las Vegas) |
| **Renewal Status** | **ON HOLD** — Adobe is overhauling the renewal process. Monitor the certification portal. |
| **Edge Delivery Services** | NEW separate certification (EDS-D200, Professional level) — AD0-E117 retains traditional AEM focus |

> **Important:** The new Edge Delivery Services Developer Professional certification (EDS-D200) is a separate path. This means AD0-E117 likely retains its focus on traditional AEM 6.5 / AEMaaCS Sites architecture rather than being heavily updated with EDS content.

---

## Community Exam Advice (Latest)

Based on recent [Experience League discussions](https://experienceleaguecommunities.adobe.com/t5/adobe-experience-manager/ad0-e117-exam-prep-how-deep-should-i-go-into-cloud-manager-amp/m-p/784179):

### Cloud Manager Depth
- **Architect-level understanding** required, NOT deep hands-on DevOps
- Focus on **conceptual/design-level** knowledge: why and when to use pipelines
- Know purpose of each stage (build, code quality, security, performance testing, deploy)
- Understand how quality gates ensure stability
- Know deployment types (full stack, front-end, configuration) and when each is appropriate
- Be able to **select and justify** environment types and scaling options

### Question Style
- Questions emphasize **architectural reasoning** applied to unfamiliar contexts
- Not memorization — ability to apply principles to new scenarios
- "As an Architect, what do you recommend?" format
- Multiple correct answers may exist — pick the BEST per Adobe best practices

---

## New AEM as a Cloud Service Features (2025-2026)

### AEM 2026.3.0 (March 26, 2026)

| Feature | Architect Impact |
|---|---|
| **Content Fragment check-out/check-in** | Concurrent editing control — consider for multi-author CF workflows |
| **TipTap editor (replacing TinyMCE)** | Unified rich text across Universal Editor and CF editors |
| **Content Advisor in AEM Sites** | AI-powered intelligent asset discovery for authors |
| **Dynamic Media Template Editor** | Enhanced on-the-fly media transformations |
| **Simplified JSON-based index management** | Easier custom Oak index definitions |
| **Cloud Manager MCP Server** | Interact with Cloud Manager via AI-powered IDEs (natural language) |
| **Java API deprecations** | March 30, 2026 deadline — audit custom code |

### AEM 2026.2.0

| Feature | Architect Impact |
|---|---|
| **CF Management with OpenAPI** | Programmatic CF management via standardized APIs |
| **Launches with OpenAPI** | API-driven Launch management |
| **Quiet Hours & Update-Free Periods (GA)** | Schedule maintenance windows — no Adobe updates during critical periods |
| **Content Advisor in Adobe Express** | Cross-product content intelligence |

### Cloud Manager 2026.4.0 (April 2, 2026)

```
Key Updates:
├── Edge Delivery Services with AEM Authoring mode
│   └── NEW: Optional publish tier when Edge Delivery handles delivery
├── Incremental Builds
│   └── Module-level caching for shorter build times
│   └── Significant CI/CD performance improvement
├── MCP Server
│   └── AI IDE interaction with Cloud Manager
│   └── Natural language DevOps commands
└── Environment Flexibility
    └── Publish tier optional with EDS
    └── Cost optimization potential
```

### AEM 2025.12.0

| Feature | Architect Impact |
|---|---|
| **Automatic malware scanning** | All uploaded assets auto-scanned — security improvement |
| **AI-generated metadata (all customers)** | No GenAI Rider required — broader adoption |

### Beta Programs (May Appear on Future Exams)

```
Beta Features to Watch:
├── IDE AI Tooling
│   └── AI-assisted Java and Dispatcher development
├── AEM Edge Functions
│   └── CDN-layer JavaScript execution
│   └── Compute at the edge without full publish tier
├── Canary Production Deployments
│   └── Pre-release validation in production
│   └── Rolling deployments for zero-downtime testing
└── RDE Snapshots
    └── Save/restore Rapid Development Environment state
    └── Code + content state management
```

---

## Edge Delivery Services (Separate Cert — Know Conceptually)

```
Edge Delivery Services Architecture:
├── Serverless micro-services architecture
├── Authoring Methods:
│   ├── Document-based (Google Docs / Microsoft Word)
│   └── Universal Editor (WYSIWYG with AEM content)
├── Content Delivery:
│   ├── Pre-rendered static HTML at the edge
│   ├── Sub-second page loads
│   └── Perfect Lighthouse scores (target)
├── Development:
│   ├── Simple HTML/CSS/JS
│   ├── No complex build pipeline needed
│   └── GitHub-based deployment
└── Architecture Implications:
    ├── Publish tier becomes optional
    ├── Dispatcher role reduced/eliminated
    ├── CDN-first delivery model
    └── Different from traditional AEM Sites architecture

Exam Impact for AD0-E117:
├── Know conceptually what EDS is
├── Understand when EDS vs traditional AEM is appropriate
├── NOT expected to know EDS implementation details
└── The separate EDS-D200 cert covers implementation depth
```

### When to Recommend EDS vs Traditional AEM

| Criteria | Edge Delivery Services | Traditional AEM Sites |
|---|---|---|
| **Content complexity** | Simple to moderate | Complex, structured |
| **Authoring needs** | Non-technical authors | Technical content authors |
| **Performance priority** | Ultra-fast (sub-second) | Standard (< 3s) |
| **Customization depth** | Limited | Deep |
| **Integration needs** | Light | Heavy |
| **Existing AEM investment** | New project | Large existing codebase |
| **Development skills** | Frontend web dev | Full-stack AEM dev |

---

## Updated AEM Architecture Decision Table

### Deployment Model (2026 Update)

```
New project — what deployment model?
├── Simple marketing site, speed priority?
│   └── Edge Delivery Services
├── Complex site, deep AEM features needed?
│   └── AEM as a Cloud Service (traditional)
├── Existing AEM 6.5, stable, no urgency?
│   └── AEM 6.5 LTS (extended support)
├── Existing AEM 6.5, needs modernization?
│   └── Phased migration to AEMaaCS
├── Hybrid: marketing pages fast + complex app?
│   └── EDS for marketing pages + AEMaaCS for complex sections
└── Strict data sovereignty, no cloud?
    └── AEM 6.5 on-premise (rare case)
```

---

## Java API Deprecation Alert (March 30, 2026)

```
⚠️ CRITICAL for existing AEMaaCS implementations:

Deprecated APIs (deadline March 30, 2026):
├── Review all custom bundles for deprecated API usage
├── Check AEM SDK changelogs for affected classes
├── Use the Cloud Readiness Analyzer for automated detection
└── Refactor before deadline to avoid build failures

This is a real-world architect concern that may appear
in scenario-based exam questions about maintenance/migration.
```

---

*[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E117 Index](../README.md)*
