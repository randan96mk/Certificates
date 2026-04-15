# AD0-E902 — Scenario-Based Questions

[Back to AD0-E902 Index](../README.md)

---

## Scenario 1: Multi-System Onboarding Automation [Advanced]

**Situation:** HR uses Workfront for onboarding projects. When a new hire's onboarding project reaches "Current" status, the following must happen automatically:
- Create a Google Workspace account
- Create a Jira account
- Send a welcome Slack message to the team channel
- Add the new hire to a shared Google Sheet tracker
- If any step fails, the others should still complete

**Question:** Design the Fusion scenario with appropriate error handling.

<details>
<summary>Detailed Solution</summary>

```
Trigger: Workfront Watch Events
  Filter: objectType = Project, status changed to "Current"
  Additional filter: Template = "Onboarding"
  │
  ▼
Get Project Details (Workfront: Read a Record)
  → Extract: new hire name, email, department, start date
  │
  ▼
Router (4 parallel paths, each with IGNORE error handler):
  │
  ├── Path A: Google Workspace
  │   → HTTP: POST to Google Admin API (create user)
  │   → Error handler: IGNORE + log to Data Store
  │
  ├── Path B: Jira
  │   → HTTP: POST to Jira REST API (create user)
  │   → Error handler: IGNORE + log to Data Store
  │
  ├── Path C: Slack
  │   → Slack: Send Message to #new-hires channel
  │   → Error handler: IGNORE (non-critical)
  │
  └── Path D: Google Sheets
      → Google Sheets: Add Row (name, email, dept, date)
      → Error handler: IGNORE + log to Data Store

After all paths complete:
  │
  ▼
Data Store: Search Records (failed = true from this execution)
  → If any failures: Send summary email to IT admin
    "Onboarding for {name}: X of 4 systems provisioned. 
     Failed: {list of failed systems}. Manual action needed."
```

**Why IGNORE on each path:**
- Each system is independent — failure in Jira shouldn't block Slack
- All paths execute regardless of individual failures
- Summary notification ensures nothing falls through cracks
</details>

---

## Scenario 2: Real-Time Invoice Processing [Advanced]

**Situation:** An accounting system sends invoice data via webhook. Each invoice has line items (1-50 items). For each line item:
- Look up the product in Workfront by SKU
- If product exists, update the Workfront project financial data
- If product doesn't exist, create it first, then update
- After all line items are processed, send a summary to the finance team

**Question:** Design the scenario handling the array processing and conditional logic.

<details>
<summary>Detailed Solution</summary>

```
Trigger: Custom Webhook (receives invoice JSON)
  │
  ▼
Webhook Response (200 OK — respond immediately)
  │
  ▼
Iterator (split invoice.lineItems array)
  │ Outputs: sku, quantity, unitPrice, description per item
  │
  ▼
Workfront: Search Records
  Filter: {DE:SKU} = {{iterator.sku}}
  │
  ▼
Router:
  ├── Filter: length(searchResults) > 0 (product exists)
  │   → Workfront: Update Record
  │     Update project expense with new line item
  │
  └── Filter: length(searchResults) = 0 (product not found)
      → Workfront: Create Record (new project for product)
        Name: {{iterator.description}}
        Custom Form: SKU = {{iterator.sku}}
      → Workfront: Update Record
        Add expense to newly created project
  │
  ▼ (both paths merge)
Array Aggregator (source: Iterator)
  Collect: sku, status ("updated" or "created"), amount
  │
  ▼
Text Aggregator
  Template: "{{item.sku}}: {{item.status}} (${{item.amount}})"
  Separator: "\n"
  │
  ▼
Send Email to Finance Team
  Subject: "Invoice {{webhook.invoiceNumber}} Processed"
  Body: Summary of all line items + total amount
```
</details>

---

## Scenario 3: Bidirectional Workfront-Salesforce Sync [Architect]

**Situation:** Projects in Workfront map to Opportunities in Salesforce. Requirements:
- When a WF project status changes, update the linked SF opportunity
- When a SF opportunity stage changes, update the linked WF project
- Prevent infinite loops (WF updates SF → SF webhook fires → updates WF → WF event fires → ...)
- Handle cases where the linked record doesn't exist

**Question:** Design the architecture to prevent infinite sync loops.

<details>
<summary>Detailed Solution</summary>

**Anti-Loop Strategy: "Source Tracking" via Data Store**

```
Data Store: "sync-lock"
  Key: "{system}-{recordID}"
  Value: { lockedUntil: timestamp, source: "WF" or "SF" }

Scenario 1: Workfront → Salesforce
────────────────────────────────
Watch WF Events (status changed)
  │
  ▼
Data Store: Get "SF-{linked_sf_id}" 
  │
  ▼
Router:
  ├── Lock exists AND lockedUntil > now
  │   → SKIP (this change was triggered by SF sync, not user)
  │
  └── No lock OR lock expired
      → Data Store: Add "WF-{wf_project_id}" 
        (lock for 30 seconds, source = "WF")
      → HTTP: Update Salesforce Opportunity
      → Error handler: Break

Scenario 2: Salesforce → Workfront
────────────────────────────────
Webhook (SF outbound message)
  │
  ▼
Data Store: Get "WF-{linked_wf_id}"
  │
  ▼
Router:
  ├── Lock exists AND lockedUntil > now
  │   → SKIP (this change was triggered by WF sync, not user)
  │
  └── No lock OR lock expired
      → Data Store: Add "SF-{sf_opp_id}" 
        (lock for 30 seconds, source = "SF")
      → Workfront: Update Project status
      → Error handler: Break
```

**How it prevents loops:**
1. WF user changes status → Scenario 1 fires
2. Scenario 1 sets lock on WF record, updates SF
3. SF fires webhook → Scenario 2 fires
4. Scenario 2 checks: WF lock exists (set by Scenario 1) → SKIP
5. Loop prevented!

**Lock expiry (30 seconds) ensures:**
- If a genuine SF user changes the opportunity 31+ seconds later, it syncs normally
- Locks don't accumulate forever
</details>

---

## Scenario 4: Data Migration with Progress Tracking [Advanced]

**Situation:** Migrate 5,000 records from a legacy system to Workfront. Requirements:
- Process in batches of 100
- Track progress (how many processed, how many failed)
- Resume from where it left off if the scenario fails
- Generate a completion report

**Question:** Design the migration scenario.

<details>
<summary>Detailed Solution</summary>

```
Data Store: "migration-state"
  Key: "current-offset"
  Value: { offset: 0, processed: 0, failed: 0, startedAt: timestamp }

Scenario:
─────────
Schedule: Every 5 minutes (runs until migration complete)
  │
  ▼
Data Store: Get "current-offset"
  │
  ▼
Router:
  ├── offset >= 5000 → Migration complete!
  │   → Send completion email with stats
  │   → Deactivate scenario
  │
  └── offset < 5000 → Continue migration
      │
      ▼
      HTTP: GET legacy API (offset={{state.offset}}, limit=100)
        │
        ▼
      Iterator (split 100 records)
        │
        ▼
      Workfront: Create Record
        → Error handler: 
          IGNORE + increment failed counter
        │
        ▼
      Numeric Aggregator (COUNT successful creates)
        │
        ▼
      Data Store: Update "current-offset"
        offset = old.offset + 100
        processed = old.processed + successCount
        failed = old.failed + failCount
        │
        ▼
      (scenario ends, runs again in 5 minutes)

After 50 runs (5000 / 100), migration completes.
If scenario fails mid-batch, Data Store has last offset.
On restart, resumes from correct position.
```
</details>

---

*[Back to AD0-E902 Index](../README.md)*
