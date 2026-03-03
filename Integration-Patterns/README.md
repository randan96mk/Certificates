# Integration Patterns

[Back to Main README](../README.md)

---

## Overview

This section covers cross-product integration patterns relevant to both certifications and your architect role.

## Sections

| Section | Description |
|---|---|
| [AEM + Workfront](./AEM-Workfront/README.md) | Native integration: linked folders, metadata sync, workflows |
| [Workfront + Fusion](./Workfront-Fusion/README.md) | Automation patterns: project provisioning, sync, escalation |
| [End-to-End Scenarios](./End-to-End-Scenarios/README.md) | Full lifecycle: campaign, asset production, multi-system |

## Integration Architecture Overview

```
┌──────────────────────────────────────────────────────────────┐
│                    Adobe Ecosystem                            │
│                                                              │
│  ┌──────────┐    Native     ┌──────────┐    Native          │
│  │ Workfront │◀────────────▶│   AEM    │◀──────────▶ Target │
│  │           │  Connector   │          │           Launch    │
│  └─────┬────┘              └──────────┘           Analytics  │
│        │                                                     │
│        │ API                                                 │
│        ▼                                                     │
│  ┌──────────┐                                                │
│  │  Fusion  │──▶ Any REST API (Slack, Jira, Salesforce...)   │
│  └──────────┘                                                │
└──────────────────────────────────────────────────────────────┘
```

---

*[Back to Main README](../README.md)*
