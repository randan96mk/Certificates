# AD0-E902 — Latest Exam & Product Updates (2025-2026)

[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E902 Index](../README.md)

---

## Exam Administration Changes

| Change | Details |
|---|---|
| **Proctoring Platform** | **Meazure Learning / ProctorU** with Guardian secure browser (AI + human proctors) |
| **Scheduling** | Up to 60 days in advance via [certification.adobe.com](https://certification.adobe.com/); $10 fee within 24 hrs; free reschedule >24 hrs before |
| **Summit 2026** | Free exam voucher with full conference pass (April 19-22, Las Vegas) |
| **Renewal Status** | **ON HOLD** — Adobe overhauling the renewal process; certifications expiring in the hold window auto-extended. Monitor the portal. |
| **Last Official Update** | Exam last updated **October 21, 2024**; **not retired or replaced** as of mid-2026 |
| **Language** | English only |

> **Important:** AD0-E902 remains a current, active exam. Unlike the Project Manager exam (AD0-E903 → AD0-E911, Jan 2026), there is no announced replacement for AD0-E902 as of mid-2026.

---

## Confirmed Exam Facts (Research-Verified)

| Parameter | Value |
|---|---|
| **Questions** | 51 |
| **Time** | 102 minutes |
| **Passing Score** | 33/51 (~64.7%) |
| **Cost** | $125 USD global / $95 USD India |
| **Domain Weights** | Foundational Concepts 39% · Scenario Design 35% · Testing & Error Handling 16% · APIs 10% |

> **Study Impact:** Domains 1 + 2 together = **74%** of the exam. Prioritize functions, data transformation, flow control, and scenario architecture.

---

## New Workfront Fusion Features You Should Know (2025-2026)

### Chain Scenarios (Modular Automation)

```
Chain Scenarios:
├── Parent-child scenario references
│   └── Parent calls child via webhook, child returns via Webhook Response
├── Reuse automation logic across multiple parent scenarios
├── Modular design — update child once, all parents benefit
├── Independent error handling per scenario
└── Better organization for complex enterprise workflows

Exam Relevance:
├── Understand WHY to chain (reuse, maintainability)
├── Know the mechanism (HTTP call to child webhook)
└── Recognize when a monolithic scenario should be decomposed
```

### New & Updated Connectors

```
Connector Updates (2025-2026):
├── Updated Workfront connector (Oct 2025)
│   └── Aligned with Workfront API v21
│   └── New modules for Planning objects
├── Microsoft SharePoint Online module (Feb 2026)
├── Adobe Firefly integration
│   └── Image generation, resizing, and transformation in scenarios
├── Adobe Agent Studio touchpoints (AI orchestration)
└── Rolling connector release cadence (frequent updates)
```

### AI-Assisted Scenario Building (Emerging)

```
AI in Fusion (2025-2026 direction):
├── AI assistance for scenario design (guided building)
├── Firefly modules for generative image tasks within flows
├── Integration with Adobe's broader Agent Studio strategy
└── Note: Core exam still tests manual scenario construction —
    AI features are supplementary, not a replacement for
    understanding modules, functions, and flow control
```

### Platform & UX Improvements

```
2025-2026 Improvements:
├── Better scenario execution monitoring
├── Improved scenario management UX
├── Enhanced execution history and debugging views
└── Rolling release cadence (Fusion is built on the Make platform)
```

---

## Make.com Platform Relationship

```
Workfront Fusion = Adobe's licensed/embedded version of Make (Celonis/Make.com)

What this means for the exam:
├── Core concepts are shared with Make: modules, bundles, operations,
│   routers, iterators, aggregators, data stores, functions
├── Fusion-specific: deep Workfront connector, Adobe SSO/IMS auth,
│   Adobe support and SLAs
├── Webhook URLs historically use hook.*.make.com domains
└── Function syntax and error-handling directives mirror Make
```

> **Exam Tip:** If you have Make.com experience, most of it transfers directly. The Fusion exam adds Workfront-specific connector knowledge and Adobe ecosystem authentication.

---

## Deprecated / Retired Features (Ecosystem-Wide)

| Feature | Status | Replacement |
|---|---|---|
| Workfront legacy G Suite integration | **Retired Feb 28, 2026** | Workfront Fusion |
| Workfront legacy Jira integration | **Retired Feb 28, 2026** | Workfront Fusion |
| Workfront legacy Salesforce integration | **Retired Feb 28, 2026** | Workfront Fusion |
| Outlook legacy token support | **Ended Oct 1, 2025** | Updated OAuth flow |

> **Architect Tip:** For any exam question asking how to integrate Workfront with Jira/Salesforce/Google in a *new* implementation, the correct answer is now **Fusion** — the legacy native connectors are retired.

---

## Relationship to Other Certifications

```
Fusion Certification Landscape:
├── AD0-E902 — Workfront Fusion Developer Professional (THIS EXAM)
│   └── 100% Fusion-focused, 51 questions, Professional level
├── AD0-E907 — Workfront Core Developer Expert
│   └── Broad Workfront (7 domains); Fusion is only a light touch
│   └── NO dedicated Fusion section
└── Complementary paths:
    ├── AD0-E902 → prove deep Fusion/integration skill
    └── AD0-E907 → prove broad Workfront platform expertise
```

---

## Study Resources (Current)

| Resource | Link / Detail |
|---|---|
| Prep Guide (Course 1200) | [certification.adobe.com/courses/1200](https://certification.adobe.com/courses/1200) |
| Fusion Tutorials | Experience League Fusion training overview |
| Free Practice Sandbox | Request via `wfttstdr@adobe.com` (valid 3 months) |
| Community | [Experience League — Workfront Fusion](https://experienceleaguecommunities.adobe.com/t5/workfront-fusion/ct-p/workfront-fusion) |

> **Community Note:** Multiple test-takers report that third-party practice exams are noticeably *easier* than the real exam. Use the official prep guide "Study for exam" tabs and hands-on sandbox practice as your primary preparation.

---

*[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E902 Index](../README.md)*
