# Domain 2: Intake, Planning, and Processes (~22%)

[Back to AD0-E907 Index](../README.md)

---

## Overview

This domain covers how work enters Workfront and how processes are designed. It is the **second-highest weighted domain** and is critical for architect-level understanding.

## Topics Covered

1. [Request Queues & Intake Design](./request-queues.md)
2. [Approval Processes](./approval-processes.md)
3. [Blueprints & Templates](./blueprints-templates.md)

---

## Architect's Intake Design Framework

```
                    ┌──────────────────┐
                    │   Intake Channel │
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │  Request Queue    │
                    │  (Queue Topics)   │
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │  Routing Rules    │
                    │  (Auto-assign)    │
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │  Triage/Review    │
                    │  (Approval?)      │
                    └────────┬─────────┘
                             │
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
        ┌──────────┐  ┌──────────┐  ┌──────────┐
        │ Quick Fix │  │ Convert  │  │ Convert  │
        │ (Issue)   │  │ to Task  │  │ to Proj  │
        └──────────┘  └──────────┘  └──────────┘
```

## Key Architectural Decisions

| Decision | Options | Recommendation |
|---|---|---|
| Queue per team vs shared queue | Separate / Shared | Separate for accountability, shared for small orgs |
| Approval before/after routing | Pre-route / Post-route | Post-route for speed; pre-route for governance |
| Issue vs Task conversion | Stay as Issue / Convert | Convert for significant work (>4 hours) |
| Template standardization | Strict / Flexible | Strict for regulated; flexible for creative teams |

---

*[Back to AD0-E907 Index](../README.md)*
