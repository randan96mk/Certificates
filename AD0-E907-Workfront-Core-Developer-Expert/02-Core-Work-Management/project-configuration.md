# Project Configuration & Management

[Back to Domain 1](./README.md) | [Back to AD0-E907 Index](../README.md)

---

## Project Creation Methods

| Method | Use Case | Notes |
|---|---|---|
| **From Template** | Repeatable processes | Best practice for standardization |
| **Blank Project** | One-off unique projects | Requires manual setup |
| **Copy Project** | Similar to existing | Can select what to copy |
| **MS Project Import** | Migration from MS Project | .mpp file import |
| **From Request** | Issue → Project conversion | Carries issue data forward |
| **From Blueprint** | Adobe-provided templates | Pre-built configurations |

## Project Settings Deep Dive

### Schedule Settings

```
Project Schedule Mode:
├── Schedule From Start Date
│   └── Tasks flow forward from project start
│   └── Default: ASAP constraint on tasks
└── Schedule From Completion Date
    └── Tasks flow backward from deadline
    └── Default: ALAP constraint on tasks
```

### Update Types

| Update Type | When Recalculated | Best For |
|---|---|---|
| **Automatic and On Change** | Real-time + manual changes | Most responsive; default |
| **Automatic** | Nightly recalculation only | Large projects; performance |
| **Change Only** | When user edits | Manual control |
| **Manual** | Only when user triggers | Full manual control |

> **Architect Tip:** For enterprise portfolios with 500+ projects, consider "Automatic" over "Automatic and On Change" to reduce system load.

### Condition Types

**Manual Condition:**
- On Target (Green)
- At Risk (Yellow)
- In Trouble (Red)
- Set manually by project owner

**Progress Status (Automatic):**
- On Time — projected completion ≤ planned completion
- Behind — projected completion > planned completion but ≤ today
- At Risk — projected completion is in the future but past planned
- Late — planned completion is past and not yet complete

### Financial Settings

| Setting | Description |
|---|---|
| **Performance Index Method** | Hour-Based or Cost-Based (for EVM) |
| **Budget** | Project budget amount |
| **Fixed Cost** | Non-labor costs |
| **Fixed Revenue** | Guaranteed revenue amount |
| **Revenue Type** | Not Billable, Role Hourly, User Hourly, Fixed Hourly, Role/User with Cap, Fixed Revenue |
| **Cost Type** | No Cost, Fixed Hourly, Role Hourly, User Hourly |

### Earned Value Management (EVM)

```
Key EVM Formulas:
─────────────────
CPI (Cost Performance Index) = BCWP / ACWP
  → CPI > 1 = Under budget
  → CPI < 1 = Over budget

SPI (Schedule Performance Index) = BCWP / BCWS
  → SPI > 1 = Ahead of schedule
  → SPI < 1 = Behind schedule

EAC (Estimate at Completion) = BAC / CPI
  → Projected total cost

Where:
  BCWP = Budgeted Cost of Work Performed (Earned Value)
  ACWP = Actual Cost of Work Performed
  BCWS = Budgeted Cost of Work Scheduled (Planned Value)
  BAC  = Budget at Completion
```

## Project Statuses

### System Statuses vs Custom Statuses

```
System Statuses (cannot delete):
├── Planning (PLN) — project being planned
├── Current (CUR) — actively in progress
├── Complete (CPL) — finished
├── Dead (DED) — cancelled
├── On Hold (ONH) — paused
└── Requested (REQ) — request awaiting approval

Custom Statuses:
├── Map to a system status (equates with)
├── Can be system-wide or group-level
├── Can set as default for specific groups
└── Support custom colors and labels
```

### Status Behavior Rules
- Moving from Complete/Dead back to Current resets Actual Completion Date
- Completed projects don't show in most active views
- Status changes can trigger notifications
- Status changes can be restricted by approval processes

## Project Templates — Architecture Patterns

### Template Design Best Practices

1. **Naming Convention:** `[Department]-[ProcessType]-[Version]`
   - Example: `MKT-CampaignLaunch-v3`

2. **Template Hierarchy:**
   ```
   Portfolio: Marketing Operations
   └── Template: MKT-CampaignLaunch-v3
       ├── Template Task: Strategy & Planning (Milestone)
       │   ├── Define objectives
       │   ├── Identify audience
       │   └── Approval gate
       ├── Template Task: Content Development
       │   ├── Creative brief
       │   ├── Asset creation
       │   └── Review & approve
       └── Template Task: Launch & Measure
           ├── Deploy assets
           ├── Monitor metrics
           └── Post-mortem report
   ```

3. **Template Components to Configure:**
   - Task structure with predecessors
   - Duration and planned hours estimates
   - Role-based assignments (not user-based)
   - Custom forms attached
   - Approval processes
   - Milestone path
   - Financial settings
   - Queue setup (if needed)
   - Document templates
   - Sharing settings

> **Architect Tip:** Design templates with role-based assignments rather than user-based. This ensures templates scale across teams and don't break when users leave the organization.

## Project Copy vs Template

| Feature | Create from Template | Copy Project |
|---|---|---|
| Task Structure | Yes | Yes |
| Task Assignments | Role-based | Can copy user assignments |
| Custom Data | Template defaults | Copies actual values |
| Documents | Template documents | Can copy project documents |
| Issues | No | Can copy issues |
| Actual Data | No | Can copy actuals |
| Financial Data | Template defaults | Can copy financials |
| Approvals | Template approvals | Can copy approvals |
| Updates/Notes | No | Can copy updates |

## Baselines

- **Purpose:** Capture point-in-time snapshot of project plan
- **Auto-Baseline:** System creates when project moves to Current
- **Manual Baseline:** User can create at any time
- **Max Baselines:** 10 per project
- **Captured Data:** Planned dates, hours, costs, duration
- **Use:** Compare current plan against baseline for variance analysis

---

### Practice Scenarios

**Scenario 1:** A PMO director needs all new marketing campaigns to follow the same process with pre-defined tasks, approvals at each phase gate, and automatic assignment to the appropriate roles. What's the recommended approach?

<details>
<summary>Answer</summary>

Create a **project template** with:
- Structured task groups representing each phase
- Milestone path with milestones at each gate
- Approval processes attached to gate tasks
- Role-based assignments (not users)
- Custom forms for campaign metadata
- Request queue for intake (optional)

The template ensures consistency while role-based assignments provide flexibility.
</details>

**Scenario 2:** A project manager reports that task dates aren't recalculating when predecessors change. What should you check?

<details>
<summary>Answer</summary>

1. **Update Type** — Ensure it's set to "Automatic and On Change" (not Manual)
2. **Task Constraints** — Check if tasks have hard constraints (MSO, MFO, Fixed Dates) that override predecessor logic
3. **Cross-project predecessors** — Verify they're properly configured
4. **Timeline recalculation** — Admin can manually trigger via Setup > Project Preferences
</details>

---

*[Back to Domain 1](./README.md) | [Next: Task Management →](./task-management.md)*
