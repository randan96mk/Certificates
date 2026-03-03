# Domain 6: Methodology / Best Practices / Use Cases (~22%) — Includes Fusion & Integrations

[Back to AD0-E907 Index](../README.md)

---

## Overview

This is the **highest-weighted domain** at 22%. It tests practical application of Workfront methodology, best practices, financial tracking, governance, and Boards usage.

## Key Exam Objectives

- Set up tracking of deliverables within a single campaign
- Demonstrate strategic prioritization and justification of work
- Demonstrate financials, utilization, forecasting, billing rates/records
- Set up workflow and approvals
- Recommend governance framework for system administration scaling
- Identify areas for consideration when expanding to additional teams
- Demonstrate Workfront Boards native features
- Recommend governance frameworks for system and group administration

---

## Workfront Fusion & Integration Patterns

### Fusion Scenario Architecture

```
Scenario Components:
├── Trigger (first module)
│   ├── Polling trigger (scheduled check)
│   │   └── Runs on schedule: every 15 min, hourly, etc.
│   └── Instant trigger (webhook)
│       └── Fires immediately on event
├── Action Modules
│   ├── Create Record
│   ├── Update Record
│   ├── Delete Record
│   ├── Read Record
│   └── Search Records
├── Flow Control
│   ├── Router (parallel paths)
│   ├── Iterator (loop through arrays)
│   ├── Aggregator (combine results)
│   ├── Repeater (loop N times)
│   └── Sleep (wait between actions)
├── Error Handling
│   ├── Commit (save partial results)
│   ├── Rollback (undo all changes)
│   ├── Ignore (skip and continue)
│   ├── Break (stop and queue for retry)
│   └── Resume (retry from failure point)
└── Data Stores
    ├── Key-value storage
    ├── Cross-scenario data sharing
    └── Lookup tables
```

### Common Fusion Patterns

#### Pattern 1: Intake Automation
```
Webhook → Parse Email → Create Workfront Project from Template →
  Assign Team → Send Slack Notification
```

#### Pattern 2: Time Tracking Sync
```
Schedule (daily) → Get Workfront Hours → Transform Data →
  Push to Finance System → Log Success/Failure
```

#### Pattern 3: Document Routing
```
Watch Workfront Documents → Check Metadata → Router
  → Path A: If type=Contract → Upload to SharePoint
  → Path B: If type=Creative → Upload to AEM Assets
  → Path C: Default → Archive to Google Drive
```

#### Pattern 4: Cross-System Status Sync
```
Workfront Event Subscription → Webhook → Get Project Details →
  Update Jira Issue → Update Salesforce Opportunity
```

### Error Handling Best Practices

| Strategy | When to Use | Behavior |
|---|---|---|
| **Ignore** | Non-critical operations | Skips failed module, continues |
| **Break** | Transient errors (API timeout) | Queues for manual retry |
| **Rollback** | Data consistency required | Undoes all changes in execution |
| **Commit** | Partial success acceptable | Saves successful operations |
| **Resume** | Retry with modified data | Retries from failure point |

### Workfront API Integration

```
API Endpoint: /attask/api/v17.0/{object}

Authentication:
├── API Key (in header or query param)
├── Session ID (login endpoint)
└── OAuth 2.0 (recommended for integrations)

Common Operations:
├── GET /project/search?status=CUR
├── POST /project {"name":"New Project","templateID":"xxx"}
├── PUT /task/{id} {"status":"INP"}
├── DELETE /issue/{id}
└── GET /project/{id}?fields=tasks:*,owner:*

Event Subscriptions:
├── POST /eventSubscription
├── Webhook URL receives events
├── Filter by object type and event type
└── Events: CREATE, UPDATE, DELETE
```

## Methodology & Best Practices

### Campaign Tracking Pattern

```
Campaign Tracking Architecture:
├── Portfolio: "Marketing Campaigns 2026"
│   ├── Program: "Q2 Campaigns"
│   │   ├── Project: "Spring Launch" (from template)
│   │   │   ├── Phase 1: Strategy (milestone)
│   │   │   ├── Phase 2: Content Creation
│   │   │   ├── Phase 3: Asset Production
│   │   │   ├── Phase 4: Channel Deployment
│   │   │   └── Phase 5: Measurement
│   │   └── Project: "Partner Co-brand"
│   └── Program: "Q3 Campaigns"
└── Dashboard: "Campaign Performance"
    ├── Status by campaign (chart)
    ├── Budget vs actual (table)
    └── Timeline view (Gantt-style)
```

### Financial Management

#### Cost Types

| Cost Type | Source | Calculated From |
|---|---|---|
| **No Cost** | N/A | Zero cost |
| **Fixed Hourly** | Fixed rate | Hours × Fixed Rate |
| **User Hourly** | User's cost rate | Hours × User Rate |
| **Role Hourly** | Role's cost rate | Hours × Role Rate |

#### Revenue Types

| Revenue Type | Source | Calculated From |
|---|---|---|
| **Not Billable** | N/A | Zero revenue |
| **Fixed Revenue** | Flat amount | Fixed amount on task |
| **User Hourly** | User's billing rate | Hours × User Billing Rate |
| **Role Hourly** | Role's billing rate | Hours × Role Billing Rate |
| **User/Role with Cap** | Capped amounts | Min(Hours × Rate, Cap) |

#### Billing Records

```
Billing Record:
├── Associated with project
├── Status: Billable, Not Billable, Billed
├── Contains:
│   ├── Task hours
│   ├── Issue hours
│   ├── Project hours
│   ├── Expenses
│   └── Fixed revenue
├── Once marked "Billed":
│   └── Locked — hours/expenses cannot be modified
└── Use for: Client invoicing, revenue recognition
```

### Governance Framework

```
Governance Architecture:
├── Tier 1: System Administration
│   ├── System admins (2-3 max)
│   ├── Responsible for: access levels, global settings
│   └── Change control for system-wide changes
├── Tier 2: Group Administration
│   ├── Group admins per department
│   ├── Responsible for: group statuses, templates, approvals
│   └── First point of contact for group users
├── Tier 3: Power Users
│   ├── Report builders, dashboard creators
│   ├── Responsible for: departmental reporting
│   └── Liaison between users and admins
└── Tier 4: End Users
    ├── Task completion, time logging
    └── Request submission

Governance Documents:
├── Naming conventions
├── Object creation guidelines
├── Custom form standards
├── Approval process matrix
├── Escalation procedures
└── Change request process
```

### Scaling to Multiple Teams

```
Expansion Considerations:
├── Access Level Architecture
│   ├── Review existing access levels
│   ├── Create new levels if different roles needed
│   └── Don't over-customize (keep ≤10 access levels)
├── Group Structure
│   ├── New groups per department/team
│   ├── Subgroups for sub-teams
│   └── Group admin assignments
├── Template Library
│   ├── Existing templates: still valid?
│   ├── New templates for new team processes
│   └── Shared vs team-specific templates
├── Layout Templates
│   ├── New layouts per audience
│   ├── Terminology for new teams
│   └── Menu customization
├── Training & Adoption
│   ├── New user onboarding plan
│   ├── Training materials
│   └── Champions program
└── Process Alignment
    ├── Standardize where possible
    ├── Allow team flexibility where needed
    └── Document decisions
```

### Workfront Boards

```
Boards Use Cases:
├── Sprint/Iteration Tracking
│   ├── Columns: Backlog, In Progress, Review, Done
│   ├── Connected cards linked to tasks
│   └── WIP limits per column
├── Intake Triage
│   ├── Columns: New, Evaluating, Approved, Rejected
│   ├── Ad-hoc cards for incoming requests
│   └── Convert to tasks when approved
├── Status Overview
│   ├── Columns by project phase
│   ├── Cards show key metrics
│   └── Used for stand-up meetings
└── Personal Task Management
    ├── Individual board per user
    ├── Mix of ad-hoc and connected cards
    └── Kanban for personal workflow
```

---

### Practice Scenarios

**Scenario 1:** A company is expanding Workfront from the Marketing department (50 users) to include IT (30 users) and HR (20 users). What governance recommendations would you make?

<details>
<summary>Answer</summary>

**Governance Recommendations:**

1. **Group Structure:**
   - Create groups: IT Department, HR Department
   - Assign group admins for each
   - Create subgroups if needed (IT-Development, IT-Support)

2. **Access Levels:**
   - Review if existing access levels cover IT/HR roles
   - Create 1-2 new access levels if needed (avoid proliferation)
   - IT may need more Setup access than Marketing

3. **Templates:**
   - IT: Software Deployment, Incident Response, Change Request
   - HR: Recruitment, Onboarding, Performance Review
   - Use naming convention: `[DEPT]-[Process]-v[X]`

4. **Layout Templates:**
   - IT layout: Include Setup, hide Marketing-specific items
   - HR layout: Simplified menu, focus on Requests and Home
   - Keep Marketing layout unchanged

5. **Governance:**
   - Establish change advisory board (1 rep per department)
   - Document naming conventions for cross-department use
   - Set up shared reporting standards
   - Create onboarding training per department
   - Assign power users (champions) per department

6. **Process Standardization:**
   - Use same request queue pattern (one per department)
   - Standardize approval process structure
   - Shared milestone paths where applicable
</details>

---

*[Back to AD0-E907 Index](../README.md)*
