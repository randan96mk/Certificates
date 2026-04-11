# AD0-E907 — Last-Week Revision Checklist

[Back to Main README](../README.md)

---

## Exam Day Quick Facts
- **53 questions** | **106 minutes** | **66% passing (35/53)** | ~2 min/question

---

## Domain 1: Core System Administration & Setup (17%)

- [ ] Permission sharing models (access levels + object sharing)
- [ ] Group status vs system status — when to use each
- [ ] Kickstart data imports/exports — best practices
- [ ] Auto-provisioning users (SSO, SCIM)
- [ ] Groups vs Teams — impact on visibility and access
- [ ] Boards configuration (connected cards, columns, WIP limits)
- [ ] Group administration capabilities and limitations

## Domain 2: Intake, Custom Forms, Project Initiation (13%)

- [ ] Request queue architecture (queue topics, topic groups, routing)
- [ ] Custom form calculated fields — common expressions (IF, CONCAT, DATEDIFF, WEEKDAYDIFF)
- [ ] Display logic vs skip logic
- [ ] Multi-object custom forms — field compatibility across objects
- [ ] Value passing: issue → project/task conversion
- [ ] Section break permissions
- [ ] Template design best practices

## Domain 3: Strategic — Portfolio, Program, Resource Mgmt (13%)

- [ ] Portfolio Optimizer and Business Case components
- [ ] Scorecard-based project evaluation
- [ ] Qualitative vs quantitative risk management
- [ ] Workload Balancer: identifying and resolving overallocation
- [ ] Resource Planner: Project/Role/User views
- [ ] Available vs Planned vs Budgeted vs Actual hours
- [ ] Utilization reporting

## Domain 4: Document Management & Proofing (13%)

- [ ] Document management vs proofing — key differences
- [ ] Proof roles: Read Only, Reviewer, Approver, R&A, Author, Moderator
- [ ] Proof decisions: Approved, Approved with Changes, Changes Required
- [ ] Automated proof workflows (stages, activation, locking)
- [ ] "Latest Version" label concept
- [ ] External document integrations (AEM, Google Drive, SharePoint)
- [ ] AEM as a Cloud Service + AEM Assets Essentials connection

## Domain 5: Reporting (11%)

- [ ] Text mode syntax: valuefield, valueexpression, valueformat
- [ ] Wildcards: $$USER, $$TODAY, $$NOW and their use in filters
- [ ] Shared columns (sharecol=true)
- [ ] Collection references for parent-child reporting
- [ ] Conditional formatting (styledef)
- [ ] Rich text field rendering: list view vs detail view
- [ ] Report types and chart options

## Domain 6: Methodology / Best Practices (22%) — HIGHEST

- [ ] Campaign tracking setup within a single project
- [ ] Work prioritization and strategic justification
- [ ] Financial management: cost types, revenue types, billing records
- [ ] Approval workflow design (single-use, global, group-level)
- [ ] Governance framework for scaling (tiered: sys admin → group admin → power user)
- [ ] Expansion considerations for additional teams
- [ ] Workfront Boards (kanban, connected cards, workstreams)

## Domain 7: Business Consulting (11%)

- [ ] Cross-functional implementation planning
- [ ] Adapting workflows for business changes (test environment first!)
- [ ] Configuration settings that are hard to reverse
- [ ] Change management strategies (involve stakeholders, communicate benefits)
- [ ] Multi-team efficiency recommendations

---

## Key Formulas to Memorize

```
EVM:
  CPI = BCWP / ACWP (>1 = under budget)
  SPI = BCWP / BCWS (>1 = ahead of schedule)
  EAC = BAC / CPI

Text Mode:
  $$USER.ID, $$USER.homeGroupID
  $$TODAY, $$NOW
  DATEDIFF(), WEEKDAYDIFF(), ADDDAYS()
  IF(), CONCAT(), IN(), ISBLANK()
  sharecol=true, styledef
```

## Common Trap Answers

| Trap | Reality |
|---|---|
| "Give everyone admin access" | Always wrong — violates least privilege |
| "Install directly in production" | Always wrong — test in sandbox/staging first |
| "Create per-user configurations" | Usually wrong — use groups/roles |
| "Force immediate adoption" | Usually wrong — use change management |
| "Single massive template" | Usually wrong — keep templates focused |

---

## NEW: 2025-2026 Features to Know

- [ ] **Workfront Planning:** Record types, workspaces, connections, planning requests
- [ ] **AI Assistant:** Formula generation, item location, document summarization
- [ ] **Unified Review & Approval:** Frame.io integration for video review
- [ ] **Canvas Dashboards (Beta):** Flexible visualization builder
- [ ] **Fusion Chain Scenarios:** Parent-child scenario reuse
- [ ] **Legacy deprecations:** G Suite, Jira, Salesforce integrations retired Feb 2026
- [ ] **API v21:** Breaking changes to Event Subscriptions
- [ ] **Rich Text Fields:** Replacing Text with Formatting (Q2 2026)
- [ ] **Multi-select External Lookup:** New custom form field type
- [ ] **Proctoring change:** Meazure Learning / ProctorU with Guardian browser

---

*Confidence Check: Mark each item. Anything unchecked = study priority.*
