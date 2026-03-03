# Approval Processes

[Back to Domain 2](./README.md) | [Back to AD0-E907 Index](../README.md)

---

## Approval Process Types

| Type | Scope | Created By | Available To |
|---|---|---|---|
| **System-wide** | Entire Workfront instance | System admin | All groups |
| **Group-level** | Specific group only | Group admin | Group members only |
| **Single-use** | One specific object | Any user with Manage | That object only |

## Approval Process Architecture

### Single-Path Approval

```
Request Submitted
    │
    ▼
Stage 1: Manager Approval
    │ ├── Approved → moves to next stage
    │ └── Rejected → back to requestor
    ▼
Stage 2: Director Approval
    │ ├── Approved → status changes to "Approved"
    │ └── Rejected → back to requestor
    ▼
Completed / Approved
```

### Multi-Path Approval (Advanced)

```
Request Submitted
    │
    ▼
    ┌── Path A (if Amount ≤ $5,000) ──┐
    │   Stage 1: Manager              │
    │   └── Approved → Done           │
    │                                  │
    ├── Path B (if $5,001-$25,000) ───┤
    │   Stage 1: Manager              │
    │   Stage 2: Director             │
    │   └── Approved → Done           │
    │                                  │
    └── Path C (if Amount > $25,000) ─┘
        Stage 1: Manager
        Stage 2: Director
        Stage 3: VP
        └── Approved → Done
```

> **Note:** Multi-path approvals are configured by mapping statuses to different approval stages. The system evaluates which path to follow based on the object's status.

## Approval Process Components

### Stages

| Component | Description |
|---|---|
| **Stage Name** | Descriptive label |
| **Approver(s)** | User, Role, or Team |
| **Approval Type** | Any one / All must approve |
| **Stage Duration** | Auto-escalation after X days (optional) |
| **Fallback Approver** | Who approves if primary is absent |
| **Rejection Behavior** | Return to previous status / specific status |

### Approver Types

```
Approver Options:
├── Specific User(s)
│   └── Named individual
├── Job Role
│   └── Any user with that role can approve
├── Team
│   └── Any team member can approve
├── Project Manager
│   └── Dynamic: current PM
├── Project Sponsor
│   └── Dynamic: current sponsor
├── Requestor's Manager
│   └── Dynamic: submitter's direct manager
└── Project Owner
    └── Dynamic: project owner
```

### Approval Actions

| Action | Behavior |
|---|---|
| **Approve** | Moves to next stage or completes approval |
| **Reject** | Returns to specified status; may restart process |
| **Recall** | Submitter withdraws the approval |
| **Change Status** | Admin can remove approval by changing status |

## Approval + Status Integration

```
Status Flow with Approval:

Normal:     New → In Progress → Complete
                                    │
With Approval:                      │
            New → In Progress → Pending Approval → Approved → Complete
                                    │
                                    └── Rejected → Back to In Progress
```

### Status Mapping
- Approval process triggers when object enters a specific status
- Each approval stage can map to different statuses
- Rejection can map to a specific "Rejected" status or revert

## Approval Delegation

```
User going on leave:
├── Setup > My Settings > Delegate My Approvals
├── Select delegate user
├── Set date range
├── Delegate receives all pending approvals
└── Original approver retains access when delegation ends

Admin delegation:
├── Setup > Users > select user
├── "Delegate Approvals" option
└── Override user's delegation settings
```

## Best Practices for Architects

### Approval Design Guidelines

1. **Minimize stages** — Each stage adds latency
2. **Use role-based approvers** — More resilient than user-based
3. **Set timeouts** — Auto-escalate after X days
4. **Define rejection clearly** — Which status does it revert to?
5. **Test approval flows** — Ensure all paths work before go-live
6. **Document approval chains** — Maintain a RACI-style matrix

### Common Anti-Patterns

| Anti-Pattern | Problem | Better Approach |
|---|---|---|
| Too many stages (5+) | Delays work | Consolidate to 2-3 stages |
| User-specific approvers | Breaks when user leaves | Use job roles |
| No timeout | Approvals stuck indefinitely | Set auto-escalation |
| Approval on every status change | Over-governance | Approve at key gates only |
| Single-use approvals everywhere | Hard to maintain | Use system/group approvals |

---

### Practice Scenarios

**Scenario 1:** Design an approval process for a creative brief where:
- Drafts need review by the creative director
- If the budget exceeds $10,000, the VP of Marketing must also approve
- Rejected briefs should go back to "Needs Revision" status

<details>
<summary>Answer</summary>

Create a **system-wide approval process** attached to the project custom form with budget field:

**Stage 1:** Creative Director (Role-based approver)
- Approval type: One approver must approve
- Rejection status: "Needs Revision"

**Stage 2 (conditional):** VP of Marketing
- Only applies when budget field > $10,000
- This is achieved using a **multi-path approach** based on status:
  - Budget ≤ $10K: Approval completes after Stage 1
  - Budget > $10K: Route to Stage 2

**Implementation:**
- Custom form has "Budget" calculated/number field
- Approval process has 2 stages
- Use a calculated field or Fusion scenario to evaluate budget and set appropriate status to trigger the correct approval path
</details>

---

*[← Request Queues](./request-queues.md) | [Next: Blueprints & Templates →](./blueprints-templates.md)*
