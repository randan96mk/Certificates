# AD0-E907 — Practice Q&A Bank

[Back to AD0-E907 Index](../README.md)

---

## Instructions

- Questions are tagged by domain and difficulty
- Use `<details>` blocks to hide answers (click to reveal)
- Difficulty: `[Basic]` `[Intermediate]` `[Advanced]` `[Architect]`

---

## Section 1: Core System Administration and Setup

### Q1 [Intermediate]
How can a system administrator effectively share permissions at the group level while maintaining security?

- a) Grant all users system admin access
- b) Use access levels with fine-tuned permissions and group-level sharing
- c) Create individual sharing rules for each user
- d) Remove all permission restrictions

<details><summary>Answer</summary>

**b) Use access levels with fine-tuned permissions and group-level sharing**

Access levels set the maximum possible permissions, while group-level sharing provides appropriate access to objects within the group. This follows the principle of least privilege.
</details>

### Q2 [Advanced]
When should a group status be used instead of a system status?

- a) When the status needs to apply to all projects in the system
- b) When a specific department needs a custom workflow that doesn't apply to other groups
- c) When you want to replace a system default status
- d) When the status needs to be visible to external users

<details><summary>Answer</summary>

**b) When a specific department needs a custom workflow that doesn't apply to other groups**

Group statuses allow departments to have unique workflow states without affecting the entire system. They equate to a system status but are only available to that group's projects.
</details>

### Q3 [Advanced]
What are the best practices for Kickstart data imports? (Select two)

- a) Import directly into production for fastest results
- b) Always export existing data first to get the correct template format
- c) Test imports in a sandbox environment before production
- d) Skip validation to speed up the import process

<details><summary>Answer</summary>

**b) Always export existing data first to get the correct template format**
**c) Test imports in a sandbox environment before production**

Kickstarts require specific column formats. Exporting first gives you the correct template. Testing in sandbox prevents data corruption in production.
</details>

### Q4 [Intermediate]
How do Groups and Teams within a user's profile affect what they can see or do in Workfront?

- a) They have no effect on visibility
- b) Home Group determines layout template and status access; Teams determine shared work queues
- c) Teams determine access levels; Groups determine notifications
- d) Both Groups and Teams only affect reporting

<details><summary>Answer</summary>

**b) Home Group determines layout template and status access; Teams determine shared work queues**

Home Group controls: layout template priority, available statuses, group-level features. Teams control: shared work assignments, team calendar, Workload Balancer visibility.
</details>

---

## Section 2: Intake, Custom Forms, and Project Initiation

### Q5 [Advanced]
A calculated field on a multi-object custom form (Project + Task) displays "WHOOPS!" when viewed on tasks. The formula is `{portfolio}.{name}`. What's the issue and fix?

- a) The portfolio field doesn't exist; create it
- b) Tasks don't have a direct portfolio relationship; use `{project}.{portfolio}.{name}`
- c) The valueformat is incorrect
- d) The form needs to be republished

<details><summary>Answer</summary>

**b) Tasks don't have a direct portfolio relationship; use `{project}.{portfolio}.{name}`**

On a task, you must traverse through the project to reach the portfolio. Additionally, wrap in `IF(ISBLANK(...))` to handle cases where the project isn't in a portfolio.
</details>

### Q6 [Advanced]
How do you control form access, visibility, and dependencies across multiple forms attached to the same object?

- a) Only one form can be attached at a time
- b) Use section break permissions for access control, display logic for field visibility, and calculated fields can reference other attached forms
- c) Separate forms cannot interact with each other
- d) Use layout templates to control form visibility

<details><summary>Answer</summary>

**b) Use section break permissions for access control, display logic for field visibility, and calculated fields can reference other attached forms**

Multiple forms can be attached to one object. Calculated fields can reference `{DE:Field Name}` from any attached form. Section breaks control who sees which sections. Display logic controls which fields appear.
</details>

---

## Section 3: Strategic Functionality

### Q7 [Architect]
A PMO wants to capture post-project KPIs that feed into the Portfolio Optimizer for future project scoring. What tools should they use?

- a) Custom reports only
- b) Business Case with scorecards, plus custom forms for KPI tracking
- c) External spreadsheets imported via Fusion
- d) Enhanced Analytics dashboards

<details><summary>Answer</summary>

**b) Business Case with scorecards, plus custom forms for KPI tracking**

Scorecards in the Business Case directly feed the Portfolio Optimizer's alignment scores. Custom forms on projects can track KPIs. The Optimizer uses Business Case data (net value, alignment, risk) for project comparison and prioritization.
</details>

### Q8 [Advanced]
Using Resource Management tools, how would you determine why a specific user is consistently overallocated?

- a) Check their timesheet submissions
- b) Use the Workload Balancer filtered to the user; check daily allocation across all projects
- c) Run a project status report
- d) Check the user's access level settings

<details><summary>Answer</summary>

**b) Use the Workload Balancer filtered to the user; check daily allocation across all projects**

The Workload Balancer shows assigned hours vs available hours per day. Filter by the specific user to see all their assignments across projects. Red highlighting indicates overallocation. Also check: schedule assignment, time off, and cross-project tasks.
</details>

---

## Section 4: Document Management and Proofing

### Q9 [Intermediate]
How can a user quickly identify the latest version of a document in Workfront?

- a) Filter by upload date
- b) Check the "Latest Version" label
- c) Review proofing history
- d) Access the archived folder

<details><summary>Answer</summary>

**b) Check the "Latest Version" label**

Workfront automatically labels the most recent version of a document as "Latest Version". This label moves automatically when a new version is uploaded.
</details>

### Q10 [Advanced]
What distinguishes a proofing use case from a document management use case?

- a) Proofing is for PDFs only; document management is for all file types
- b) Proofing provides structured review with inline markup and multi-stage approval workflows; document management handles storage, versioning, and simple approvals
- c) They are identical features
- d) Document management requires Desktop Viewer; proofing uses Web Viewer

<details><summary>Answer</summary>

**b) Proofing provides structured review with inline markup and multi-stage approval workflows; document management handles storage, versioning, and simple approvals**

Key differences: Proofing has proof roles, stage-based workflows, inline markup tools, version comparison, and formal decision types. Document management provides upload/download, versioning, and simple approve/reject.
</details>

### Q11 [Advanced]
A company needs to connect AEM as a Cloud Service with Workfront for asset management. What integration capabilities are available?

- a) Only manual upload/download
- b) Native integration with linked folders, metadata sync, and workflow triggers
- c) Fusion-only integration
- d) No native integration exists

<details><summary>Answer</summary>

**b) Native integration with linked folders, metadata sync, and workflow triggers**

The AEM + Workfront native integration provides: linked folders (AEM folder ↔ Workfront project), automatic document sync, bi-directional metadata mapping, and workflow triggers when proof decisions are made or asset status changes.
</details>

---

## Section 5: Reporting

### Q12 [Advanced]
How do you combine multiple data columns into a single column in a Workfront report?

- a) Use the standard column picker
- b) Use text mode with the `sharecol=true` parameter to merge columns
- c) Create a calculated custom field
- d) Use groupings instead of columns

<details><summary>Answer</summary>

**b) Use text mode with the `sharecol=true` parameter to merge columns**

Set `sharecol=true` on the first column and configure the second column with `displayname=` (empty) and `width=0`. Both values appear in the same visual column.
</details>

### Q13 [Advanced]
What is the benefit of using wildcard values like `$$USER` and `$$TODAY` in report filters?

- a) They make reports faster
- b) They create dynamic, personalized reports that adapt to the current user and date
- c) They are required for all filters
- d) They only work in text mode

<details><summary>Answer</summary>

**b) They create dynamic, personalized reports that adapt to the current user and date**

`$$USER` filters results to the logged-in user (e.g., "my tasks"). `$$TODAY` creates dynamic date filters (e.g., "overdue tasks"). These make reports reusable across users without per-user customization.
</details>

### Q14 [Intermediate]
How does a rich text field render differently in a list view vs a detail view?

- a) No difference
- b) List view shows HTML-stripped plain text; detail view shows fully formatted HTML
- c) List view shows full formatting; detail view shows plain text
- d) Both show plain text

<details><summary>Answer</summary>

**b) List view shows HTML-stripped plain text; detail view shows fully formatted HTML**

In list/report views, rich text fields have HTML tags stripped for compactness. In the object detail view, the full HTML formatting (bold, links, lists, etc.) is rendered. This is an important distinction for report design.
</details>

---

## Section 6: Methodology / Best Practices / Use Cases

### Q15 [Architect]
What governance framework should you recommend when expanding a Workfront instance from one department to three?

- a) Give all new users system admin access for flexibility
- b) Create a tiered governance model with system admins, group admins, power users, and end users with documented naming conventions and change control processes
- c) Keep everything centralized under one admin
- d) Let each department manage independently without coordination

<details><summary>Answer</summary>

**b) Create a tiered governance model with system admins, group admins, power users, and end users with documented naming conventions and change control processes**

Governance scaling requires: group admin delegation, standardized naming, template libraries per department, layout templates per audience, change advisory board, and documented escalation paths.
</details>

### Q16 [Architect]
A client needs to set up deliverable tracking within a single campaign that spans multiple teams. Which approach is most effective?

- a) One task list with all deliverables
- b) Project from template with phased task groups, milestone path, cross-team assignments, and custom forms for deliverable metadata
- c) A spreadsheet shared via Workfront documents
- d) Individual projects per team with no linkage

<details><summary>Answer</summary>

**b) Project from template with phased task groups, milestone path, cross-team assignments, and custom forms for deliverable metadata**

A single project with structured phases enables cross-team visibility, milestone-based progress tracking, integrated approval workflows, and unified reporting. Custom forms capture deliverable-specific data.
</details>

---

## Section 7: Business Consulting

### Q17 [Architect]
Which configuration settings are most difficult to change once implemented in Workfront? (Select two)

- a) Custom form field names
- b) Company structure and group hierarchy
- c) Dashboard layouts
- d) Object naming conventions established across hundreds of projects

<details><summary>Answer</summary>

**b) Company structure and group hierarchy** — Restructuring groups affects: status availability, layout templates, approval processes, sharing, and reporting. Many objects reference groups.

**d) Object naming conventions established across hundreds of projects** — Retroactively renaming hundreds of projects, templates, and reports is extremely labor-intensive and affects cross-references.

These are "foundational" decisions that should be planned carefully before implementation.
</details>

### Q18 [Advanced]
What strategies help reduce user resistance during a Workfront implementation? (Select two)

- a) Involve stakeholders early in the design process
- b) Force immediate adoption with strict deadlines
- c) Communicate benefits clearly and frequently
- d) Disable advanced features to simplify the experience

<details><summary>Answer</summary>

**a) Involve stakeholders early in the design process**
**c) Communicate benefits clearly and frequently**

Change management principles: early involvement creates ownership, clear communication of "what's in it for me" drives adoption. Forcing adoption or disabling features breeds resentment.
</details>

---

*Total: 18 questions covering all 7 domains. [Back to AD0-E907 Index](../README.md)*
