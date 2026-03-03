# Request Queues & Intake Design

[Back to Domain 2](./README.md) | [Back to AD0-E907 Index](../README.md)

---

## Request Queue Architecture

A Request Queue in Workfront is a **project configured to accept requests**. Requests become issues in that project.

### Queue Setup Components

```
Request Queue (Project)
├── Queue Details
│   ├── Queue Type: Help Request Queue
│   ├── Publish as Help Request Queue: Yes
│   ├── Who can add requests: specified access
│   ├── Default custom form(s)
│   └── Default approval process
├── Topic Groups (organizational categories)
│   ├── Topic Group: "IT Requests"
│   │   ├── Queue Topic: "Hardware Request"
│   │   ├── Queue Topic: "Software Request"
│   │   └── Queue Topic: "Access Request"
│   └── Topic Group: "Facilities"
│       ├── Queue Topic: "Office Move"
│       └── Queue Topic: "Maintenance"
├── Queue Topics (individual request types)
│   ├── Name & Description
│   ├── Custom Form(s)
│   ├── Approval Process
│   ├── Routing Rule
│   └── Default Duration & Assignee
└── Routing Rules
    ├── Default Routing Rule
    └── Topic-specific Routing Rules
```

### Queue Access Configuration

| Setting | Who Can Submit |
|---|---|
| **Anyone** | External users with URL (no login) |
| **Users with View access** | Users with View permission to the project |
| **Company users** | Users in the same company as the project |
| **Users in the project's group** | Users in the project's group/subgroup |
| **Users with Contribute access** | Users with Contribute permission |

> **Architect Tip:** For external-facing help desks, use "Anyone" with carefully scoped queue topics. For internal queues, restrict by group or company.

### Topic Groups (Nested Categories)

```
Multi-level topic groups:
IT Service Desk (Topic Group - Level 1)
├── Hardware (Topic Group - Level 2)
│   ├── Laptop Request (Queue Topic)
│   ├── Monitor Request (Queue Topic)
│   └── Peripheral Request (Queue Topic)
├── Software (Topic Group - Level 2)
│   ├── New Software (Queue Topic)
│   ├── Software Upgrade (Queue Topic)
│   └── License Request (Queue Topic)
└── Access & Accounts (Topic Group - Level 2)
    ├── New Account (Queue Topic)
    ├── Permission Change (Queue Topic)
    └── Account Deactivation (Queue Topic)

Maximum nesting depth: 10 levels
```

### Queue Topic Configuration

Each Queue Topic can have:

| Component | Purpose |
|---|---|
| **Custom Form** | Collect topic-specific data |
| **Approval Process** | Auto-attach approval when request is created |
| **Routing Rule** | Auto-assign to user/role/team |
| **Default Duration** | Pre-set expected completion time |
| **Default Assignee** | Pre-assign to specific user/role |

### Routing Rules — Deep Dive

```
Routing Rule Components:
├── Name
├── Default Route (fallback)
│   ├── Assign to: User / Role / Team
│   └── Project: where issue is created
└── Routing Rule Mappings
    ├── Queue Topic → Route Target
    └── Multiple mappings per rule

Route Target Options:
├── User → Direct assignment
├── Role → Shows in unassigned work
├── Team → Shows in team's request queue
└── Project → Creates issue in specific project
```

### Intake Design Patterns

#### Pattern 1: Centralized Intake

```
Single Help Desk Queue
├── All departments submit here
├── Triage team reviews and routes
├── Topic groups organize by department
└── Best for: small-medium organizations
```

#### Pattern 2: Distributed Intake

```
Department-Specific Queues
├── IT Service Desk Queue
├── Marketing Request Queue
├── HR Service Queue
├── Facilities Queue
└── Best for: large organizations with specialized teams
```

#### Pattern 3: Tiered Intake

```
Tier 1: Self-Service Queue
├── Common requests auto-routed
├── Simple approvals
└── Template-based responses

Tier 2: Specialist Queue
├── Escalated from Tier 1
├── Complex requests
└── Expert review required

Tier 3: Project Conversion
├── Major work items
├── Converted to projects
└── Full project management
```

## Request Form Design Best Practices

1. **Keep it short** — Only ask for essential information
2. **Use display logic** — Show relevant fields based on topic/type
3. **Typeahead fields** — For selecting projects, users, etc.
4. **Dropdown menus** — For categorization (easier to report on)
5. **Required fields** — Mark truly essential fields only
6. **Section breaks** — Organize long forms into logical sections
7. **Descriptive text** — Guide users through the form

---

### Practice Scenarios

**Scenario 1:** A company wants to implement an IT help desk where:
- External vendors can submit requests without logging in
- Hardware requests need manager approval
- Software requests route to the IT procurement team
- Access requests route to the security team

Design the queue structure.

<details>
<summary>Answer</summary>

**Queue Configuration:**
- Project: "IT Help Desk" — Published as Help Request Queue
- Access: "Anyone" (for external vendor access)
- Use a public URL for external submissions

**Topic Groups & Queue Topics:**
```
IT Help Desk (Queue)
├── Hardware Requests (Topic Group)
│   ├── Laptop Request → Custom Form: Hardware Details
│   │   └── Approval: Manager Approval Process
│   │   └── Route: IT Hardware Team
│   └── Equipment Request → Custom Form: Hardware Details
│       └── Approval: Manager Approval Process
│       └── Route: IT Hardware Team
├── Software Requests (Topic Group)
│   ├── New Software → Custom Form: Software Details
│   │   └── Route: IT Procurement Team
│   └── License Request → Custom Form: License Details
│       └── Route: IT Procurement Team
└── Access Requests (Topic Group)
    ├── New Account → Custom Form: Access Details
    │   └── Route: Security Team
    └── Permission Change → Custom Form: Access Details
        └── Route: Security Team
```

Each queue topic has its own custom form tailored to the request type, appropriate approval process, and routing rule.
</details>

**Scenario 2:** Request queue performance is slow — users report long load times when submitting requests. What should you investigate?

<details>
<summary>Answer</summary>

1. **Custom form complexity** — Too many calculated fields evaluating on load
2. **Typeahead fields** — Searching large user/project sets
3. **Display logic rules** — Complex cascading logic slowing rendering
4. **Number of queue topics** — Excessive topics in dropdown
5. **External lookup fields** — API calls timing out
6. **Approval process evaluation** — Complex multi-path approvals evaluating on creation

**Optimization:**
- Simplify calculated fields
- Limit typeahead result sets with filters
- Flatten topic group hierarchy
- Cache external lookup results
- Simplify display logic chains
</details>

---

*[Back to Domain 2](./README.md) | [Next: Approval Processes →](./approval-processes.md)*
