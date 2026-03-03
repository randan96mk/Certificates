# Domain 1: Core Work Management (~26%)

[Back to AD0-E907 Index](../README.md)

---

## Overview

This is the **highest-weighted domain** on the AD0-E907 exam. It covers the foundational building blocks of Workfront: projects, tasks, issues, and their interrelationships.

## Topics Covered

1. [Project Configuration & Management](./project-configuration.md)
2. [Task Management & Dependencies](./task-management.md)
3. [Issue Management & Conversion](./issue-management.md)
4. [Object Model & Relationships](./object-model.md)

---

## Key Concepts to Master

### Workfront Object Hierarchy
- Portfolios → Programs → Projects → Tasks/Issues
- Understanding parent-child relationships
- Object sharing and permission inheritance

### Project Lifecycle
- Planning → Current → Complete (standard flow)
- Dead vs On Hold states
- Group-level status customization

### Duration Types — Comparison Table

| Duration Type | Planned Hours | Duration | Assignments | Use When |
|---|---|---|---|---|
| **Simple** | Fixed (manual) | Fixed | Variable | Default; fixed scope |
| **Effort Driven** | Fixed | Adjusts with assignments | Variable | Adding people should finish sooner |
| **Calculated Assignment** | Fixed | Fixed | Auto-calculated | Hours/duration known, calc % allocation |
| **Calculated Work** | Auto-calculated | Fixed | Fixed | Duration and % known, calc hours |

### Task Constraints — Comparison Table

| Constraint | Abbreviation | Behavior |
|---|---|---|
| As Soon As Possible | ASAP | Default; earliest possible date |
| As Late As Possible | ALAP | Latest possible date |
| Must Start On | MSO | Forces specific start date |
| Must Finish On | MFO | Forces specific finish date |
| Start No Earlier Than | SNET | Cannot start before date |
| Start No Later Than | SNLT | Must start by date |
| Finish No Earlier Than | FNET | Cannot finish before date |
| Finish No Later Than | FNLT | Must finish by date |
| Fixed Dates | FD | Both dates locked |

> **Architect Tip:** In most enterprise implementations, use ASAP/ALAP as defaults and only apply hard constraints (MSO/MFO) when business rules require fixed dates. Over-constraining causes scheduling rigidity.

---

*[Back to AD0-E907 Index](../README.md)*
