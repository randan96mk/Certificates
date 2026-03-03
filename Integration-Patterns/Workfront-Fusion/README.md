# Workfront + Fusion Integration Patterns

[Back to Integration Patterns](../README.md) | [Back to Main README](../../README.md)

---

## Fusion Architecture for Workfront

### Core Integration Patterns

```
Fusion + Workfront Capabilities:
├── Watch/Trigger Modules
│   ├── Watch Records (polling)
│   ├── Watch Events (webhook-based)
│   └── Download Document
├── Action Modules
│   ├── Create/Read/Update/Delete Record
│   ├── Search Records
│   ├── Upload Document
│   ├── Convert Object (Issue → Task/Project)
│   └── Custom API Call
└── Utility Modules
    ├── List Projects/Tasks/Issues
    ├── Get Object Details
    └── Miscellaneous Actions
```

## Key Automation Patterns

### Pattern 1: Project Provisioning

```
Trigger: Workfront Event (Project created from template)
  → Get Project Details
  → Router:
    ├── Path A: Create Slack Channel
    │   └── Invite team members
    ├── Path B: Create Confluence Space
    │   └── Copy template pages
    ├── Path C: Create Jira Project
    │   └── Link to Workfront project (custom field)
    └── Path D: Send Welcome Email
        └── Include project brief & links
```

### Pattern 2: Approval Escalation

```
Trigger: Schedule (every 4 hours)
  → Search: Pending Approvals > 48 hours old
  → Iterator: For each stale approval
    → Get Approver Details
    → Get Approver's Manager
    → Send Escalation Email to Manager
    → Add Update Note to Workfront object
    → If > 72 hours: Auto-delegate to Manager
```

### Pattern 3: Time Entry Validation

```
Trigger: Schedule (Monday morning)
  → Search: Users with < 32 hours logged last week
  → Filter: Exclude PTO/Holiday users
  → Iterator: For each under-logger
    → Send Reminder Email
    → Create Issue: "Missing Time Entries"
    → Assign to User
    → If 2nd consecutive week: Notify Manager
```

### Pattern 4: Cross-System Status Sync

```
Trigger: Workfront Watch Events (Project status changed)
  → Get Project Details + Custom Fields
  → Router:
    ├── If status = "Current":
    │   → Update Salesforce Opportunity stage
    │   → Send Slack notification
    ├── If status = "Complete":
    │   → Close Jira tickets
    │   → Generate completion report
    │   → Send to stakeholders
    └── If status = "On Hold":
        → Pause Jira sprint
        → Notify team in Slack
```

### Pattern 5: Document Routing to AEM

```
Trigger: Workfront Watch Events (Document uploaded)
  → Get Document Details
  → Get Project Custom Form data
  → Router (based on document type/metadata):
    ├── If type = "Final Approved Asset":
    │   → Upload to AEM DAM (via AEM API)
    │   → Set AEM metadata from WF custom fields
    │   → Trigger AEM publishing workflow
    ├── If type = "Contract":
    │   → Upload to SharePoint (legal folder)
    │   → Set permissions
    └── Default:
        → Upload to Google Drive (archive)
```

## Error Handling Patterns

### Resilient Scenario Design

```
Best Practice Error Handling:
┌─────────────┐
│   Trigger    │
└──────┬──────┘
       │
┌──────▼──────┐     ┌──────────────┐
│  API Call    │──▶  │ Error Handler │
│  (Action)   │     │              │
└──────┬──────┘     │ ├── Break    │ → Queue for retry (API timeout)
       │            │ ├── Ignore   │ → Skip and continue (non-critical)
       │            │ ├── Rollback │ → Undo all (data integrity)
       │            │ └── Commit   │ → Save partial (mixed criticality)
       │            └──────────────┘
┌──────▼──────┐
│  Next Step  │
└─────────────┘

Pattern: API Retry with Break
  Action Module → Error: Break
    → Incomplete execution stored
    → Admin reviews and retries manually
    → Or: configure automatic retry schedule
```

## Data Store Patterns

```
Data Stores in Fusion:
├── Lookup Tables
│   ├── User mapping: Workfront ID → Slack ID
│   ├── Project mapping: WF Project → Jira Key
│   └── Status mapping: WF Status → External Status
├── State Tracking
│   ├── Last sync timestamp per system
│   ├── Error counts per scenario
│   └── Rate limit remaining
└── Caching
    ├── API response cache
    ├── Frequently accessed reference data
    └── Avoid redundant API calls
```

---

*[Back to Integration Patterns](../README.md)*
