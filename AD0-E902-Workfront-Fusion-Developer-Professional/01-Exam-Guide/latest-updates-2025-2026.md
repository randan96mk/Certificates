# AD0-E902 — Latest Exam & Product Updates (2025-2026)

[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E902 Index](../README.md)

---

## Exam Administration Changes

| Change | Details |
|---|---|
| **Proctoring Platform** | **Meazure Learning / ProctorU** with Guardian secure browser (AI + human proctors) |
| **Scheduling** | Up to 60 days in advance via [certification.adobe.com](https://certification.adobe.com/); $10 fee within 24 hrs; free reschedule >24 hrs before |
| **Summit 2026** | Free exam voucher with full conference pass (April 19-22, Las Vegas) |
| **Renewal Status** | **RESUMED** (relaunched ~March 2025) — module-based renewal (two ~15-min modules), free for most certs, 2-year validity. See [renewal note](#certification-renewal-resumed) below. |
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

### New & Updated Connectors (Research-Confirmed)

```
Connector Updates (2025-2026):
├── Workfront connector upgrades
│   ├── New Workfront API released May 5, 2025 — Fusion modules updated to match
│   └── Further connector update Oct 22, 2025
├── Unified Review & Approvals connector (2025)
│   ├── Modules: create/update approval stages, add participants,
│   │   send reminders, make decisions, get suggested approvers
│   └── ⚠️ Scenarios using the OLD approval approach need remediation
├── Frame.io V4 modules (account-level metadata read/create/update/delete)
├── Veeva Vault modules expanded (Oct 2025) incl. single object-record update
├── Adobe Workfront Planning connector modules
├── Microsoft SharePoint — new Microsoft Entra certificate-based auth
├── Webhook security — ability to add credentials to webhooks
└── Rolling connector release cadence (frequent updates)
```

> **Deprecation (Fusion-specific):** Legacy Fusion **Photoshop** modules are deprecated **after July 30, 2026** — migrate active scenarios. Figma (Legacy) connection deprecated (Jan 2025). JWT credentials for the Adobe Authenticator connector stopped working after Jan 1, 2025 (use OAuth). Legacy Workfront modules were removed from the module selector in **May 2025** (replaced by custom-form-aware modules).

### Adobe Firefly Connector in Fusion (Confirmed)

```
Firefly modules available in Fusion:
├── Generate an image
├── Generate images with Image5 (newer model)
├── Generate video
├── Expand image / Fill image
├── Generate adaptive / object / precise composite
├── Generate similar images
└── Make a custom API call

Also available: OpenAI (ChatGPT & DALL-E) connector.
```

> **Reality check:** A dedicated "AI scenario builder" *inside* Fusion is **NOT confirmed** in Adobe release notes as of mid-2026. The confirmed AI story is (a) the **Firefly connector** for generative image/video tasks within scenarios and (b) the broader Workfront **Workflow Optimization Agent** (a Workfront platform agent, announced Summit 2026, still rolling out). The core exam still tests **manual** scenario construction — modules, functions, flow control, and error handling.

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

## Certification Renewal (Resumed)

```
Adobe Certification Renewal — status as of mid-2026:
├── RELAUNCHED (~March 2025) with a simplified, module-based process
│   ├── Renew by passing TWO short renewal modules (~15 min each)
│   ├── FREE for most Digital Experience / Document Cloud certs
│   ├── 2-year validity; renewal window opens 180 days before expiry
│   └── Renewing one cert in an application renews the others in it
├── During the earlier pause, certs expiring Oct 14, 2024 – Sep 30, 2025
│   were auto-extended to Oct 1, 2025
├── Exceptions: Captivate & ColdFusion require a full retake ($99)
└── ⚠️ CAVEAT: Some Experience League "renew" doc pages (updated
    May 21, 2026) STILL show a stale "temporarily on hold" banner
    that contradicts the live portal. Trust the portal
    (certification.adobe.com) as authoritative.
```

> **Bottom line:** Renewal is operational again. If your credential is nearing expiry, check the **"Renewals" tab** in My Account on the new portal rather than the legacy doc banner.

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
