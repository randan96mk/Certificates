# AD0-E907 — Latest Exam & Product Updates (2025-2026)

[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E907 Index](../README.md)

---

## Exam Administration Changes

| Change | Details |
|---|---|
| **Proctoring Platform** | Now **Meazure Learning / ProctorU** with Guardian browser (replaced Examity) |
| **Scheduling** | Up to 60 days in advance via [certification.adobe.com](https://certification.adobe.com/) |
| **Summit 2026** | Free exam with full conference pass (April 19-22, Las Vegas) |
| **Renewal Status** | **RESUMED** (~March 2025) — simplified module-based renewal (two ~15-min modules), free, 2-year validity. Note: some Experience League doc pages still show a stale "on hold" banner — trust the live portal. |
| **Exam Status (mid-2026)** | **UNCHANGED & ACTIVE** — AD0-E907 remains the current exam ID (verified via [certification.adobe.com/courses/211](https://certification.adobe.com/courses/211)) |
| **Related Exam Update** | The AD0-E903 → AD0-E911 renumbering affected the **Project Manager** track ONLY (not AD0-E907) |
| **Cert Overview Last Updated** | Adobe Workfront certification [overview page](https://experienceleague.adobe.com/en/docs/certification/program/technical-certifications/aw/aw-overview) last updated **May 4, 2026** |

> **Important:** Certification renewals have been on hold since late 2024. If your certification is approaching expiration, contact [Adobe support](https://certification.adobe.com/support/contactus) directly.

> **Confirmed (mid-2026):** Despite the Project Manager exam being renumbered (AD0-E903 → AD0-E911 in Jan 2026), there is **no official evidence** of an AD0-E907 retirement, renumber, or version refresh. It is still the active ID for Workfront Core Developer Expert. Note that a separate lower-tier **Workfront Core Developer – Professional** credential also exists ([course 1046](https://certification.adobe.com/courses/1046)) — don't confuse the two.

---

## New Workfront Features You Should Know (2025-2026)

### Workfront Planning (Major New Product Area)

```
Workfront Planning:
├── Unifying metadata layer for content supply chain
├── Hierarchies between record/object types
│   └── Up to 4 types in one hierarchy, 5 hierarchies per workspace
├── Planning Requests integrated into My Requests widget
├── AI-powered Planning Designer (Beta) for workspace creation
├── Approval rules for dynamic routing based on field values
├── Free 60-day Planning trial (launched March 2, 2026)
└── Integration with Adobe Agent Studio

Key Concepts:
├── Record Types: Define custom objects
├── Workspaces: Contain record types and views
├── Connections: Link record types to each other and to WF objects
├── Views: Table, Timeline, Calendar for planning data
└── Planning Requests: Submit requests against record types
```

> **Exam Impact:** Planning is a major new product area. While it may not be heavily tested on the current AD0-E907, understand it conceptually as it represents Workfront's strategic direction.

### AI Assistant

```
AI Assistant Capabilities:
├── Locate items across Workfront
├── Generate calculated field formulas
├── Refine existing formulas
├── Summarize updates and documents within timeframes
├── Write business rules
├── Access Planning data
└── Respects user roles and permissions

Access: Chat interface within Workfront
Limitation: Cannot create/edit objects directly (advisory role)
```

### Unified Review & Approval (Frame.io Integration)

```
Unified Review & Approval:
├── Powered by Workfront + Frame.io
├── Multi-stage approval workflows
├── Native video review capabilities
├── Combines Workfront project management with Frame.io review
└── Replaces some traditional proofing workflows for video

When to Use:
├── Video content → Unified Review (Frame.io)
├── Documents/images → Traditional Workfront Proofing
└── Both can coexist in the same project
```

### Fusion Enhancements

```
New Fusion Capabilities (2025-2026):
├── Chain Scenarios
│   ├── Parent-child scenario references
│   ├── Reuse automation logic across scenarios
│   └── Modular scenario design
├── New Connectors
│   ├── Updated Workfront connector (Oct 2025)
│   ├── Microsoft SharePoint Online module (Feb 2026)
│   └── Adobe Firefly integration (image generation/resize)
├── Performance Improvements
│   ├── Better scenario execution monitoring
│   ├── Improved UX for scenario management
│   └── Rolling release cadence
└── Legacy Deprecation Alert
    └── G Suite, Jira, Salesforce legacy integrations
        retired Feb 28, 2026 — use Fusion instead
```

### Canvas Dashboards (Open Beta)

```
Canvas Dashboards:
├── Flexible visualization builder
├── Drag-and-drop layout
├── More visual options than traditional dashboards
├── Currently in Open Beta
└── Will eventually replace classic dashboard experience
```

### API Changes

```
API v21 (Released Oct 23, 2025):
├── Breaking changes to Event Subscriptions
├── New endpoints for Planning objects
└── Updated authentication flows

Key for Fusion/Integration developers:
├── Review migration guide for Event Subscription changes
├── Test existing integrations against v21
└── Update custom API integrations
```

### Other Notable Changes

| Feature | Details |
|---|---|
| **Multi-select External Lookup** | New custom form field type — API-driven multi-select |
| **Rich Text Fields** | Replacing "Text with Formatting" (Q2 2026) |
| **AI Reviewer (Beta)** | Automated brand compliance checking for images |
| **AI Project Health Advisor (Beta)** | AI-powered performance assessments for projects |
| **Content Review AI Collaborator (Q2 2026)** | AI agents in projects with configurable brand guidelines |

---

## Workfront 2026 Release Highlights (Q1–Q3, Adobe-Confirmed)

These are drawn from Adobe's official Q2 2026 (26.2–26.4) and Q3 2026 (26.5–26.7) release overviews.

### Workfront Planning — GA & 2026 Enhancements

```
Workfront Planning:
├── GA since August 28, 2024 (shipped, licensed product)
├── Planning AI Assistant (GA) — search/create/update/delete records
│   in page context; can create records from uploaded docs (PPTX/PDF/DOCX)
├── Planning Designer (Beta) — AI builds workspaces, record types,
│   fields, formulas, views (even from an uploaded org chart/doc)
├── Q1 2026: flexible hierarchies (up to 4 levels, 5 per workspace),
│   global record types, up to 30 connection fields per record type
├── Q2 2026: trigger-based automations, approval rules for requests,
│   global search (Ctrl/⌘+K), real-time presence indicators
├── Q3 2026: record-level permissions, default field values,
│   Table View redesign, AEM Content Fragment lookups + preview
└── Planning API v2 (GA May 28, 2026) — programmatic workspace/record CRUD
```

### AI Agents & MCP (Confirmed on Release Pages)

| Feature | Status | Date |
|---|---|---|
| **Content Review AI Collaborator** | GA | Apr 15-16, 2026 (preview Apr 2) |
| **Content Advisor** (AEM Assets discovery in WF) | GA | Apr 16, 2026 |
| **GenStudio Foundation** (auto-provisioned to all WF customers) | Provisioned | Mar 31, 2026 |
| **Workfront MCP Server** (Claude/ChatGPT natural-language access) | GA | July 16, 2026 (Claude + EU support Jun 11) |
| **Workflow Optimization Agent** | Announced (rolling out) | Summit, Apr 20, 2026 |

> **Nuance:** Adobe's own June 2026 "Workfront Wire" calls the Workflow Optimization Agent and the broader third-party **AI Collaborators** framework "**upcoming** capabilities" — third-party "GA June 2026" claims are unverified. The **Content Review AI Collaborator** (a narrower feature) IS confirmed GA in Q2 2026.

### Other Confirmed 2026 Features

```
Q2 2026 (26.2–26.4):
├── Enterprise Operations capabilities suite (GA Apr 15-16)
│   └── Advanced financials (multi-level cost/billing hierarchies),
│       historical data tracing, enterprise permissions,
│       business-rules automation, custom localization
├── Data Connect auth via RSA keys + Programmatic Access Tokens
└── Shareable report folders, scheduled report link delivery

Q3 2026 (26.5–26.7, through July 15-16):
├── Change History List — central admin view of object changes
├── Native Financial Fields + Rich-Text field type in custom forms
├── System-wide custom form sharing
├── Required fields enforced in Bulk Edit
├── Adobe Express integration for structured review/approval (Jun 15)
├── AEM status badges on documents; document summary printing
├── Cloud Storage usage tracking; legacy→Adobe cloud storage conversion
└── Canvas Dashboard: prompt defaults, persistent selections, currency fields
```

### API & Records Model Changes

| Change | Date |
|---|---|
| **API v22** released (supersedes v21) | May 8, 2026 |
| **Planning API v2** released | May 28, 2026 |
| New Record ID field type (Planning) | 2026 |
| `actualWorkRequired` → `actualWorkRequiredDouble` migration | June 1, 2026 |
| Legacy connector API v20 deprecation | Scheduled release 28.4 (Apr 2028) |

---

## Deprecated / Retired Features

| Feature | Status | Replacement |
|---|---|---|
| G Suite integration (legacy) | **Retired Feb 28, 2026** | Workfront Fusion |
| Jira integration (legacy) | **Retired Feb 28, 2026** | Workfront Fusion |
| Salesforce integration (legacy) | **Retired Feb 28, 2026** | Workfront Fusion |
| Outlook legacy token support | **Ended Oct 1, 2025** | Updated OAuth flow |
| Text with Formatting field | **Replaced Q2 2026** | Rich Text Fields |
| Legacy Fusion Photoshop modules | **Deprecated after July 30, 2026** | Migrate scenarios to current modules |
| Enhanced Analytics | **Deprecating (ongoing)** | Workfront Data Connect |

> **Architect Tip:** If you see exam questions about Jira/Salesforce integrations, the correct answer for new implementations is now "Use Fusion" rather than legacy connectors.

---

## Industry Recognition

Adobe Workfront was recognized as a **Leader** in the Forrester Wave for Collaborative Work Management (June 2025), receiving the highest scores among all vendors. This validates the platform choice for enterprise implementations.

---

*[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E907 Index](../README.md)*
