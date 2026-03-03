# Dashboards & Analytics

[Back to Domain: Reporting](./README.md) | [Back to AD0-E907 Index](../README.md)

---

## Dashboards

### Dashboard Components

```
Dashboard:
├── Reports (up to 25 per dashboard)
│   ├── Standard reports
│   ├── Matrix reports
│   └── Chart-only reports
├── External Pages
│   ├── Embedded URLs (iframes)
│   ├── Workfront Canvas Analytics
│   └── Third-party dashboards
├── Calendar Reports
│   └── Date-based visual views
└── Layout
    ├── 1, 2, or 3 column layout
    └── Drag-and-drop ordering
```

### Dashboard Best Practices

| Practice | Description |
|---|---|
| **Purpose-driven** | Each dashboard serves one audience/goal |
| **7 ± 2 reports** | Don't overload; keep it scannable |
| **Use prompts** | Add filters the user can control |
| **Chart + detail** | Pair chart reports with detail list reports |
| **External pages** | Embed Canvas Analytics for deeper visuals |
| **Naming convention** | `[Audience]-[Purpose]-Dashboard` |

### Dashboard Sharing

```
Sharing Options:
├── Share with specific users
├── Share with teams
├── Share with roles
├── Share system-wide
├── Share with external users (view-only link)
└── Set as Landing Page (via layout template)
```

### External Pages in Dashboards

- Embedded via iframe URL
- Must be HTTPS
- Can embed: Workfront Analytics, third-party tools, custom web apps
- Cannot embed: Sites that block iframe embedding (X-Frame-Options)

## Enhanced Analytics

### Available Visualizations

| Visualization | What It Shows | Use Case |
|---|---|---|
| **Burndown** | Planned vs actual completion over time | Track sprint/project progress |
| **Tasks in Flight** | Tasks by status across projects | Identify bottlenecks |
| **Project Activity** | Timeline of project events | See patterns in activity |
| **Resource Capacity** | Team members' capacity vs allocation | Identify overallocation |
| **Team Capacity** | Aggregate team workload | Workforce planning |
| **Project Treemap** | Visual size comparison of projects | Portfolio overview |
| **Activity by Team** | Work distribution across teams | Balance workload |
| **Flight Plan** | Task status visualization | Delivery health check |

### Enhanced Analytics Filters

- Date range selection
- Project condition
- Project status
- Team / Group
- Portfolio / Program

### Analytics vs Reports: When to Use What

| Need | Use Reports | Use Analytics |
|---|---|---|
| Specific data points | Yes | No |
| Trend visualization | Limited (charts) | Yes |
| Resource capacity view | Manual (text mode) | Built-in |
| Custom calculations | Yes (text mode) | No |
| Cross-project patterns | Grouped reports | Yes |
| Share with stakeholders | Dashboard | Limited |
| Real-time data | Yes | Near real-time |

## User Adoption Reporting

### Key Adoption Metrics

```
Adoption Reports:
├── Login Frequency
│   ├── Report type: User
│   ├── Key field: Last Login Date
│   └── Filter: Last login > 30 days ago (inactive users)
├── Task Completion Rate
│   ├── Report type: Task
│   ├── Grouping: Assigned To
│   └── Metric: % completed on time
├── Hour Logging Compliance
│   ├── Report type: Hour
│   ├── Grouping: User, Week
│   └── Metric: Total hours vs expected
├── Proof Review Timeliness
│   ├── Report type: Proof Approval
│   └── Metric: Time to first decision
└── Request Queue Usage
    ├── Report type: Issue
    ├── Filter: Request Queue source
    └── Metric: Requests submitted per week/team
```

### Field Type Rendering in Reports

| Field Type | List View | Detail View | Export |
|---|---|---|---|
| **Plain Text** | Truncated at column width | Full text | Full text |
| **Rich Text** | HTML stripped; plain text shown | Full HTML rendering | HTML tags included |
| **Multi-Select** | Comma-separated values | All selected values | Comma-separated |
| **Image** | Thumbnail/link | Displayed | File reference |
| **Calculated** | Computed value | Computed value | Computed value |
| **Date** | Formatted date | Formatted date | Date format |
| **Number** | Formatted number | Formatted number | Raw number |

> **Exam Tip:** Know how rich text fields render differently in list views (plain text) vs detail views (formatted HTML). This is a commonly tested distinction.

---

### Practice Scenarios

**Scenario 1:** An executive wants a single dashboard showing: portfolio health, team workload, and overdue tasks across all active projects. Design the dashboard.

<details>
<summary>Answer</summary>

**Dashboard: "Executive Overview Dashboard"** (3-column layout)

**Column 1: Portfolio Health**
- Report 1: Project Status chart (pie chart by condition: On Target / At Risk / In Trouble)
- Report 2: Project list filtered to Active, sorted by planned completion date

**Column 2: Team Workload**
- Report 3: External page embedding Enhanced Analytics — Resource Capacity view
- Report 4: Hour report grouped by user, showing planned vs actual hours

**Column 3: Risk & Overdue**
- Report 5: Task report filtered to overdue (planned completion < today, status ≠ complete), grouped by project
- Report 6: Issue report showing unresolved issues > 7 days old

**Prompts:** Add project portfolio prompt so executive can drill into specific portfolios.
</details>

---

*[← Report Builder & Text Mode](./report-builder-text-mode.md) | [Back to Domain: Reporting](./README.md)*
