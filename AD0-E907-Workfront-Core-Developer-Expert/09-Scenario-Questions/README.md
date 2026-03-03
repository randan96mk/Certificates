# AD0-E907 — Scenario-Based Questions

[Back to AD0-E907 Index](../README.md)

---

## About Scenario Questions

The AD0-E907 exam is heavily scenario-based (especially Domains 6 & 7, totaling 33%). These questions present real-world situations and ask you to recommend the best approach.

---

## Scenario 1: Enterprise PMO Setup [Architect]

**Situation:** A global manufacturing company with 500+ employees across 4 regions is implementing Workfront. They have 3 PMOs (Engineering, Marketing, Operations) each with different processes. They need:
- Standardized project intake
- Regional visibility controls
- Cross-functional resource management
- Executive portfolio dashboards

**Question:** How would you architect this implementation?

<details>
<summary>Detailed Solution</summary>

### Organization Architecture

```
Company: GlobalMfg Corp
├── Group: Engineering PMO
│   ├── Subgroup: EMEA Engineering
│   ├── Subgroup: APAC Engineering
│   └── Subgroup: Americas Engineering
├── Group: Marketing PMO
│   ├── Subgroup: Digital Marketing
│   └── Subgroup: Brand Marketing
├── Group: Operations PMO
│   ├── Subgroup: Supply Chain
│   └── Subgroup: Quality
└── Group: Executive Office (view-only dashboards)
```

### Intake Architecture
- **Per-PMO request queues** with department-specific queue topics
- **Routing rules** route to team leads based on topic
- **Standardized custom forms** (shared fields) + department-specific forms
- **Approval process**: Manager approval → PMO director approval

### Access Architecture
- **Access levels** (6 max):
  - System Admin, PMO Director, Project Manager, Team Member, Stakeholder, External
- **Group-level delegation**: Each PMO director = group admin
- **Regional visibility**: Group/subgroup sharing restricts cross-region visibility
- **Portfolio managers** see cross-regional data

### Resource Architecture
- **Resource pools per region and skill set**
- **Job roles standardized across regions** (same role names)
- **Workload Balancer** available to PMs per group
- **Resource Planner** at portfolio level for capacity planning

### Dashboard Architecture
- **Executive Dashboard**: Portfolio Optimizer view, cross-PMO status, budget vs actual
- **PMO Dashboards**: Per-department project status, resource utilization, risk register
- **PM Dashboards**: Individual project health, team workload, timeline view
</details>

---

## Scenario 2: Change Management During Migration [Architect]

**Situation:** A marketing agency is migrating from spreadsheets and email to Workfront. There are 80 users, many resistant to change. The CEO wants full adoption in 90 days. Current pain points: lost emails, duplicate work, no visibility into project status.

**Question:** Design a change management and implementation strategy.

<details>
<summary>Detailed Solution</summary>

### Phase 1: Foundation (Days 1-30)
- **Stakeholder alignment workshop**: Map current processes to Workfront capabilities
- **Champions program**: Identify 5-8 power users across teams for early training
- **Process documentation**: Document current workflows before configuring Workfront
- **Sandbox setup**: Configure in sandbox; validate with champions
- **Communication**: "What's changing and why" email series

### Phase 2: Pilot (Days 31-60)
- **Pilot group**: 15-20 users from most receptive team
- **Limited scope**: Start with project tracking and request intake only
- **Feedback loops**: Weekly check-ins with pilot group
- **Iterate**: Adjust configuration based on feedback
- **Quick wins**: Show dashboard demonstrating improved visibility vs. spreadsheets

### Phase 3: Rollout (Days 61-90)
- **Phased rollout**: One department per week
- **Role-based training**: 2-hour sessions per role (PM, Team Member, Exec)
- **Office hours**: Daily 30-min drop-in sessions for first 2 weeks
- **Retire old tools**: Gradually close spreadsheet access
- **Adoption metrics**: Track login frequency, task completion, time logging

### Change Management Tactics
| Tactic | Purpose |
|---|---|
| Involve resistors in design | Give them ownership |
| Show personal benefit | "You'll stop losing emails" |
| Executive sponsorship | CEO sends kickoff message |
| Simplify initial experience | Reduced menu via layout templates |
| Celebrate wins | Share success stories publicly |
| Measure and report | Weekly adoption dashboard |
</details>

---

## Scenario 3: Cross-Functional Workflow Design [Advanced]

**Situation:** A content team creates marketing assets with this workflow:
1. Marketing manager submits creative brief
2. Creative director reviews and prioritizes
3. Designer creates assets
4. Copywriter adds content
5. Legal reviews for compliance
6. Marketing manager gives final approval
7. Assets are published

**Question:** Design the Workfront configuration for this workflow.

<details>
<summary>Detailed Solution</summary>

### Project Template: "Creative Asset Production"

```
Template Tasks:
├── 1. Creative Brief Submission (Marketing Mgr)
│   └── Custom Form: Brief Details (brand, audience, specs)
│   └── Duration Type: Simple, 1 day
├── 2. Creative Review & Prioritization (Creative Director)
│   └── Predecessor: FS to Task 1
│   └── Approval Process: CD approval
│   └── Milestone: Brief Approved ★
│   └── Duration Type: Simple, 2 days
├── 3. Asset Design (Designer Role)
│   └── Predecessor: FS to Task 2
│   └── Duration Type: Effort Driven, 5 days
│   └── Proof: Upload design for review
├── 4. Copy Development (Copywriter Role)
│   └── Predecessor: SS+2d to Task 3 (starts 2 days into design)
│   └── Duration Type: Simple, 3 days
├── 5. Legal Compliance Review (Legal Team)
│   └── Predecessor: FF to Tasks 3 & 4
│   └── Proof: Automated workflow - Legal review stage
│   └── Duration Type: Simple, 3 days
├── 6. Final Approval (Marketing Mgr)
│   └── Predecessor: FS to Task 5
│   └── Approval Process: Marketing Manager approval
│   └── Milestone: Approved for Publication ★
│   └── Duration Type: Simple, 1 day
└── 7. Publish Assets (Marketing Team)
    └── Predecessor: FS to Task 6
    └── Duration Type: Simple, 1 day

Proofing Workflow:
├── Stage 1: Designer uploads → Internal review (CD)
├── Stage 2: Legal review stage
└── Stage 3: Final approval (Marketing Mgr)
```

### Custom Forms
- **Brief Form**: Brand, target audience, deliverable types, channels, budget, deadline
- **Asset Form**: File format, dimensions, version, publication channels

### Reporting
- Dashboard: Asset pipeline by status, time-to-completion by asset type, legal review bottleneck analysis
</details>

---

## Scenario 4: Financial Tracking Setup [Advanced]

**Situation:** A consulting firm needs to track project financials in Workfront:
- Consultants bill at different rates based on role
- Projects have fixed-price and time-and-materials components
- Monthly billing records must be generated
- Budget variance must be visible to partners

**Question:** Configure Workfront's financial settings for this use case.

<details>
<summary>Detailed Solution</summary>

### Rate Configuration
- **Role billing rates**: Configure per job role (e.g., Senior Consultant: $250/hr, Analyst: $150/hr)
- **User cost rates**: Configure per user for internal cost tracking
- **Project rate overrides**: For clients with negotiated rates

### Project Financial Settings
- **Revenue Type**: Role Hourly (for T&M work)
- **Cost Type**: User Hourly (for internal cost)
- **Fixed Revenue**: Set on tasks for fixed-price deliverables
- **Performance Index Method**: Cost-Based (for EVM)

### Billing Records Workflow
1. PM creates monthly billing record
2. Selects date range for hours/expenses
3. Reviews included entries
4. Marks record as "Billable"
5. Finance reviews and marks as "Billed"
6. Once billed, entries are locked from editing

### Budget Variance Dashboard
- **Report 1**: Project budget vs actual cost (bar chart)
- **Report 2**: Revenue vs cost by project (profit margin)
- **Report 3**: Utilization by consultant (planned vs actual hours)
- **Report 4**: EVM metrics (CPI, SPI) by project

### Text Mode: Budget Variance Column
```
displayname=Budget Variance %
textmode=true
valueexpression=IF({budget}>0,
  ROUND(({actualCost}-{budget})/{budget}*100,1),
  0)
valueformat=doubleAsPercentRounded
```
</details>

---

## Scenario 5: Fusion Integration Architecture [Architect]

**Situation:** A company uses Workfront, Salesforce, Jira, and Slack. They need:
- When a Salesforce opportunity closes, create a Workfront project
- When a Workfront task status changes, update the linked Jira issue
- Send Slack notifications for overdue tasks
- Sync time entries to a finance system nightly

**Question:** Design the Fusion integration architecture.

<details>
<summary>Detailed Solution</summary>

### Scenario 1: Salesforce → Workfront

```
Trigger: Salesforce Watch Records (Opportunity)
  → Filter: Stage = "Closed Won"
  → Get Salesforce Opportunity Details
  → Create Workfront Project from Template
    (Map: Opportunity Name → Project Name,
     Account → Project Custom Field,
     Amount → Budget)
  → Assign Workfront PM based on Opportunity Owner mapping
  → Send Slack Notification to PM
```

Error Handling: Break (retry on API timeout)

### Scenario 2: Workfront → Jira Sync

```
Trigger: Workfront Event Subscription (Task Updated)
  → Filter: Status field changed
  → Get Workfront Task with custom field "Jira Issue Key"
  → Filter: Jira Issue Key is not blank
  → Map Workfront status → Jira status
  → Update Jira Issue
  → Log sync in Data Store
```

Error Handling: Ignore (non-critical; log failure)

### Scenario 3: Overdue Task Alerts

```
Trigger: Schedule (daily at 9 AM)
  → Search Workfront Tasks
    (Filter: Planned Completion < TODAY,
     Status NOT IN CPL/DED)
  → Iterator (loop through results)
  → Get Assignee Slack ID (Data Store lookup)
  → Send Slack Message to assignee
    "Task '[Name]' in project '[Project]' is overdue by X days"
```

Error Handling: Commit (send notifications for found tasks even if some fail)

### Scenario 4: Nightly Time Sync

```
Trigger: Schedule (daily at 11 PM)
  → Search Workfront Hour entries (last 24 hours)
  → Aggregator (group by user + project)
  → Transform to finance system format
  → HTTP: POST to finance API
  → Log success/failure in Data Store
  → If failure: Send email to admin
```

Error Handling: Rollback (all or nothing for financial data)

### Architecture Diagram
```
┌───────────┐   Webhook    ┌──────────┐   API      ┌──────────┐
│ Salesforce │ ───────────▶ │  Fusion  │ ─────────▶ │ Workfront│
└───────────┘              │          │            └──────────┘
                           │          │   API      ┌──────────┐
                           │          │ ─────────▶ │   Jira   │
                           │          │            └──────────┘
                           │          │   API      ┌──────────┐
                           │          │ ─────────▶ │  Slack   │
                           │          │            └──────────┘
                           │          │   HTTP     ┌──────────┐
                           │          │ ─────────▶ │ Finance  │
                           └──────────┘            └──────────┘
```
</details>

---

*[Back to AD0-E907 Index](../README.md)*
