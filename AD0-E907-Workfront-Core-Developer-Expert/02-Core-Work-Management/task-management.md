# Task Management & Dependencies

[Back to Domain 1](./README.md) | [Back to AD0-E907 Index](../README.md)

---

## Task Fundamentals

### Task Types in Workfront

| Type | Description | Key Difference |
|---|---|---|
| **Regular Task** | Standard work item | Has duration, hours, assignments |
| **Summary/Parent Task** | Contains subtasks | Values roll up from children |
| **Milestone Task** | Marks a key achievement | Zero or minimal duration |
| **Recurring Task** | Repeats on schedule | Auto-creates instances |
| **Personal Task** | User's private task | Not tied to a project |

### Task Duration vs Planned Hours

```
Duration  = How long the task spans on the calendar
Hours     = How much effort is required

Example:
  Duration: 5 days
  Planned Hours: 16 hours
  Assignment: 1 person at 40% allocation
  → Person works ~3.2 hrs/day over 5 days
```

## Duration Types — Deep Dive

### Simple Duration (Default)

```
Behavior:
  Duration = Fixed (user sets)
  Planned Hours = Fixed (user sets)
  Assignments = Adding/removing users does NOT change duration or hours

Example:
  Duration: 3 days, Hours: 24
  Assign 1 person → 8 hrs/day
  Assign 2 people → 8 hrs/day each (hours stay 24, NOT 48)

  *** Key: Hours are manually set, never recalculated ***
```

### Effort Driven

```
Behavior:
  Planned Hours = Fixed (preserved)
  Duration = Adjusts based on assignments
  More people → Shorter duration

Example:
  Hours: 40, Duration: 5 days, 1 person
  Add 2nd person → Duration shrinks to 2.5 days (hours stay 40)

  *** Key: "Adding resources finishes work faster" ***
```

### Calculated Assignment

```
Behavior:
  Duration = Fixed
  Planned Hours = Fixed
  Assignment % = Auto-calculated from hours ÷ (duration × schedule hours)

Example:
  Duration: 5 days, Hours: 20
  1 person → 50% allocation auto-calculated (20 ÷ 40)

  *** Key: System calculates how much of a person's time is needed ***
```

### Calculated Work

```
Behavior:
  Duration = Fixed (user sets)
  Assignment % = Fixed (user sets)
  Planned Hours = Auto-calculated from duration × allocation × schedule hours

Example:
  Duration: 5 days, Allocation: 50%
  Hours auto-calculated → 20 hours (5 × 8 × 0.5)

  *** Key: "Given this time and allocation, how many hours?" ***
```

### Duration Type Decision Tree

```
Need to fix the total hours of work?
├── YES → How should resources affect timeline?
│   ├── More people = shorter timeline → EFFORT DRIVEN
│   └── Timeline stays fixed → SIMPLE
└── NO → What do you want auto-calculated?
    ├── Calculate hours from duration + allocation → CALCULATED WORK
    └── Calculate allocation from hours + duration → CALCULATED ASSIGNMENT
```

## Predecessor Relationships

### Dependency Types

| Type | Code | Meaning | Example |
|---|---|---|---|
| Finish-Start | FS | B starts after A finishes | Design → Build |
| Start-Start | SS | B starts when A starts | Review + Testing (parallel) |
| Finish-Finish | FF | B finishes when A finishes | Development + Documentation |
| Start-Finish | SF | B finishes when A starts | Rare; shift handoff |

### Lag and Lead

```
Lag (positive offset):
  Task A (FS +2d) → Task B
  → B starts 2 days AFTER A finishes
  → Use for: waiting periods, drying time, review buffers

Lead (negative offset):
  Task A (FS -3d) → Task B
  → B starts 3 days BEFORE A finishes
  → Use for: overlapping work (fast-tracking)

Percentage Lag:
  Task A (FS +50%) → Task B
  → B starts after A finishes + 50% of A's duration
```

### Predecessor Enforcement

| Setting | Behavior |
|---|---|
| **Enforced** | Successor cannot start until predecessor satisfies the condition |
| **Not Enforced** | Dates shown but not locked; user can override |
| **Cross-Project** | Predecessor in another project; uses `ProjectID-TaskID` format |

> **Architect Tip:** Use cross-project predecessors sparingly. They create tight coupling between projects and can cause cascading timeline issues. For loose coupling, consider milestone-based coordination instead.

## Task Assignment Architecture

### Assignment Types

```
User Assignment:
  └── Direct assignment to a specific person
  └── Best for: known resources, small teams

Role Assignment:
  └── Assignment to a job role (not a person)
  └── Best for: templates, resource planning
  └── Resolved during resource management

Team Assignment:
  └── Assignment to a team
  └── Best for: shared work queues
  └── Shows in team's Workload Balancer
```

### Multiple Assignments

```
Task with multiple assignees:
├── Primary Assignee (marked with star)
│   └── Determines: task icon, notification defaults
├── Additional Assignees
│   └── Can have individual hour allocations
└── Allocation Split
    └── Planned Hours divided by assignment percentage

Example:
  Task: 40 hours
  User A: 60% → 24 hours
  User B: 40% → 16 hours
```

## Advanced Task Topics

### Recurring Tasks

- Created from project or template
- Recurrence patterns: Daily, Weekly, Monthly, Yearly
- Each occurrence creates a separate task
- Parent summary task groups all occurrences
- Useful for: status meetings, sprint reviews, monthly reports

### Task Approval Integration

```
Approval on a task:
├── Status changes to "Pending Approval"
├── Approver(s) notified
├── Outcomes:
│   ├── Approved → status changes to approved status
│   ├── Rejected → status reverts or changes to rejected status
│   └── Recalled → approval withdrawn
└── Impact: task cannot progress until approved
```

### Planned Completion Date Calculations

```
For ASAP constraint tasks:
  Planned Start = MAX(predecessors finish, project start)
  Planned Completion = Planned Start + Duration (skip non-work days)

For constrained tasks:
  Planned dates = Constraint date (may conflict with predecessors)
  → System flags constraint conflicts
```

---

### Practice Scenarios

**Scenario 1:** A task has Duration Type "Effort Driven" with 40 planned hours and 1 assignee (duration = 5 days). The PM adds a second assignee. What happens?

<details>
<summary>Answer</summary>

- **Planned Hours** remain **40 hours** (fixed in Effort Driven)
- **Duration** recalculates to **2.5 days** (40 hrs ÷ 2 people ÷ 8 hrs/day)
- Each person is allocated **20 hours** over 2.5 days
- The task finishes sooner because more resources are applied

Key insight: Effort Driven = "More people, less time, same total effort"
</details>

**Scenario 2:** An architect needs to set up a project where tasks in the "Development" phase must not start until the "Design Review" milestone is approved. How should this be configured?

<details>
<summary>Answer</summary>

1. Create a **milestone task** for "Design Review" with zero duration
2. Attach an **approval process** to the milestone task
3. Set **FS predecessors** from the milestone task to all development tasks
4. Set predecessors as **Enforced**
5. Until the milestone task is approved and marked complete, development tasks won't be schedulable

This creates a formal gate that prevents downstream work from starting prematurely.
</details>

**Scenario 3:** A project shows tasks with incorrect timelines — tasks overlap their predecessors. What's the likely cause?

<details>
<summary>Answer</summary>

Common causes (check in order):
1. **Task constraints** overriding predecessors (MSO, MFO, Fixed Dates)
2. **Update type** set to Manual — timelines haven't recalculated
3. **Predecessor enforcement** set to "Not Enforced"
4. **Lead time** (negative lag) on predecessor relationships
5. **Cross-project predecessors** with deleted/moved tasks

Resolution: Review each task's constraint type and change to ASAP where possible, then trigger timeline recalculation.
</details>

---

*[← Project Configuration](./project-configuration.md) | [Next: Issue Management →](./issue-management.md)*
