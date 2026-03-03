# AD0-E907 — Domain Breakdown & Detailed Objectives

[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E907 Index](../README.md)

---

## Domain 1: Core Work Management (~26%)

### Objectives
- Configure and manage projects, tasks, and issues
- Understand object relationships and hierarchy in Workfront
- Apply project templates and configure project settings
- Manage task dependencies, constraints, and duration types
- Configure milestone paths and project timelines
- Understand issue routing and conversion (issue → task/project)
- Apply status workflows and group-level status customization

### Key Topics

#### Project Configuration
- Project creation methods: template, blank, copy, MS Project import
- Project statuses: Planning, Current, Complete, Dead, On Hold
- Timeline modes: Manual vs Auto
- Schedule modes: Project start date vs Completion date
- Update types: Automatic, Automatic and On Change, Change Only, Manual

#### Task Management
- Duration Types: Simple, Effort Driven, Calculated Assignment, Calculated Work
- Task Constraints: ASAP, ALAP, Must Start On, Must Finish On, Start No Earlier Than, etc.
- Predecessors: FS (Finish-Start), SS (Start-Start), FF (Finish-Finish), SF (Start-Finish)
- Lag and Lead time on dependencies
- Task assignment: roles, teams, users
- Planned Hours vs Actual Hours vs Remaining Hours

#### Issue Management
- Issue types: Bug Report, Change Order, Issue, Request
- Issue conversion to tasks or projects
- Resolving objects
- Queue topics and routing rules

#### Object Hierarchy

```
Portfolio
└── Program
    └── Project
        ├── Task
        │   ├── Subtask
        │   └── Assignment
        ├── Issue
        │   └── Assignment
        ├── Document
        │   └── Proof
        ├── Expense
        ├── Risk
        ├── Baseline
        └── Update/Note
```

---

## Domain 2: Intake, Planning, and Processes (~22%)

### Objectives
- Design and implement request queues
- Configure queue topics, topic groups, and routing rules
- Set up approval processes (single-path and multi-path)
- Implement milestone paths and project templates
- Configure blueprints
- Design intake workflows for different use cases

### Key Topics

#### Request Queues
- Queue Details configuration
- Queue Topics and Topic Groups (hierarchy)
- Routing Rules — route to user, team, role, or project
- Default routing
- Queue access: who can submit requests
- Request types mapping

#### Approval Processes
- Single-path vs multi-path approvals
- System-wide vs group-level approvals
- Approval stages and approvers (user, role, team)
- Approval delegation
- Auto-approval settings
- Recall and rejection behavior

#### Project Templates
- Template tasks and predecessor relationships
- Template sharing and access
- Financial settings in templates
- Recurring tasks
- Template queue setup

#### Blueprints
- Adobe-provided blueprints
- Custom blueprints
- Blueprint installation and customization

#### Milestone Paths
- Creating milestone paths
- Associating milestones to tasks
- Milestone view on project lists

---

## Domain 3: Reporting, Dashboards, and Analytics (~16%)

### Objectives
- Build and customize reports using Report Builder
- Apply text mode for advanced reporting
- Create and share dashboards
- Use Workfront Analytics (Enhanced Analytics)
- Understand calculated fields in reports
- Apply filters, views, and groupings

### Key Topics

#### Report Builder
- Report types: Project, Task, Issue, Hour, Document, User, etc.
- Filters: basic and advanced conditions
- Views: column configuration
- Groupings: up to 3 levels
- Charts: bar, column, line, pie, gauge
- Matrix reports
- Prompts and prompt-based reports

#### Text Mode (Critical for Expert Level)

```
# Example: Text mode filter for tasks due this week
displayname=Due This Week
valueexpression=WEEKOFYEAR({plannedCompletionDate})=WEEKOFYEAR($$TODAY)
valueformat=HTML
type=tkval
```

```
# Example: Text mode column with calculated values
displayname=Days Overdue
valueexpression=DATEDIFF($$TODAY,{plannedCompletionDate})
valueformat=int
textmode=true
```

- Text mode in filters, views, and groupings
- Collection references (e.g., tasks within projects)
- Wildcards: `$$USER`, `$$TODAY`, `$$NOW`
- `valueexpression` vs `value`
- `sharecol` for merged columns
- Conditional formatting via `styledef`

#### Enhanced Analytics
- Burndown chart
- Tasks in flight
- Project activity
- Team capacity
- Resource capacity visualization
- Filters and date ranges

#### Dashboards
- Creating dashboards
- External pages in dashboards
- Dashboard sharing and access
- Dashboard layout best practices

---

## Domain 4: Custom Forms, Access, and Layout (~14%)

### Objectives
- Design custom forms with display logic and skip logic
- Configure calculated custom fields
- Implement access levels and object-level permissions
- Create and assign layout templates
- Manage group administration

### Key Topics

#### Custom Forms
- Field types: text, dropdown, checkbox, radio, date, calculated, typeahead, external lookup
- Display logic (show/hide fields based on other field values)
- Skip logic (skip fields based on conditions)
- Section breaks with permissions
- Calculated fields and expressions:

```
# Example: Calculated field — business days between two dates
WEEKDAYDIFF({actualStartDate},{actualCompletionDate})
```

```
# Example: Concatenation
CONCAT({owner}.{name}," - ",{name})
```

- Common expressions: `IF`, `CONCAT`, `DATEDIFF`, `WEEKDAYDIFF`, `LEFT`, `RIGHT`, `CONTAINS`, `IN`, `ISBLANK`
- Multi-object custom forms
- Custom form categories

#### Access Levels
- License types: Plan, Work, Review, Request, External
- Access level components: View, Edit, No Access
- Fine-tuning access: project, task, issue, document, user, etc.
- Administrative access to custom forms, approval processes, etc.
- Access level inheritance

#### Object-Level Permissions
- Manage, Contribute, View
- Permission inheritance (project → task → issue)
- Sharing: users, teams, roles, groups
- System-wide sharing defaults

#### Layout Templates
- Customizing main menu
- Customizing left panel
- Customizing detail views
- Pinning pages
- Landing page configuration
- Terminology customization
- Layout template assignment priority

#### Group Administration
- Group-level statuses
- Group-level approval processes
- Group-level layout templates
- Subgroups
- Group administrators

---

## Domain 5: Resource Management (~10%)

### Objectives
- Configure and use the Workload Balancer
- Use the Resource Planner
- Understand resource pools and scheduling
- Apply job roles and resource allocation
- Manage utilization

### Key Topics

#### Workload Balancer
- Assigned vs Unassigned work
- Filters and views
- Bulk assignments
- Daily, weekly, monthly views
- Contour assignments
- Managing overallocation

#### Resource Planner
- Project view vs Role view vs User view
- Available, Planned, Budgeted, and Actual hours
- FTE vs Hours
- NET calculation
- Exporting data

#### Resource Pools
- Creating and managing resource pools
- Associating resource pools with projects and users
- Role-based planning

#### Utilization Report
- Revenue vs Cost tracking
- Planned vs Actual vs Budgeted
- Billing rates and cost rates

---

## Domain 6: Workfront Fusion and Integrations (~8%)

### Objectives
- Design and build Fusion scenarios
- Understand modules, connections, and data flow
- Configure triggers (instant vs polling)
- Handle errors and incomplete executions
- Work with Workfront API
- Implement webhooks

### Key Topics

#### Fusion Scenario Design
- Triggers: Watch modules (polling) vs Webhooks (instant)
- Action modules: Create, Read, Update, Delete, Search
- Routers and filters
- Iterators and aggregators
- Data stores
- Error handling: Commit, Rollback, Ignore, Break, Resume

#### Fusion Architecture Patterns

```
┌──────────┐     ┌──────────┐     ┌──────────┐
│  Trigger  │ ──▶│  Router   │ ──▶│ Action 1  │
│ (Webhook) │     │           │     │ (Create)  │
└──────────┘     │           │     └──────────┘
                  │           │     ┌──────────┐
                  │           │ ──▶│ Action 2  │
                  │           │     │ (Update)  │
                  └──────────┘     └──────────┘
```

- Scenario scheduling and on-demand execution
- Blueprint scenarios
- Organizational folders and teams

#### Workfront API
- REST API basics
- Authentication (API key, OAuth2)
- Common endpoints: `/attask/api/v17.0/`
- CRUD operations
- Search and filter parameters
- Event subscriptions
- Bulk operations

#### Webhooks
- Incoming webhooks
- Outgoing event subscriptions
- Webhook vs polling trade-offs

---

## Domain 7: Proofing and Document Management (~4%)

### Objectives
- Configure proofing workflows
- Set up automated workflows for proofs
- Manage document routing and approvals
- Understand proofing roles and permissions

### Key Topics

#### Proofing
- Proof roles: Read Only, Reviewer, Approver, Reviewer & Approver, Author, Moderator
- Automated workflow templates
- Proof stages and deadlines
- Email notifications for proofs
- Proof viewer (Web vs Desktop)
- Proof decisions: Approved, Approved with changes, Changes required, Not relevant

#### Document Management
- Document upload and versioning
- Document folders
- External document integrations (Experience Manager Assets, Google Drive, SharePoint, etc.)
- Document approvals (separate from proof approvals)
- Linked folders (AEM integration)

---

## Study Priority Matrix

| Priority | Domain | Weight | Recommendation |
|---|---|---|---|
| **P0 — Critical** | Core Work Management | 26% | Must master thoroughly |
| **P0 — Critical** | Intake, Planning, Processes | 22% | Must master thoroughly |
| **P1 — High** | Reporting & Analytics | 16% | Focus on text mode |
| **P1 — High** | Custom Forms, Access, Layout | 14% | Focus on calculated fields |
| **P2 — Medium** | Resource Management | 10% | Workload Balancer focus |
| **P2 — Medium** | Fusion & Integrations | 8% | Scenario design patterns |
| **P3 — Low** | Proofing & Documents | 4% | Basic understanding |

---

*[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E907 Index](../README.md)*
