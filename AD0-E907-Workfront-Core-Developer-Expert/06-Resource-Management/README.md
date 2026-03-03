# Domain 3: Strategic Functionality — Portfolio, Program & Resource Management (~13%)

[Back to AD0-E907 Index](../README.md)

---

## Overview

This domain covers strategic planning tools: Portfolio Optimizer, business cases, resource management, and risk management.

## Key Exam Objectives

- Determine tools for capturing post-project KPIs reflected in the Portfolio Optimizer
- Create views/reports that assess whether a project meets portfolio criteria
- Identify qualitative and quantitative risk management approaches
- Ensure highest-priority projects are set up for appropriate resourcing
- Use Resource Management tools to determine why a user is overallocated
- Identify full staffing mechanisms through Resource Management

---

## Portfolio Optimizer

### Architecture

```
Portfolio Strategy:
├── Portfolio
│   ├── Business Case
│   │   ├── Project Info
│   │   ├── Goals
│   │   ├── Expenses
│   │   ├── Budgets (Resource Planner)
│   │   ├── Risks
│   │   └── Scorecards
│   └── Portfolio Optimizer
│       ├── Scoring
│       ├── Alignment
│       ├── Net Value
│       ├── Risk
│       └── Priority ranking
└── Programs (organizational grouping)
```

### Business Case Components

| Component | Purpose | Data |
|---|---|---|
| **Project Info** | Basic project metadata | Sponsor, dates, benefit |
| **Goals** | Strategic alignment | Alignment scores |
| **Expenses** | Cost estimation | Planned, actual |
| **Resource Budgeting** | Resource costs | Budgeted hours/cost |
| **Risks** | Risk identification | Potential cost, probability |
| **Scorecards** | Custom evaluation criteria | Weighted scoring |
| **Custom Forms** | Additional metadata | Custom data fields |

### Portfolio Optimizer Metrics

```
Key Metrics:
├── Net Value = Planned Benefit - Aligned Cost
├── Risk Value = Sum(Risk Cost × Probability)
├── Alignment Score = Scorecard results
├── ROI = (Planned Benefit - Planned Cost) / Planned Cost × 100
└── Cost Confidence = Budget reliability indicator
```

## Resource Management

### Workload Balancer

```
Workload Balancer Layout:
┌─────────────────────────────────────────────────┐
│ UNASSIGNED WORK                                  │
│ ┌──────────────────────────────────────────────┐ │
│ │ Task A (Role: Developer, 16h)               │ │
│ │ Task B (Team: Design, 24h)                  │ │
│ └──────────────────────────────────────────────┘ │
├─────────────────────────────────────────────────┤
│ ASSIGNED WORK                                    │
│ ┌──────────────────────────────────────────────┐ │
│ │ User 1 (8h/day capacity)                    │ │
│ │   ██████░░ Mon (6h)                         │ │
│ │   ████████████ Tue (12h) ← OVERALLOCATED    │ │
│ │   ████░░░░ Wed (4h)                         │ │
│ │                                              │ │
│ │ User 2 (8h/day capacity)                    │ │
│ │   ████████ Mon (8h)                         │ │
│ │   ████████ Tue (8h)                         │ │
│ └──────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────┘
```

### Identifying Overallocation

```
Overallocation occurs when:
  Assigned Hours > Available Hours (per day/week)

Sources of overallocation:
├── Multiple task assignments on same days
├── Task assignments during time off
├── Role-based assignments resolved to same user
├── Cross-project assignments not coordinated
└── Incorrect schedule assignments

Resolution:
├── Reassign tasks to other resources
├── Adjust task dates/duration
├── Modify task allocations (contour)
├── Use Workload Balancer to rebalance
└── Update resource pools
```

### Resource Planner

| View | Shows | Best For |
|---|---|---|
| **Project** | Resources grouped by project | Project-level planning |
| **Role** | Resources grouped by role | Capacity by skill |
| **User** | Individual user allocation | Person-level detail |

### Key Resource Metrics

| Metric | Formula | Meaning |
|---|---|---|
| **Available Hours** | Schedule hours - Time off | Capacity |
| **Planned Hours** | Sum of task planned hours | Demand |
| **Budgeted Hours** | PM-entered estimate | Budget |
| **Actual Hours** | Logged time entries | Actuals |
| **NET Hours** | Available - Planned (or Budgeted) | Surplus/Deficit |
| **FTE** | Hours ÷ Schedule hours | Full-time equivalent |

### Utilization Report

```
Utilization tracks:
├── Planned Utilization = Planned Hours / Available Hours
├── Actual Utilization = Actual Hours / Available Hours
├── Revenue tracking
│   ├── Planned Revenue
│   ├── Actual Revenue
│   └── Budgeted Revenue
├── Cost tracking
│   ├── Planned Cost
│   ├── Actual Cost
│   └── Budgeted Cost
└── Billing rates
    ├── User billing rate
    ├── Role billing rate
    └── Project override rates
```

## Risk Management

### Qualitative vs Quantitative

| Approach | Method | Output |
|---|---|---|
| **Qualitative** | Expert judgment, categorization | Risk severity ranking |
| **Quantitative** | Probability × Impact calculation | Monetary risk value |

### Workfront Risk Register

```
Risk Entry:
├── Description
├── Potential Cost ($)
├── Probability (%)
├── Risk Value = Potential Cost × Probability
├── Mitigation Plan
└── Status: Open / Mitigated / Closed
```

---

### Practice Scenarios

**Scenario 1:** A PMO needs to decide which 5 of 12 proposed projects to fund. All projects have business cases submitted. How should they use Workfront to make this decision?

<details>
<summary>Answer</summary>

Use the **Portfolio Optimizer**:

1. Ensure all 12 projects have complete business cases with scorecards
2. Navigate to the Portfolio Optimizer
3. Compare projects by: Net Value, Alignment Score, Risk, ROI
4. Use the optimizer's priority slider to model different scenarios
5. Select the top 5 based on highest alignment and net value within budget constraints
6. Set selected projects to "Approved" status
7. Budget resources using Resource Planner for the approved set
</details>

**Scenario 2:** A manager reports a team member is consistently overallocated. How do you diagnose and fix this?

<details>
<summary>Answer</summary>

**Diagnosis using Workload Balancer:**
1. Open Workload Balancer → filter by the user
2. Check assigned work area for daily/weekly hours
3. Identify days where assigned hours exceed capacity (highlighted in red)
4. Check if user has time off scheduled that isn't reflected
5. Check if user's schedule is correctly assigned

**Resolution:**
1. Reassign excess tasks to other team members with capacity
2. Adjust task timing to spread work more evenly
3. Modify daily allocation contours
4. If systemic, review resource pool assignments and role-based planning
</details>

---

*[Back to AD0-E907 Index](../README.md)*
