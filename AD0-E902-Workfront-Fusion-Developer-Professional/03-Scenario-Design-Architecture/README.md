# Domain 2: Scenario Design & Architecture (35% — ~18 questions)

[Back to AD0-E902 Index](../README.md)

---

## Scenario Structure

```
Complete Scenario:
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│ Trigger   │──▶│ Action 1  │──▶│ Router    │──▶│ Action 2  │
│ (Watch/   │   │ (Get/     │   │ (Filter   │   │ (Create/  │
│  Webhook) │   │  Search)  │   │  paths)   │   │  Update)  │
└──────────┘    └──────────┘    └────┬──────┘    └──────────┘
                                     │
                                ┌────▼──────┐    ┌──────────┐
                                │ Path B    │──▶│ Action 3  │
                                │ (Filter)  │   │ (Delete/  │
                                └───────────┘   │  Notify)  │
                                                └──────────┘
```

## Flow Control Modules

### Router (Parallel Paths)

```
Router Architecture:
                    ┌── Filter: status = "Approved" ──▶ Path A (Create in AEM)
                    │
Input ──▶ Router ──┤── Filter: status = "Rejected" ──▶ Path B (Send rejection email)
                    │
                    └── Fallback (no filter) ──▶ Path C (Log to data store)

Key Rules:
├── All matching paths execute (not just the first)
├── Paths with filters: evaluated in order
├── Fallback path: runs when NO other path matches
├── Each path is independent (own error handling)
└── Bundles flow through ALL matching paths
```

### Iterator (Split Arrays)

```
Iterator Use Case:
  Input: { projectName: "Campaign", tasks: ["Design", "Build", "Test"] }
  
  Iterator on "tasks" field:
  ├── Bundle 1: "Design"   (index: 1, totalBundles: 3)
  ├── Bundle 2: "Build"    (index: 2, totalBundles: 3)
  └── Bundle 3: "Test"     (index: 3, totalBundles: 3)

  Each item becomes a separate bundle for downstream processing.
  Iterator outputs: value, index (1-based), totalNumberOfBundles
```

### Aggregator (Combine Bundles)

```
Aggregator Types:
├── Array Aggregator
│   ├── Collects values from multiple bundles into one array
│   ├── Configure: Source module, Target structure
│   └── Output: Single bundle with array
├── Text Aggregator
│   ├── Concatenates text from multiple bundles
│   ├── Configure: Separator character
│   └── Output: Single bundle with combined text
├── Numeric Aggregator
│   ├── Calculates: SUM, AVG, COUNT, MIN, MAX
│   ├── Configure: Field to aggregate, function
│   └── Output: Single bundle with computed value
└── Table Aggregator
    ├── Creates CSV/table from bundles
    └── Output: Single bundle with table structure

CRITICAL: Aggregators MUST pair with a source module.
  Iterator → [processing] → Aggregator = back to single bundle
```

### Iterator + Aggregator Pattern

```
Common Pattern: Transform each item in an array

Input: Array of 5 tasks
  │
  ▼
Iterator (splits into 5 bundles)
  │
  ▼
Action Module (process each task individually)
  │
  ▼
Array Aggregator (combines results back into one array)
  │
  ▼
Next Module (receives single bundle with transformed array)
```

### Repeater

```
Repeater:
├── Executes subsequent modules N times
├── Configure: Initial value, repeats count, step
├── Use cases:
│   ├── Pagination (repeat API calls with offset)
│   ├── Retry logic (attempt N times)
│   └── Generate test data
└── Output: i (iteration number), starts at initial value
```

### Sleep

```
Sleep Module:
├── Pauses execution for specified duration
├── Max: 300 seconds (5 minutes)
├── Use cases:
│   ├── Rate limit compliance (wait between API calls)
│   ├── Allow external processing to complete
│   └── Stagger bulk operations
└── Place between action modules that need delay
```

## Scheduling & Execution

### Schedule Options

```
Scheduling Types:
├── At Regular Intervals
│   ├── Every N minutes (15 min minimum for most)
│   ├── Every N hours
│   └── Every N days
├── Once
│   └── Run one time immediately
├── Every Day
│   └── At specific time(s)
├── Days of the Week
│   └── Specific days + times
└── Advanced (CRON)
    └── Full cron expression support

Execution Settings:
├── Max cycles: bundles per execution (default: 1)
│   └── 1 cycle = process one set of trigger results
│   └── >1 cycle = keep fetching until empty or limit hit
├── Max operations: total operations per execution
├── Sequential processing: process bundles one at a time
└── Max errors: stop after N errors in execution
```

### Execution vs Cycle vs Operation

```
Terminology:
├── Execution: One complete run of a scenario
│   └── Triggered by schedule or webhook
├── Cycle: One iteration within an execution
│   └── Multiple cycles if "max cycles" > 1
├── Operation: One module processing one bundle
│   └── 3 bundles × 5 modules = 15 operations
└── Data Transfer: Volume of data processed
    └── Measured in bytes
```

## Data Structures

```
Data Structure = Schema definition for complex data

Use Cases:
├── Define webhook payload format
├── Structure data store records
├── Parse complex JSON responses
├── Define HTTP request body format
└── Create consistent data interfaces between modules

Example: "Project Record" Data Structure
├── projectId (text, required)
├── projectName (text, required)  
├── status (text, enum: Active/Complete/On Hold)
├── budget (number)
├── startDate (date)
├── assignee (collection)
│   ├── name (text)
│   └── email (text)
└── tasks (array of collections)
    ├── taskName (text)
    ├── dueDate (date)
    └── priority (number)
```

## Scenario Design Patterns

### Pattern 1: Lookup Enrichment

```
Trigger → Get Additional Data → Combine → Action

Example:
Watch WF Tasks → Get Project Details → Get User Email →
  Create Enriched Record in External System

Why: Trigger often returns limited fields; 
     enrich with additional API calls before acting.
```

### Pattern 2: Conditional Routing

```
Trigger → Router
  ├── Path A (condition met): Primary action
  ├── Path B (alternative): Secondary action
  └── Fallback: Error logging / notification

Example:
Watch WF Issues → Router
  ├── Priority = Urgent → Create Jira Ticket + Slack Alert
  ├── Priority = Normal → Create Jira Ticket only
  └── Fallback → Log to Data Store for manual review
```

### Pattern 3: Batch Processing

```
Trigger (returns many) → Iterator → Action → Aggregator → Summary

Example:
Search WF Projects (100 results) →
  Iterator (100 bundles) →
  Get Budget Details per project →
  Numeric Aggregator (SUM budgets) →
  Send Summary Email ("Total portfolio: $2.5M")
```

### Pattern 4: Pagination

```
Repeater (N iterations) → API Call (with offset) → Iterator → Action

Example:
Repeater (10 iterations) →
  HTTP: Get Records (offset = (i-1) * 100, limit = 100) →
  Iterator (process each record) →
  Create in Target System

Handles APIs that return max 100 records per call.
```

### Pattern 5: Deduplication

```
Trigger → Data Store: Check Exists → Router
  ├── Exists: Skip (already processed)
  └── Not Exists: Process + Store ID

Example:
Watch WF Documents →
  Data Store: Get Record (key = documentID) →
  Router:
    ├── Record exists: Ignore (duplicate)
    └── Record not found: 
        Process document → 
        Data Store: Add Record (key = documentID)
```

### Pattern 6: Two-Phase Sync

```
Phase 1: Collect (scheduled daily)
  Search Source System → Store in Data Store

Phase 2: Push (scheduled hourly)
  Search Data Store (unsynced) → Push to Target →
  Update Data Store (mark synced)

Why: Decouples collection from pushing, handles rate limits.
```

## Scenario Organization Best Practices

```
Organization:
├── Naming: "[System]-[Action]-[Trigger]"
│   └── "WF-to-Jira-Task-Sync"
│   └── "Slack-Alert-Overdue-Tasks"
├── Folders: Group by integration or department
├── Notes: Add notes to modules explaining logic
├── Versioning: Scenario history tracks all changes
├── Testing: Use "Run Once" before scheduling
└── Monitoring: Set up error notifications

Blueprint Management:
├── Export scenarios as blueprints (.json)
├── Import to other teams/organizations
├── Version control blueprints in Git
└── Use for standard integration patterns
```

---

### Practice Scenarios

**Scenario 1:** Design a scenario that watches a Workfront request queue, enriches the request with project data, routes based on request type, and creates records in different external systems.

<details><summary>Answer</summary>

```
Watch WF Issues (Request Queue filter)
  │
  ▼
Get WF Project Details (from issue's project ID)
  │
  ▼
Get Custom Form Data (DE:Request Type field)
  │
  ▼
Router:
  ├── Filter: Request Type = "IT Hardware"
  │   → Create Jira Ticket (IT Project)
  │   → Send Slack to #it-requests
  │
  ├── Filter: Request Type = "Software License"
  │   → Create Record in SAM tool
  │   → Send Email to Procurement
  │
  └── Fallback:
      → Data Store: Add unrouted request
      → Send Slack to #admin: "Unrouted request"
```
</details>

**Scenario 2:** You need to sync 10,000 records from an external API that returns max 200 per call. Design the pagination strategy.

<details><summary>Answer</summary>

```
Repeater (initial: 0, repeats: 50, step: 1)
  │
  ▼
HTTP: Make a Request
  URL: https://api.example.com/records
  Query: offset={{(2.i) * 200}}&limit=200
  │
  ▼
Router:
  ├── Filter: length(3.body.data) > 0
  │   → Iterator (split response array)
  │     → Create WF Record per item
  │     → Sleep (1 second — rate limit)
  │
  └── Filter: length(3.body.data) = 0
      → (empty response = done, path ends)

Total: up to 50 × 200 = 10,000 records
Stop early when response returns empty array.
```
</details>

---

*[Back to AD0-E902 Index](../README.md)*
