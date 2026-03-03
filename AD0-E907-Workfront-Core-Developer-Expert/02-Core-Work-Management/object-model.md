# Workfront Object Model & Relationships

[Back to Domain 1](./README.md) | [Back to AD0-E907 Index](../README.md)

---

## Complete Object Hierarchy

```
Company
├── Group
│   ├── Subgroup
│   └── Users
├── Portfolio
│   └── Program
│       └── Project ◄──── Template
│           ├── Task
│           │   ├── Subtask
│           │   ├── Assignment (User/Role/Team)
│           │   ├── Approval
│           │   ├── Predecessor
│           │   ├── Document → Proof
│           │   ├── Expense
│           │   ├── Hour Entry
│           │   └── Note/Update
│           ├── Issue
│           │   ├── Assignment
│           │   ├── Approval
│           │   ├── Document
│           │   ├── Hour Entry
│           │   ├── Resolving Object
│           │   └── Note/Update
│           ├── Milestone Path → Milestone
│           ├── Risk
│           ├── Baseline
│           ├── Billing Record
│           ├── Queue (Request Queue)
│           │   ├── Topic Group
│           │   ├── Queue Topic
│           │   └── Routing Rule
│           └── Custom Form ◄──── Custom Form Definition
├── Team
│   └── Members (Users)
├── Schedule
├── Timesheet Profile
│   └── Timesheet → Hour Entry
└── Resource Pool
    └── Users
```

## Key Object Relationships

### One-to-Many Relationships
| Parent | Children |
|---|---|
| Portfolio | Programs |
| Program | Projects |
| Project | Tasks, Issues, Risks, Baselines, Documents |
| Task | Subtasks, Assignments, Hour Entries, Documents |
| User | Assignments, Hour Entries, Timesheets |
| Group | Users, Subgroups, Statuses, Layout Templates |

### Many-to-Many Relationships
| Object A | Object B | Via |
|---|---|---|
| User | Project | Assignment/Team/Access |
| User | Resource Pool | Membership |
| Custom Form | Object (Project/Task/Issue) | Form attachment |
| User | Team | Team membership |

## Object API Names (For Text Mode & API)

| UI Name | API Object Code | API Name |
|---|---|---|
| Project | PROJ | project |
| Task | TASK | task |
| Issue | OPTASK | opTask |
| User | USER | user |
| Portfolio | PORT | portfolio |
| Program | PRGM | program |
| Document | DOCU | document |
| Hour Entry | HOUR | hour |
| Note/Update | NOTE | note |
| Assignment | ASSGN | assignment |
| Expense | EXPNS | expense |
| Team | TEAMOB | team |
| Company | CMPY | company |
| Group | GROUP | group |
| Role | ROLE | role |
| Template | TMPL | template |
| Template Task | TTSK | templateTask |
| Approval Process | ARVPRC | approvalProcess |
| Baseline | BLIN | baseline |
| Timesheet | TSHET | timesheet |

> **Architect Tip:** Knowing API object codes is essential for text mode reporting and Fusion scenario design. The most commonly tested ones are PROJ, TASK, OPTASK, USER, and HOUR.

## Object Permissions Model

```
Permission Levels:
├── View    — Can see the object
├── Contribute — Can view + add updates, hours, documents
└── Manage  — Full control (edit, delete, share)

Inheritance:
  Project permissions → automatically grant task/issue permissions
  (Can be overridden at object level)

Sharing Sources:
├── Direct sharing (explicit)
├── Inherited from project
├── Team membership
├── Job role
├── Home group
├── Portfolio/Program manager
├── Access level defaults
└── System-wide sharing preferences
```

### Permission Inheritance Flow

```
User gets Manage on Project
    │
    ├── Automatically gets Contribute on Tasks (default)
    ├── Automatically gets Contribute on Issues (default)
    └── Automatically gets View on Documents

    These defaults can be modified in:
    Setup > Access Levels > Fine-tune permissions
```

## Custom Data Model

### Custom Forms Architecture

```
Custom Form Definition
├── Name & Description
├── Object Type(s): Project, Task, Issue, User, etc.
├── Sections
│   ├── Section Break (with permissions)
│   └── Fields
│       ├── Text (single line, paragraph)
│       ├── Number
│       ├── Date
│       ├── Dropdown (single/multi select)
│       ├── Checkbox
│       ├── Radio Button
│       ├── Typeahead (user, project, etc.)
│       ├── Calculated
│       ├── External Lookup
│       └── Descriptive Text (no data)
├── Display Logic Rules
│   └── Show/hide field based on other field value
├── Skip Logic Rules
│   └── Skip to field/section based on condition
└── Sharing (who can attach this form)
```

### Calculated Field Cross-References

```
Calculated fields can reference:
├── Fields on the same form
├── Fields on other attached forms
├── Native Workfront fields
├── Parent object fields (e.g., {project}.{name} from a task)
└── Collection fields (limited)

Common expressions:
  IF({status}="CPL","Complete","In Progress")
  DATEDIFF({actualCompletionDate},{plannedCompletionDate})
  CONCAT({owner}.{firstName}," ",{owner}.{lastName})
  ROUND({actualCost}/{plannedCost}*100,2)
```

## Notifications Architecture

```
Notification Types:
├── Event Notifications (Setup level)
│   ├── Triggered by system events
│   ├── Controlled by admin at system level
│   └── Can be overridden by user preferences
├── Reminder Notifications
│   ├── Date-based triggers
│   ├── Custom timing (before/after/on date)
│   └── Attached to projects, tasks, issues
├── Automatic Reminders
│   ├── System-wide date-based
│   └── Before/after planned dates
└── Custom Notifications (via Fusion)
    ├── Flexible trigger conditions
    └── Custom message content
```

---

### Quick Reference Card

| Concept | What to Remember |
|---|---|
| Issue API name | `opTask` (not `issue`) |
| Hour Entry | Logs time against task, issue, or project |
| Resolving Object | Task/Project/Issue that resolves an issue |
| Summary Task | Parent task; values roll up from children |
| Baseline | Point-in-time snapshot; max 10 per project |
| Billing Record | Groups hours/expenses for invoicing |
| Milestone Path | Defined at project level; milestones assigned to tasks |

---

*[← Issue Management](./issue-management.md) | [Back to Domain 1](./README.md)*
