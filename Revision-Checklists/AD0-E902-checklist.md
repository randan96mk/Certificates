# AD0-E902 — Last-Week Revision Checklist

[Back to Main README](../README.md)

---

## Exam Day Quick Facts
- **51 questions** | **102 minutes** | **64.7% passing (33/51)** | ~2 min/question

---

## Domain 1: Foundational Technical Concepts (39%) — HEAVIEST

### Module Types & Triggers
- [ ] 6 module types: Trigger, Action, Search, Aggregator, Iterator, Transformer
- [ ] Polling triggers (Watch) vs Instant triggers (Webhooks) — when to use each
- [ ] Webhook: no operations consumed, real-time, unique URL per instance
- [ ] "Choose where to start" option on first run of polling trigger

### Bundle Processing
- [ ] Bundle = one unit of data; 3 bundles × 5 modules = 15 operations
- [ ] Sequential: Bundle 1 through ALL modules, THEN Bundle 2, etc.
- [ ] NOT all bundles through module 1, then module 2

### Data Mapping
- [ ] Direct: `{{1.name}}`, Nested: `{{1.user.email}}`, Array: `{{1.items[1].id}}`
- [ ] Function wrapping: `{{upper(1.name)}}`
- [ ] Conditional: `{{if(1.status = "CPL"; "Done"; "Open")}}`
- [ ] Nested functions: `{{formatDate(addDays(now; 7); "YYYY-MM-DD")}}`

### Functions — Must Memorize
- [ ] **String:** `lower`, `upper`, `length`, `substring`, `trim`, `replace`, `contains`, `split`, `join`
- [ ] **Math:** `round`, `floor`, `ceil`, `min`, `max`, `sum`, `average`, `abs`
- [ ] **Date:** `now`, `formatDate`, `parseDate`, `addDays`, `addMonths`, `dateDifference`
- [ ] **Array:** `length`, `first`, `last`, `merge`, `contains`, `map`, `sort`, `flatten`, `deduplicate`
- [ ] **General:** `if`, `ifempty`, `switch`, `omit`, `pick`, `get`, `typeof`, `toNumber`, `toString`

### Data Stores
- [ ] Persistent across executions AND scenarios
- [ ] Operations: Add, Update, Get, Search, Delete, Check Existence
- [ ] Use cases: lookup tables, deduplication, state tracking, caching
- [ ] Data Store vs Variable: persistent vs execution-only

---

## Domain 2: Scenario Design & Architecture (35%)

### Flow Control
- [ ] **Router:** ALL matching paths execute (not just first)
- [ ] **Router fallback:** runs when NO other path matches
- [ ] **Iterator:** splits array into individual bundles; outputs value, index, totalBundles
- [ ] **Aggregator:** combines bundles back into one; MUST pair with source module
- [ ] **Aggregator types:** Array, Text, Numeric (SUM/AVG/COUNT/MIN/MAX), Table
- [ ] **Repeater:** execute N times (pagination, retry, test data)
- [ ] **Sleep:** max 300 seconds; rate limit compliance

### Scheduling
- [ ] Execution vs Cycle vs Operation terminology
- [ ] Max cycles: how many times trigger fetches per execution
- [ ] Schedule types: intervals, once, daily, weekly, CRON

### Data Structures
- [ ] Define schemas for webhooks, data stores, HTTP bodies
- [ ] Fields: text, number, date, boolean, collection, array

### Design Patterns
- [ ] Lookup Enrichment (trigger → get details → act)
- [ ] Conditional Routing (Router + filters)
- [ ] Batch Processing (Iterator → Action → Aggregator)
- [ ] Pagination (Repeater → HTTP with offset)
- [ ] Deduplication (Data Store check before processing)
- [ ] Two-Phase Sync (collect → push separately)

---

## Domain 3: Testing & Error Handling (16%)

### Error Handlers — MEMORIZE ALL 5
- [ ] **Rollback:** Stop + undo all changes (data integrity)
- [ ] **Commit:** Stop + keep completed changes (partial success OK)
- [ ] **Ignore:** Skip failed module, continue scenario (non-critical)
- [ ] **Break:** Stop + queue in Incomplete Executions (transient errors)
- [ ] **Resume:** Continue with substitute data (provide defaults)

### Incomplete Executions
- [ ] Created by Break handler
- [ ] Options: retry, edit, delete, auto-retry
- [ ] Max storage configurable; oldest dropped when full

### Testing
- [ ] Run Once: single execution, inspect each module I/O
- [ ] Test ALL router paths with different test data
- [ ] Execution History: timestamp, operations, status, per-module details
- [ ] DevTool: scenario dump, stream, module inspection

### Common Errors
- [ ] 429 = Rate Limited → Sleep + Break handler
- [ ] ConnectionError → Break handler (transient)
- [ ] DataError → ifempty() + validation

---

## Domain 4: Working with APIs (10%)

### HTTP Module
- [ ] Variants: Make a Request, Basic Auth, OAuth 2.0, API Key, Client Cert
- [ ] Response fields: data, headers, statusCode, body
- [ ] Parse Response = Yes for auto JSON parsing

### Webhooks
- [ ] Custom Webhook: generates unique URL, instant trigger
- [ ] Webhook Response: send HTTP response back to caller
- [ ] Use Webhook Response immediately for long-running scenarios

### JSON
- [ ] Parse JSON: string → structured data (needs Data Structure)
- [ ] Create JSON: mapped fields → JSON string
- [ ] Access: `{{1.data.user.name}}`, `{{1.data.items[1].id}}`

### Authentication
- [ ] OAuth 2.0 auto-refreshes tokens
- [ ] Connections: reuse across scenarios, one per service per env
- [ ] App Connector vs HTTP Module: convenience vs flexibility

---

## Key Formulas to Memorize

```
Date chain: formatDate(parseDate("input"; "inputFormat"); "outputFormat")
Add days:   addDays(now; 7)
Default:    ifempty(1.email; "fallback@company.com")
Condition:  if(1.status = "CPL"; "Done"; "Open")
Array map:  map(1.items; "name")  → extracts all "name" values
Dedup:      deduplicate(1.array)
Count:      length(1.array)
Concat:     join(["a";"b"]; ", ")  → "a, b"
```

## Common Trap Answers

| Trap | Reality |
|---|---|
| "Router executes only the first matching path" | ALL matching paths execute |
| "Variables persist across executions" | Only within a single execution |
| "Rollback undoes external API calls" | It marks failure; can't undo external side effects |
| "Polling triggers are real-time" | Webhooks are real-time; polling runs on schedule |
| "Aggregator can work without a source module" | MUST pair with a source (Iterator, etc.) |

---

*Confidence Check: Mark each item. Anything unchecked = study priority.*
