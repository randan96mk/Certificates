# AD0-E902 — Practice Q&A Bank

[Back to AD0-E902 Index](../README.md)

---

## Domain 1: Core Technical Concepts (26%)

### Q1 [Intermediate]
Which function chain correctly converts "may 15, 2026" to "2026-05-15"?

- a) `formatDate("may 15, 2026"; "YYYY-MM-DD")`
- b) `formatDate(parseDate("may 15, 2026"; "MMM DD, YYYY"); "YYYY-MM-DD")`
- c) `parseDate("may 15, 2026"; "YYYY-MM-DD")`
- d) `toString(parseDate("may 15, 2026"))`

<details><summary>Answer</summary>

**b) `formatDate(parseDate("may 15, 2026"; "MMM DD, YYYY"); "YYYY-MM-DD")`**

You must first `parseDate` (convert text to date object using the INPUT format), then `formatDate` (convert date object to text using the OUTPUT format). This is a nested/chained function call.
</details>

### Q2 [Advanced]
What is the difference between a Data Store and a variable in Fusion?

- a) Variables persist across executions; data stores are in-memory only
- b) Data stores persist across executions and scenarios; variables exist only within a single execution
- c) They are identical features
- d) Data stores can only hold numbers; variables hold any type

<details><summary>Answer</summary>

**b) Data stores persist across executions and scenarios; variables exist only within a single execution**

Data stores are permanent, database-like storage shared across scenarios. Variables (Set Variable / Get Variable) only exist during the current execution and are lost when the scenario finishes.
</details>

### Q3 [Intermediate]
Which function extracts all "name" values from an array of objects?

- a) `get(array; "name")`
- b) `map(array; "name")`
- c) `pick(array; "name")`
- d) `first(array).name`

<details><summary>Answer</summary>

**b) `map(array; "name")`**

The `map()` function extracts a specific field from each item in an array of objects, returning an array of values. Example: `map([{name:"A"},{name:"B"}]; "name")` → `["A", "B"]`
</details>

### Q4 [Intermediate]
How do you provide a default value when a mapped field might be empty?

- a) `default(1.email; "none")`
- b) `ifempty(1.email; "no-reply@company.com")`
- c) `if(1.email; 1.email; "none")`
- d) `coalesce(1.email; "none")`

<details><summary>Answer</summary>

**b) `ifempty(1.email; "no-reply@company.com")`**

`ifempty(value; default)` returns the default value if the first value is empty, null, or undefined. This is the standard Fusion pattern for fallback values.
</details>

---

## Domain 2: Scenario Design & Architecture (32%)

### Q5 [Advanced]
What happens when a Router has two paths and BOTH filters match?

- a) Only the first matching path executes
- b) Both paths execute (all matching paths run)
- c) An error is thrown
- d) The fallback path executes instead

<details><summary>Answer</summary>

**b) Both paths execute (all matching paths run)**

Unlike a switch statement, a Fusion Router executes ALL paths whose filter conditions are met. To get exclusive routing, use mutually exclusive filter conditions.
</details>

### Q6 [Advanced]
An Iterator splits an array of 5 items. Each item goes through 3 action modules. After the Iterator block, an Aggregator combines results. How many total operations are consumed?

- a) 5
- b) 8
- c) 15 (Iterator) + 1 (Aggregator) = 16
- d) 5 (Iterator only counts once)

<details><summary>Answer</summary>

**c) 15 (Iterator) + 1 (Aggregator) = 16**

5 items × 3 modules = 15 operations, plus 1 aggregator operation = 16 total. Each bundle flowing through each module counts as one operation. The Iterator itself produces 5 bundles.
</details>

### Q7 [Advanced]
You need a scenario to run Monday-Friday at 9 AM only. Which scheduling approach is correct?

- a) "At Regular Intervals" set to every 24 hours
- b) "Days of the Week" with Mon-Fri selected, time set to 09:00
- c) "Every Day" at 09:00 with a Router filter for weekdays
- d) Advanced CRON: `0 9 * * 1-5`

<details><summary>Answer</summary>

**b) "Days of the Week" with Mon-Fri selected, time set to 09:00**

This is the most direct configuration. While CRON (d) would also work, the "Days of the Week" option is the built-in UI approach that most directly solves this.
</details>

### Q8 [Advanced]
What is the correct order of operations in a Fusion execution with multiple bundles?

- a) All bundles through Module 1, then all through Module 2, etc.
- b) Bundle 1 through all modules, then Bundle 2 through all modules, etc.
- c) Random order based on processing speed
- d) Parallel processing of all bundles through all modules simultaneously

<details><summary>Answer</summary>

**b) Bundle 1 through all modules, then Bundle 2 through all modules, etc.**

Fusion processes bundles sequentially — each bundle completes the entire scenario path before the next bundle starts. This is important for understanding data availability and error impact.
</details>

### Q9 [Advanced]
A scenario needs to process an array field within each bundle from a trigger that returns multiple bundles. The trigger returns 3 records, each with an array of 5 tasks. How do you process all 15 tasks individually?

- a) Use a single Iterator on the trigger output
- b) The trigger automatically flattens nested arrays
- c) Use an Iterator after the trigger; it will process each bundle's array separately
- d) Use a Repeater with count = 15

<details><summary>Answer</summary>

**c) Use an Iterator after the trigger; it will process each bundle's array separately**

The Iterator processes the array within each bundle. Bundle 1 (5 tasks) → Iterator creates 5 sub-bundles. Then Bundle 2 (5 tasks) → 5 more. Bundle 3 → 5 more. Total: 15 individual task bundles processed.
</details>

---

## Domain 3: Error Handling & Testing (22%)

### Q10 [Advanced]
A scenario sends an email (Module A), then updates a database (Module B). Module B fails. The email was already sent. Which error handler preserves the sent email and stops execution?

- a) Rollback
- b) Commit
- c) Ignore
- d) Break

<details><summary>Answer</summary>

**b) Commit**

Commit stops execution but keeps all completed operations (the sent email). Rollback would attempt to undo everything (but can't unsend an email). Ignore would continue to the next module. Break would queue for retry (which might resend the email).
</details>

### Q11 [Intermediate]
What happens when a Break error handler fires?

- a) The scenario continues with the next bundle
- b) The execution is stopped and saved to the Incomplete Executions queue for later retry
- c) All previous operations are rolled back
- d) The scenario switches to a fallback path

<details><summary>Answer</summary>

**b) The execution is stopped and saved to the Incomplete Executions queue for later retry**

Break is specifically designed for transient errors — it preserves the execution state so it can be retried later, either manually or via auto-retry configuration.
</details>

### Q12 [Intermediate]
Which error handler should be used when an optional enrichment module fails but the rest of the scenario should continue?

- a) Rollback
- b) Commit
- c) Ignore
- d) Break

<details><summary>Answer</summary>

**c) Ignore**

Ignore skips the failed module and continues execution with subsequent modules. The downstream modules won't receive data from the failed module, but the scenario proceeds. Ideal for non-critical operations.
</details>

### Q13 [Advanced]
How should you test ALL paths of a Router?

- a) Run Once is sufficient — it tests all paths automatically
- b) Run Once with different test data for each path, verifying each filter condition is triggered
- c) Routers can't be tested, only verified in production
- d) Use the DevTool to simulate path execution

<details><summary>Answer</summary>

**b) Run Once with different test data for each path, verifying each filter condition is triggered**

You need to test each filter condition independently. Run the scenario multiple times with data that matches each path's filter. Also test the fallback path with data that matches no filters.
</details>

---

## Domain 4: APIs, Connectors & Webhooks (20%)

### Q14 [Intermediate]
What module should you add immediately after a Custom Webhook trigger when the external system requires a response within 5 seconds but your scenario takes 30 seconds?

- a) Sleep (5 seconds)
- b) Webhook Response (return 200 immediately)
- c) HTTP: Make a Request (callback)
- d) Router with timeout filter

<details><summary>Answer</summary>

**b) Webhook Response (return 200 immediately)**

Place a Webhook Response module right after the trigger to send an immediate 200 OK response. The scenario continues processing asynchronously after the response is sent. The external system won't time out.
</details>

### Q15 [Advanced]
An API uses OAuth 2.0 (Client Credentials). Tokens expire after 1 hour. How does Fusion handle token refresh?

- a) You must manually refresh tokens every hour
- b) Fusion automatically refreshes tokens when an OAuth 2.0 connection is configured
- c) Create a separate scenario to refresh tokens on a schedule
- d) Tokens never expire in Fusion connections

<details><summary>Answer</summary>

**b) Fusion automatically refreshes tokens when an OAuth 2.0 connection is configured**

When using the HTTP OAuth 2.0 module or an app connector with OAuth, Fusion manages token lifecycle automatically — requesting new tokens when the current one expires.
</details>

### Q16 [Advanced]
When should you use the HTTP module instead of a pre-built app connector?

- a) Always — HTTP is more reliable
- b) When the app connector doesn't support the specific endpoint you need, or when no connector exists
- c) Never — always use app connectors
- d) Only for GET requests

<details><summary>Answer</summary>

**b) When the app connector doesn't support the specific endpoint you need, or when no connector exists**

App connectors provide convenience (auto-discovered fields, managed auth), but they may not cover every API endpoint. The HTTP module gives full API access for custom endpoints, unsupported operations, or APIs without a Fusion connector.
</details>

### Q17 [Intermediate]
A Workfront connector module returns a 429 status code. What does this mean?

- a) Record not found
- b) Authentication failed
- c) Rate limit exceeded — too many API requests
- d) Internal server error

<details><summary>Answer</summary>

**c) Rate limit exceeded — too many API requests**

HTTP 429 means the API's rate limit has been exceeded. Solution: add a Sleep module between requests, reduce polling frequency, or use Break error handler with auto-retry.
</details>

---

## Cross-Domain Scenarios

### Q18 [Architect]
Design a complete scenario architecture for syncing Workfront projects with Jira, including error handling, deduplication, and bidirectional status updates.

<details><summary>Answer</summary>

**Architecture: Two scenarios (one per direction)**

**Scenario 1: Workfront → Jira**
```
Watch WF Events (Project status changed)
  → Data Store: Check if already synced (dedup by event ID)
  → Router:
    ├── Already processed → Ignore
    └── New event:
        Get WF Project Details
        → Data Store: Get Jira Key (lookup by WF ID)
        → Router:
          ├── Jira Key exists → HTTP: Update Jira Issue
          └── Jira Key missing → HTTP: Create Jira Issue
              → Data Store: Store mapping (WF ID ↔ Jira Key)
        → Error Handler: Break (queue for retry)
        → Data Store: Mark event as processed
```

**Scenario 2: Jira → Workfront**
```
Webhook (Jira event)
  → Webhook Response (200 OK immediately)
  → Data Store: Check dedup
  → Data Store: Get WF Project ID (lookup by Jira Key)
  → Router:
    ├── WF ID exists → Update WF Project status
    └── WF ID missing → Log warning (orphaned Jira issue)
  → Error Handler: Break + Slack notification
```

**Data Stores:**
- `project-mapping`: key = WF Project ID, value = Jira Issue Key
- `processed-events`: key = event ID, value = timestamp
</details>

---

*Total: 18 questions covering all 4 domains. [Back to AD0-E902 Index](../README.md)*
