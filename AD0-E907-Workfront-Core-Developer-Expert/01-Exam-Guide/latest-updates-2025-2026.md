# AD0-E907 — Latest Exam & Product Updates (2025-2026)

[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E907 Index](../README.md)

---

## Exam Administration Changes

| Change | Details |
|---|---|
| **Proctoring Platform** | Now **Meazure Learning / ProctorU** with Guardian browser (replaced Examity) |
| **Scheduling** | Up to 60 days in advance via [certification.adobe.com](https://certification.adobe.com/) |
| **Summit 2026** | Free exam with full conference pass (April 19-22, Las Vegas) |
| **Renewal Status** | **ON HOLD** — Adobe is overhauling the renewal process. Monitor the certification portal. |
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

## Deprecated / Retired Features

| Feature | Status | Replacement |
|---|---|---|
| G Suite integration (legacy) | **Retired Feb 28, 2026** | Workfront Fusion |
| Jira integration (legacy) | **Retired Feb 28, 2026** | Workfront Fusion |
| Salesforce integration (legacy) | **Retired Feb 28, 2026** | Workfront Fusion |
| Outlook legacy token support | **Ended Oct 1, 2025** | Updated OAuth flow |
| Text with Formatting field | **Replaced Q2 2026** | Rich Text Fields |

> **Architect Tip:** If you see exam questions about Jira/Salesforce integrations, the correct answer for new implementations is now "Use Fusion" rather than legacy connectors.

---

## Industry Recognition

Adobe Workfront was recognized as a **Leader** in the Forrester Wave for Collaborative Work Management (June 2025), receiving the highest scores among all vendors. This validates the platform choice for enterprise implementations.

---

*[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E907 Index](../README.md)*
