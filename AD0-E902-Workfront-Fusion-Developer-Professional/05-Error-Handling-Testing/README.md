# Domain 3: Testing, Error Handling & Troubleshooting (~22%)

[Back to AD0-E902 Index](../README.md)

---

## Error Handler Types — Must Memorize

### Complete Comparison Table

| Handler | Behavior | Data State | Use When |
|---|---|---|---|
| **Rollback** | Stops execution, reverts all changes | All operations undone | Data consistency is critical |
| **Commit** | Stops execution, saves completed changes | Partial success kept | Some operations can stand alone |
| **Ignore** | Skips failed module, continues execution | Error skipped, flow continues | Non-critical operations |
| **Break** | Stops execution, queues in Incomplete Executions | Saved for retry | Transient errors (API timeout) |
| **Resume** | Continues from failed module with substitute data | Custom data used | Need to provide default values |

### Error Handler Flow Diagrams

```
ROLLBACK:
  Module A ✓ → Module B ✗ → ROLLBACK
  Result: Module A changes UNDONE, execution STOPS
  Use: Financial transactions, data integrity

COMMIT:
  Module A ✓ → Module B ✗ → COMMIT
  Result: Module A changes KEPT, execution STOPS
  Use: Email sent + database update — email already sent, keep it

IGNORE:
  Module A ✓ → Module B ✗ → IGNORE → Module C ✓
  Result: Module B skipped, C processes normally
  Use: Non-critical logging, optional enrichment

BREAK:
  Module A ✓ → Module B ✗ → BREAK
  Result: Execution queued in Incomplete Executions
  Later: Admin reviews and retries manually
  Use: API rate limits, temporary service outages

RESUME:
  Module A ✓ → Module B ✗ → RESUME (with substitute data)
  Result: Module C receives substitute data instead of B's output
  Use: Provide default values when lookup fails
```

### Adding Error Handlers

```
To add an error handler:
1. Right-click a module
2. Select "Add error handler"
3. Error handler module appears below the main flow
4. Choose handler type (Rollback/Commit/Ignore/Break/Resume)
5. Configure handler-specific settings

Error Handler Scope:
├── Handles errors ONLY from the module it's attached to
├── To handle errors from multiple modules: add handler to each
├── Or: wrap modules in a "Route" and add handler to the route
└── Error handlers can contain additional modules
    (e.g., send notification before breaking)
```

### Error Handler with Notification Pattern

```
Module (might fail)
  │
  ├── Success path → continues normally
  │
  └── Error handler:
      Send Slack Message ("Scenario X failed: {{error.message}}")
        → Break (queue for retry)
        
This ensures you're notified AND the failed execution is saved.
```

## Incomplete Executions

```
Incomplete Executions Queue:
├── Created when Break error handler fires
├── Stored for manual or automatic retry
├── Contains: full execution data, error details, timestamp
├── Options:
│   ├── Retry: re-run from failure point
│   ├── Edit: modify data before retry
│   ├── Delete: discard failed execution
│   └── Auto-retry: configure automatic retry schedule
├── Max storage: configurable (default 100)
├── When full: oldest incomplete executions dropped
└── Access: Scenario → Incomplete Executions tab
```

### Auto-Retry Configuration

```
Settings → Incomplete Executions:
├── Allow storing incomplete executions: Yes/No
├── Sequential processing: process in order
├── Max number of consecutive errors: stop scenario after N
└── Auto-complete: automatically retry after specified interval
```

## Testing Strategies

### Run Once

```
Run Once:
├── Executes scenario exactly ONE time
├── Does NOT follow schedule
├── Returns execution details immediately
├── Shows: input/output for each module
├── Use for: initial testing, debugging
└── Access: Click "Run Once" button at bottom

Best Practice Testing Flow:
1. Design scenario
2. Run Once with test data
3. Inspect each module's input/output
4. Fix mapping errors
5. Run Once again
6. Verify all paths (use different test data)
7. Enable scheduling
```

### Execution History

```
Execution History:
├── Shows all past executions
├── Details per execution:
│   ├── Timestamp
│   ├── Duration
│   ├── Operations consumed
│   ├── Data transferred
│   ├── Status (Success/Warning/Error)
│   └── Per-module details (click to inspect)
├── Filtering: by status, date range
└── Retention: based on subscription tier
```

### DevTool (Browser Extension)

```
Fusion DevTool Features:
├── Scenario Dump: export full scenario JSON
├── Stream: real-time execution monitoring
├── Module Inspection: detailed I/O per module
├── History: execution timeline
├── Tools:
│   ├── Copy module
│   ├── Search in scenario
│   └── Performance analysis
└── Debugging:
    ├── Set breakpoints (via Run Once + inspect)
    ├── View raw HTTP requests/responses
    └── Analyze data transformation at each step
```

## Common Error Patterns & Solutions

| Error | Cause | Solution |
|---|---|---|
| **ConnectionError** | API unreachable | Check URL, network; use Break handler |
| **RuntimeError** | Module logic failure | Check mapping, data types |
| **DataError** | Invalid data format | Add validation, use ifempty() |
| **RateLimitError** | Too many API calls | Add Sleep module, reduce frequency |
| **InvalidAccessTokenError** | Auth expired | Refresh connection, re-authorize |
| **OperationError** | Target system rejected | Check required fields, permissions |
| **MaxOperationsExceeded** | Hit plan operation limit | Optimize scenario, reduce bundles |
| **MaxResultsExceeded** | Too many search results | Add filters, use pagination |
| **BundleSizeLimitExceeded** | Data too large | Reduce payload, paginate |

## Defensive Scenario Design

```
Best Practices:
├── ALWAYS add error handlers to critical modules
├── Use ifempty() for optional fields
│   └── ifempty(1.email; "no-reply@company.com")
├── Validate data before processing
│   └── Router filter: length(1.name) > 0
├── Set max cycles to prevent runaway executions
├── Configure max consecutive errors
├── Use Data Store for deduplication
├── Add logging/notification on errors
└── Test all router paths, not just happy path
```

---

### Practice Scenarios

**Scenario 1:** A Fusion scenario creates records in 3 external systems sequentially. If the second system fails, the first record was already created. What error handling strategy should be used?

<details><summary>Answer</summary>

**Commit** on the second module.

Reasoning:
- System 1 record already created (cannot undo via Fusion)
- Rollback would mark execution as failed but System 1 record still exists
- Commit acknowledges partial success and stops further execution
- Add error handler chain: Send notification → Commit
- The notification alerts admins to manually clean up System 1 if needed

If System 1 supports deletion, you could use:
- Custom error handler: Delete System 1 record → Break → Queue for full retry
</details>

**Scenario 2:** A scenario processes webhook events but sometimes the external API returns 429 (rate limited). What's the best approach?

<details><summary>Answer</summary>

**Break** error handler + **Sleep** module.

1. Add **Break** error handler on the API module
   - Failed execution queued in Incomplete Executions
   - Auto-retry enabled (retry after 5 minutes)

2. Add **Sleep** module (1-2 seconds) before the API call
   - Prevents hitting rate limits in the first place

3. Configure scenario **max operations** to stay within API limits

4. Consider switching from webhook to polling trigger
   - Polling gives you control over execution frequency
   - Can process in controlled batches
</details>

---

*[Back to AD0-E902 Index](../README.md)*
