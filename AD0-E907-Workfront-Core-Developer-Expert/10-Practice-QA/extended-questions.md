# AD0-E907 — Extended Practice Questions (Internet-Sourced)

[Back to AD0-E907 Index](../README.md) | [Back to Practice QA](./README.md)

---

> **Source:** These questions are compiled from publicly available certification preparation resources, Adobe community forums, and official readiness materials. Questions are categorized by domain and tagged by difficulty.

---

## Domain 1: Core System Administration & Setup (17%)

### Q1 [Advanced]
A system administrator needs to ensure that when a project is marked complete, all child tasks and issues are also automatically completed. Which configuration achieves this?

- A) Enable "Auto-Complete" in project preferences
- B) Configure a completion mode of "Automatic" at the project level
- C) Create a Fusion scenario to update child objects
- D) Set up a milestone path that triggers completion

<details><summary>Answer</summary>

**B) Configure a completion mode of "Automatic" at the project level**

When Completion Mode is set to "Automatic," the project status automatically changes to Complete when all tasks and issues are complete, and vice versa — when the project is manually set to Complete, all remaining tasks are marked complete. This is configured in Project Preferences or at the individual project level.
</details>

### Q2 [Advanced]
Which of the following is true about Workfront system statuses? (Select two)

- A) System statuses can be deleted by a system administrator
- B) Custom statuses must equate to one of the system statuses
- C) Group-level statuses override system statuses for that group
- D) System statuses cannot be hidden from the status dropdown
- E) Each custom status can only equate to one system status

<details><summary>Answer</summary>

**B) Custom statuses must equate to one of the system statuses**
**E) Each custom status can only equate to one system status**

Every custom status maps to exactly one system status (e.g., a custom "In Review" maps to "Current"). System statuses themselves cannot be deleted, only hidden. Group-level statuses supplement but don't override system statuses.
</details>

### Q3 [Intermediate]
What is the effect of locking a project preference at the system level?

- A) Group administrators cannot view the preference
- B) Group administrators can view but not override the preference
- C) The preference applies only to new projects
- D) The preference cannot be changed even by system administrators

<details><summary>Answer</summary>

**B) Group administrators can view but not override the preference**

When a project preference is locked at the system level, group administrators can see the setting but cannot change it for their group. When unlocked, group administrators can customize it for their specific group.
</details>

### Q4 [Advanced]
A company is configuring email notifications. Which statement is correct about the notification hierarchy?

- A) Users can enable notifications that are disabled at the system level
- B) Event notifications must be enabled at the system level before users can receive them
- C) Group administrators can override system-level notification settings
- D) Reminder notifications are controlled by individual users

<details><summary>Answer</summary>

**B) Event notifications must be enabled at the system level before users can receive them**

The system administrator enables event notifications globally (Setup > Email > Notifications). Only after they're enabled at the system level can individual users control whether they personally receive them. If disabled at system level, no one receives them regardless of personal preference.
</details>

### Q5 [Advanced]
When configuring a new Workfront instance, which objects should be configured FIRST to establish a foundation?

- A) Project templates, then custom forms
- B) Groups and companies, then access levels, then layout templates
- C) Portfolios and programs, then projects
- D) Reports and dashboards, then notifications

<details><summary>Answer</summary>

**B) Groups and companies, then access levels, then layout templates**

Implementation sequence: Company → Groups/Subgroups → Job Roles → Access Levels → Layout Templates → Custom Forms → Templates → Portfolios/Programs. Groups and access levels are foundational because everything else (statuses, templates, layouts) is assigned at group/access level.
</details>

---

## Domain 2: Intake, Custom Forms & Project Initiation (13%)

### Q6 [Advanced]
A calculated field uses the expression: `IF({DE:Budget}>50000,"High","Standard")`. The field shows "Standard" even when the Budget field contains 75000. What is the likely cause?

- A) The IF syntax is incorrect
- B) The Budget field is stored as text, not a number
- C) The custom form needs to be republished
- D) The calculated field needs to reference the API name, not the display name

<details><summary>Answer</summary>

**B) The Budget field is stored as text, not a number**

When a custom field is defined as text type but contains numeric characters, comparisons treat it as a string (lexicographic comparison), not a number. "75000" < "50000" in string comparison because "7" > "5" is evaluated character by character. The Budget field must be a Number field type for numeric comparison to work correctly.
</details>

### Q7 [Advanced]
Which calculated field expression correctly calculates the number of business days between the planned start date and today?

- A) `DATEDIFF($$TODAY,{plannedStartDate})`
- B) `WEEKDAYDIFF({plannedStartDate},$$TODAY)`
- C) `NETWORKDAYS({plannedStartDate},$$TODAY)`
- D) `WORKDAYDIFF({plannedStartDate},$$TODAY)`

<details><summary>Answer</summary>

**B) `WEEKDAYDIFF({plannedStartDate},$$TODAY)`**

`WEEKDAYDIFF` calculates the number of weekdays (Monday-Friday) between two dates. `DATEDIFF` calculates calendar days (including weekends). `NETWORKDAYS` and `WORKDAYDIFF` are not valid Workfront functions.
</details>

### Q8 [Advanced]
A request queue has three queue topics, each with a different custom form. When a user submits a request, they first select the queue topic, then fill out the form. What happens to the data if the user changes the queue topic after partially filling the first form?

- A) The first form's data is preserved and both forms are attached
- B) The first form is removed and replaced by the new topic's form; entered data is lost
- C) An error prevents changing the queue topic after entering data
- D) The system merges both forms' data

<details><summary>Answer</summary>

**B) The first form is removed and replaced by the new topic's form; entered data is lost**

When a user switches queue topics, the associated custom form changes. Any data entered on the previous form is discarded. This is why it's important to design queue topics carefully and communicate clearly to requestors which topic to select.
</details>

### Q9 [Intermediate]
What is the maximum number of custom forms that can be attached to a single object?

- A) 5
- B) 10
- C) 15
- D) Unlimited

<details><summary>Answer</summary>

**B) 10**

A maximum of 10 custom forms can be attached to a single Workfront object (project, task, issue, etc.). This is a system limit. If you need more fields, consolidate forms or use multi-object forms efficiently.
</details>

---

## Domain 3: Strategic Functionality (13%)

### Q10 [Architect]
A PMO needs to compare 15 proposed projects to determine which ones to fund. Each project has a business case with different ROI projections, risk levels, and strategic alignment scores. Which tool provides the best side-by-side comparison?

- A) Custom project report with matrix grouping
- B) Portfolio Optimizer
- C) Resource Planner in Project view
- D) Enhanced Analytics dashboard

<details><summary>Answer</summary>

**B) Portfolio Optimizer**

The Portfolio Optimizer is specifically designed for this use case. It provides:
- Side-by-side comparison of business case data
- Net Value, Alignment Score, Risk Value, ROI for each project
- Drag-and-drop priority ordering
- "What-if" scenario modeling by adjusting priorities
- Budget constraint visualization
</details>

### Q11 [Advanced]
In the Resource Planner, a user in the "Role" view sees that the "NET" value for the "Developer" role is -40 hours for next month. What does this indicate?

- A) Developers have 40 unused hours available
- B) Developers are overallocated by 40 hours across all projects
- C) 40 hours of developer work is unbudgeted
- D) Developers need to log 40 more hours

<details><summary>Answer</summary>

**B) Developers are overallocated by 40 hours across all projects**

NET = Available Hours - Planned Hours (or Budgeted Hours, depending on the view). A negative NET value means demand exceeds capacity. In this case, the Developer role has 40 more hours of planned work than they have available capacity.
</details>

---

## Domain 4: Document Management & Proofing (13%)

### Q12 [Advanced]
A proof has completed all review stages and the final decision is "Approved with Changes." Which statement is true?

- A) The proof cannot be published until changes are made
- B) The proof status reflects the final decision; no re-review is required
- C) The proof automatically enters a new review stage
- D) All previous proof comments are deleted

<details><summary>Answer</summary>

**B) The proof status reflects the final decision; no re-review is required**

"Approved with Changes" is a final decision that signals the content is acceptable with minor modifications. It does NOT trigger a new review cycle. The original submitter is expected to make the noted changes. If the organization requires re-review after changes, a new proof version should be uploaded.
</details>

### Q13 [Advanced]
What is the difference between a document approval and a proof approval in Workfront?

- A) They are identical features with different names
- B) Document approvals use simple approve/reject; proof approvals use the proof viewer with markup, stages, and formal decisions
- C) Document approvals are for external files; proof approvals are for internal files
- D) Proof approvals require a separate license

<details><summary>Answer</summary>

**B) Document approvals use simple approve/reject; proof approvals use the proof viewer with markup, stages, and formal decisions**

Document approvals are simple binary (approve/reject) decisions on the document page. Proof approvals provide the full proofing experience: inline markup tools, threaded comments, multi-stage workflows, and four decision types (Approved, Approved with Changes, Changes Required, Not Relevant).
</details>

### Q14 [Intermediate]
When connecting AEM Assets Essentials to Workfront, which of the following is a key benefit?

- A) Real-time video editing within Workfront
- B) Linked folders that sync documents between both systems with metadata mapping
- C) Automatic translation of assets
- D) AI-powered content generation

<details><summary>Answer</summary>

**B) Linked folders that sync documents between both systems with metadata mapping**

The AEM Assets Essentials integration provides linked folder synchronization, bi-directional metadata mapping, and seamless asset access from within Workfront. Assets uploaded to linked AEM folders appear in Workfront documents and vice versa.
</details>

---

## Domain 5: Reporting (11%)

### Q15 [Advanced]
A report needs to show projects where ALL tasks are complete, but the project status is still "Current." Which text mode filter achieves this?

- A) Filter by project status = Current AND use a sub-query for tasks
- B) Filter by project status = Current AND project percent complete = 100
- C) Filter by project status = Current AND condition = Complete
- D) This requires a Fusion scenario; it cannot be done in a report

<details><summary>Answer</summary>

**B) Filter by project status = Current AND project percent complete = 100**

When all tasks in a project are complete, the project's percent complete reaches 100%. Filtering for projects where status = "CUR" AND percentComplete = 100 identifies projects that should have been marked complete but weren't. This is a common governance report.

Text mode:
```
status=CUR
status_Mod=eq
percentComplete=100
percentComplete_Mod=eq
```
</details>

### Q16 [Advanced]
What is the purpose of the `$$USER.roleID` wildcard in a report filter?

- A) It filters by the current user's username
- B) It filters by the current user's primary job role
- C) It filters by all roles assigned to the current user
- D) It shows the role name in report columns

<details><summary>Answer</summary>

**B) It filters by the current user's primary job role**

`$$USER.roleID` returns the ID of the logged-in user's primary job role. Use it to create dynamic filters like "Show tasks assigned to my role" — the report adapts to whoever views it, showing role-relevant data without per-user customization.
</details>

### Q17 [Advanced]
How do you display a report column that shows data from a collection (child objects)?

- A) Use a standard column and select the child object
- B) Use text mode with `listmethod=nested(collection).lists` and `type=iterate`
- C) Create a sub-report and embed it
- D) Use a grouping instead of a column

<details><summary>Answer</summary>

**B) Use text mode with `listmethod=nested(collection).lists` and `type=iterate`**

Collection columns iterate through child objects. For example, showing task names on a project report:
```
valuefield=tasks:name
valueformat=HTML
displayname=Task Names
listdelimiter=<br>
listmethod=nested(tasks).lists
type=iterate
```
</details>

---

## Domain 6: Methodology / Best Practices (22%)

### Q18 [Architect]
A company with 200 users across 5 departments wants to implement Workfront. The PMO insists on a "big bang" launch for all departments simultaneously. What is the recommended approach?

- A) Agree with the big bang approach for faster ROI
- B) Recommend a phased rollout starting with the most receptive department
- C) Suggest waiting until all requirements are finalized
- D) Implement only for the IT department first

<details><summary>Answer</summary>

**B) Recommend a phased rollout starting with the most receptive department**

Phased rollout best practices:
1. Start with champion department (most receptive, clearest processes)
2. Validate configurations and gather feedback
3. Iterate on learnings
4. Expand department by department
5. Each phase builds on the previous

Big bang launches carry high risk: configuration errors affect everyone, support overwhelm, change resistance multiplied.
</details>

### Q19 [Architect]
Which financial tracking approach should an Architect recommend for a professional services firm that bills clients both fixed-price and time-and-materials?

- A) Use only Fixed Revenue on all tasks
- B) Use Role Hourly revenue type for T&M tasks and Fixed Revenue for fixed-price deliverables within the same project
- C) Create separate projects for each billing type
- D) Track financials in an external system only

<details><summary>Answer</summary>

**B) Use Role Hourly revenue type for T&M tasks and Fixed Revenue for fixed-price deliverables within the same project**

Workfront supports mixed revenue types at the task level within a single project:
- T&M tasks: Revenue Type = Role Hourly (calculates from logged hours × role billing rate)
- Fixed-price deliverables: Revenue Type = Fixed Revenue (set dollar amount per task)
- Project-level reporting aggregates both types for total revenue tracking
- Billing records group entries for client invoicing
</details>

### Q20 [Advanced]
What is the recommended maximum number of access levels in a Workfront instance?

- A) One per user
- B) As many as needed for each unique permission combination
- C) 6-10 access levels mapped to functional roles
- D) Exactly 5 (one per license type)

<details><summary>Answer</summary>

**C) 6-10 access levels mapped to functional roles**

Best practice is to keep access levels minimal and role-based. Too many access levels (20+) become unmaintainable. Too few (only defaults) don't provide appropriate security. Map to functional roles: System Admin, PMO Director, Project Manager, Team Lead, Team Member, Stakeholder, External.
</details>

### Q21 [Advanced]
A Workfront admin needs to track which users haven't logged in for 30+ days. What is the most efficient approach?

- A) Manually check each user profile
- B) Create a User report filtered by Last Login Date < $$TODAY-30d
- C) Use Enhanced Analytics
- D) Export user data via API and analyze externally

<details><summary>Answer</summary>

**B) Create a User report filtered by Last Login Date < $$TODAY-30d**

A User report with text mode filter:
```
lastLoginDate=$$TODAY-30d
lastLoginDate_Mod=lt
isActive=true
isActive_Mod=eq
```

This identifies inactive users for adoption tracking and license optimization. Schedule this report for automatic weekly delivery to the admin.
</details>

### Q22 [Architect]
During a Workfront implementation, stakeholders disagree on whether to use issue objects or task objects for tracking bug fixes. What guidance should the Architect provide?

- A) Always use tasks because they have more features
- B) Always use issues because they're simpler
- C) Use issues for intake/tracking and convert to tasks when work is planned and assigned
- D) Use a custom object type

<details><summary>Answer</summary>

**C) Use issues for intake/tracking and convert to tasks when work is planned and assigned**

The hybrid approach leverages both object types:
- **Issues:** Lightweight intake, categorization, routing — ideal for bugs reported by users
- **Tasks:** Full project planning, duration types, predecessors — ideal for planned development work
- **Conversion:** When a bug is triaged and planned for a sprint, convert to task in the development project
- The resolving object relationship maintains traceability from original bug report to the fix
</details>

---

## Domain 7: Business Consulting (11%)

### Q23 [Advanced]
A client asks: "Why should we use Workfront instead of just using email and spreadsheets?" Which response best demonstrates business value?

- A) "Workfront is a newer technology than email"
- B) "Workfront provides real-time visibility into work status, eliminates duplicate effort, enables resource optimization, and creates an auditable trail of decisions and approvals"
- C) "Adobe requires it for their ecosystem"
- D) "It's cheaper than email servers"

<details><summary>Answer</summary>

**B) "Workfront provides real-time visibility into work status, eliminates duplicate effort, enables resource optimization, and creates an auditable trail of decisions and approvals"**

Business consulting requires articulating value in business terms:
- **Visibility:** Dashboards replace status meetings; real-time project health
- **Efficiency:** Eliminate email searching, version confusion, duplicate work
- **Resource optimization:** See who's overloaded, who has capacity
- **Governance:** Audit trail, approval history, compliance documentation
- **Scalability:** Spreadsheets break at scale; Workfront grows with the org
</details>

### Q24 [Advanced]
When is it appropriate to recommend Workfront Boards vs traditional project task views?

- A) Boards replace task views entirely
- B) Boards are best for visual workflow management (Kanban) and quick collaboration; task views are best for complex timeline-dependent projects
- C) Boards are only for personal tasks
- D) Task views are deprecated in favor of Boards

<details><summary>Answer</summary>

**B) Boards are best for visual workflow management (Kanban) and quick collaboration; task views are best for complex timeline-dependent projects**

Recommendation matrix:
| Use Boards | Use Task Views |
|---|---|
| Agile/Kanban workflows | Waterfall/Gantt dependencies |
| Quick intake triage | Complex predecessor chains |
| Status-based tracking | Duration/effort planning |
| Team stand-ups | Resource allocation |
| Ad-hoc work management | Milestone tracking |

Connected cards bridge both: Board cards linked to WF tasks sync status bidirectionally.
</details>

### Q25 [Architect]
A client's Workfront instance has grown organically over 3 years. There are 47 custom forms, 23 access levels, and 150+ custom statuses. What should the Architect recommend?

- A) Start fresh with a new instance
- B) Conduct a governance audit: consolidate forms, reduce access levels to 6-10, standardize statuses, and document standards going forward
- C) Leave it as-is to avoid disruption
- D) Migrate to a different tool

<details><summary>Answer</summary>

**B) Conduct a governance audit: consolidate forms, reduce access levels to 6-10, standardize statuses, and document standards going forward**

Technical debt remediation:
1. **Custom Forms (47 → target ~15-20):** Identify duplicate/similar forms, merge using multi-object forms, retire unused forms
2. **Access Levels (23 → target 6-10):** Map to functional roles, consolidate similar levels, use fine-tuning instead of new levels
3. **Statuses (150+ → target ~30-40):** Inventory all, identify duplicates, consolidate per group, document mapping
4. **Going forward:** Governance board approves new custom objects, naming standards, change control process
</details>

---

*Total: 25 additional questions. Combined with the original 18, the AD0-E907 bank now has 43 practice questions.*

*[Back to AD0-E907 Index](../README.md)*
