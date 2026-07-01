# AD0-E117 — Latest Exam & Product Updates (2025-2026)

[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E117 Index](../README.md)

---

## Exam Administration Changes

| Change | Details |
|---|---|
| **Proctoring Platform** | Now **Meazure Learning / ProctorU** with Guardian browser (replaced Examity) |
| **Scheduling** | Up to 60 days in advance via [certification.adobe.com](https://certification.adobe.com/) |
| **Summit 2026** | Free exam with full conference pass (April 19-22, Las Vegas) |
| **Renewal Status** | **RESUMED** (~March 2025) — simplified module-based renewal (two ~15-min modules), free, 2-year validity. Note: some Experience League "renew" pages still show a stale "on hold" banner — trust the live portal. |
| **Edge Delivery Services** | NEW separate certification (EDS-D200, Professional level, [course 1308](https://certification.adobe.com/courses/1308)) — AD0-E117 retains traditional AEM focus |

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

## AEM 6.5 LTS & End-of-Life Timeline (Architect-Critical)

```
AEM 6.5 LTS (Long-Term Support):
├── GA: March 7, 2025
├── Marketing name "AEM 6.5 LTS" = internal/technical version 6.6
│   └── UberJar defined as 6.6.0
├── Latest service pack: SP2 = version 6.6.2 (released Feb 19, 2026)
│   └── Delivered as a Quickstart JAR (not the traditional ZIP)
├── Supported Java: Java 17 AND Java 21 (both recommended; Java 11 dropped)
└── Positioning: the on-prem/AMS path forward for customers not moving
    to AEM as a Cloud Service yet

AEM 6.5 (classic, non-LTS) end-of-life:
├── Core/general support ends February 28, 2027 (official Adobe FAQ)
├── Extended support reported through Feb 2028 (secondary sources)
├── AMS hosting of 6.5 reported to end Aug 31, 2026 (secondary — verify)
└── Path forward from classic 6.5: AEM as a Cloud Service OR AEM 6.5 LTS
```

> **Exam Impact:** Migration/maintenance scenario questions may hinge on knowing that classic 6.5 support ends **Feb 28, 2027**, and that **6.5 LTS (v6.6)** is the modern on-prem option requiring **Java 17/21**. AEMaaCS remains the strategic default for new projects.

---

## Deprecated Java API Enforcement Timeline (AEMaaCS)

```
⚠️ CRITICAL for existing AEMaaCS implementations — phased enforcement:

├── Jan 26, 2026 — Actions Center notification emails to affected customers
├── Feb 26, 2026 — Cloud Manager pipelines PAUSE at Code Quality step
│                   if deprecated API usage detected (manual override available)
├── Apr 14, 2026 — Pipelines FAIL at Code Quality; deployments BLOCKED
│                   until deprecated API usage is removed
└── Jul 23, 2026 — FINAL: environments still using deprecated APIs stop
                    receiving critical Adobe release updates and lose
                    standard performance/availability commitments

Deprecated packages include:
├── org.apache.sling.commons.auth
├── org.eclipse.jetty.*
├── com.mongodb.*
├── Apache Commons Lang 2 / Collections 3
└── ch.qos.logback.*

Detection & remediation:
├── Use the Cloud Readiness Analyzer / Repository Modernizer
├── Review AEM SDK changelogs for affected classes
└── Refactor custom bundles before the enforcement dates
```

> **Note:** This supersedes the earlier single "March 30, 2026" framing — Adobe moved to a phased enforcement model with the hard cutoff on **July 23, 2026**.

---

## Additional 2026 Release Highlights (Confirmed)

### AEM as a Cloud Service

| Release | Feature | Architect Impact |
|---|---|---|
| **2026.1.0** (Jan 29) | **Content MCP Server** | AI tools (ChatGPT/Claude/Cursor/Copilot) work with AEM Pages, CFs, Assets (read-only + read/write) |
| **2026.1.0** | **AI Search** for Assets | Semantic, intent-aware search (handles synonyms/misspellings) |
| **2026.1.0** | **AEM Edge Functions** (Beta) | CDN-layer JS for geo/device/attribute personalization |
| **2026.3.0** (Mar 26) | RTE **TinyMCE → TipTap** | Unified rich-text across Universal Editor + CF editor |
| **2026.6.0** (Jun 25) | **Visual Content Fragments** | Render CFs as formatted HTML via templates; deliver to web/email/EDS |
| **2026.6.0** | IDE AI Agent (Beta) | Detect/auto-fix code incl. Claude Code integration for Java teams |
| Ongoing | **Generate Variations** (GA) | GenAI content variations in Sidekick/Universal Editor/CF editor |
| Ongoing | **Agents in AEM** | Brand Experience Agent umbrella: Modernization, Production, Development, Content Advisor, Governance sub-agents |

### Cloud Manager 2026

```
├── Secrets in configuration pipelines (2026.1.0) — env-specific secret overrides
├── Smart Build / module-level caching (Beta) — rebuild only changed modules
├── Customer-Managed Keys (CMK) self-service (2026.6.0)
├── Web Tier Pipelines for AMS (Beta) — deploy Dispatcher/web-tier independently
└── Environment variable limit raised to 400
```

### Universal Editor vs Page Editor (Strategic)

```
├── Universal Editor will EVENTUALLY SUPERSEDE the classic Page Editor
├── ⚠️ NO direct migration path (fundamental technology differences)
├── UE does NOT reintroduce Template Editor, Style System, or Responsive Grid
├── New projects → default to Universal Editor
├── Existing projects → stay on Page Editor (still supported; no new innovation)
└── UE is the PREFERRED authoring tool for Edge Delivery Services ("Crosswalk")
```

> **Java Roadmap:** AEMaaCS runtime is **Java 21** (Java 11 deprecated). **Java 25** support is slated to roll out **October 2026 – June 2027**.

---

*[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E117 Index](../README.md)*
