# Domain 1: Fusion Core Technical Concepts (~26%)

[Back to AD0-E902 Index](../README.md)

---

## Fusion Architecture Overview

```
Fusion Organization:
├── Organization
│   └── Teams
│       └── Scenarios
│           ├── Modules (building blocks)
│           ├── Connections (auth to apps)
│           ├── Data Stores (persistent storage)
│           ├── Data Structures (schemas)
│           └── Webhooks (instant triggers)
├── Templates (reusable scenario blueprints)
├── Connections (centralized credential store)
└── Keys (encryption keys)
```

## Module Types

| Module Type | Icon | Purpose | Example |
|---|---|---|---|
| **Trigger** | Lightning bolt | Starts a scenario | Watch Records, Webhook |
| **Action** | Square | Performs a CRUD operation | Create Record, Send Email |
| **Search** | Magnifying glass | Finds records | Search Records, List Items |
| **Aggregator** | Circle with arrow | Combines bundles into one | Array Aggregator, Text Aggregator |
| **Iterator** | Unfold icon | Splits array into individual bundles | Iterator |
| **Transformer** | Wrench | Converts data format | Parse JSON, Compose a String |

## Trigger Types — Critical Distinction

### Polling Triggers (Watch Modules)

```
How Polling Works:
  Scenario scheduled to run every 15 minutes
  ├── Fusion calls API: "Give me records since last check"
  ├── Returns 0 to N records
  ├── Each record becomes a separate bundle
  ├── Scenario processes each bundle sequentially
  └── Stores watermark (last processed ID/timestamp)

Characteristics:
  ├── Scheduled execution (configurable interval)
  ├── Uses API calls (consumes operations)
  ├── Can return multiple bundles per execution
  ├── Processes batches (configurable limit)
  └── Has "Choose where to start" on first run
```

### Instant Triggers (Webhooks)

```
How Webhooks Work:
  External system sends HTTP POST to Fusion URL
  ├── Fusion receives data immediately
  ├── Scenario executes instantly
  ├── One execution per webhook call
  └── No polling = no API calls consumed for trigger

Characteristics:
  ├── Real-time execution (no delay)
  ├── No operations consumed for trigger
  ├── One bundle per execution (usually)
  ├── Requires external system to support webhooks
  └── Fusion provides unique URL per webhook
```

### When to Use Which

| Use Polling When | Use Webhook When |
|---|---|
| External system has no webhook support | External system supports webhooks |
| Need to process historical/backlog data | Need real-time processing |
| Batch processing is acceptable | Immediate response required |
| Simple setup needed | Low latency critical |
| "Choose where to start" needed | Want to save operations |

## Bundles — The Core Data Unit

```
Bundle = One unit of data flowing through a scenario

Example: Watch Workfront Tasks returns 3 tasks
  → Bundle 1: { id: 101, name: "Design", status: "INP" }
  → Bundle 2: { id: 102, name: "Build", status: "NEW" }
  → Bundle 3: { id: 103, name: "Test", status: "INP" }

Each bundle flows through ALL subsequent modules independently.
If there are 3 bundles and 5 modules, that's 15 module executions.
```

### Bundle Processing Order

```
┌─────────┐   Bundle 1   ┌─────────┐   Bundle 1   ┌─────────┐
│ Trigger  │ ───────────▶ │ Action  │ ───────────▶ │ Action  │
│          │   Bundle 2   │         │   Bundle 2   │         │
│ 3 bundles│ ───────────▶ │         │ ───────────▶ │         │
│          │   Bundle 3   │         │   Bundle 3   │         │
│          │ ───────────▶ │         │ ───────────▶ │         │
└─────────┘              └─────────┘              └─────────┘

Each bundle goes through the ENTIRE scenario before the next starts.
NOT: all bundles through module 1, then all through module 2.
```

## Data Mapping

### Mapping Panel

```
Mapping Sources:
├── Previous module output (click to map)
├── Variables (set/get)
├── Functions (string, math, date, etc.)
├── Keywords (emptystring, null, etc.)
├── Constants (hardcoded values)
└── Nested references (dot notation)

Example Mapping:
  Module 1 (Watch Tasks): outputs { name, status, assignedTo }
  Module 2 (Create Issue): 
    Name field = {{1.name}} + " - Follow Up"
    Description = "Task {{1.name}} needs review"
    Assignee = {{1.assignedTo.name}}
```

### Mapping Expressions

```
Direct mapping:     {{1.name}}
Nested mapping:     {{1.assignedTo.firstName}}
Array access:       {{1.tasks[0].name}}
Function wrapping:  {{upper(1.name)}}
Concatenation:      {{1.firstName}} {{1.lastName}}
Conditional:        {{if(1.status = "CPL"; "Done"; "In Progress")}}
Nested functions:   {{formatDate(addDays(now; 7); "YYYY-MM-DD")}}
```

## Fusion Functions — Complete Reference

### String Functions

| Function | Syntax | Example |
|---|---|---|
| `lower` | `lower(text)` | `lower("HELLO")` → `"hello"` |
| `upper` | `upper(text)` | `upper("hello")` → `"HELLO"` |
| `length` | `length(text)` | `length("hello")` → `5` |
| `substring` | `substring(text; start; length)` | `substring("hello"; 1; 3)` → `"hel"` |
| `trim` | `trim(text)` | `trim(" hello ")` → `"hello"` |
| `replace` | `replace(text; search; replace)` | `replace("hello"; "l"; "r")` → `"herro"` |
| `contains` | `contains(text; search)` | `contains("hello"; "ell")` → `true` |
| `split` | `split(text; separator)` | `split("a,b,c"; ",")` → `["a","b","c"]` |
| `join` | `join(array; separator)` | `join(["a","b"]; "-")` → `"a-b"` |
| `md5` | `md5(text)` | Hash for deduplication |
| `base64` | `base64(text)` | Encode for API auth |

### Math Functions

| Function | Syntax | Example |
|---|---|---|
| `round` | `round(number; decimals)` | `round(3.567; 2)` → `3.57` |
| `floor` | `floor(number)` | `floor(3.9)` → `3` |
| `ceil` | `ceil(number)` | `ceil(3.1)` → `4` |
| `min` | `min(a; b)` | `min(5; 3)` → `3` |
| `max` | `max(a; b)` | `max(5; 3)` → `5` |
| `sum` | `sum(array)` | `sum([1;2;3])` → `6` |
| `average` | `average(array)` | `average([2;4;6])` → `4` |
| `abs` | `abs(number)` | `abs(-5)` → `5` |

### Date/Time Functions

| Function | Syntax | Example |
|---|---|---|
| `now` | `now` | Current timestamp |
| `formatDate` | `formatDate(date; format)` | `formatDate(now; "YYYY-MM-DD")` |
| `parseDate` | `parseDate(text; format)` | `parseDate("2026-05-15"; "YYYY-MM-DD")` |
| `addDays` | `addDays(date; days)` | `addDays(now; 7)` |
| `addMonths` | `addMonths(date; months)` | `addMonths(now; 1)` |
| `addYears` | `addYears(date; years)` | `addYears(now; 1)` |
| `dateDifference` | `dateDifference(date1; date2; unit)` | Days between two dates |
| `setHour` | `setHour(date; hour)` | Set specific hour |

### Array Functions

| Function | Syntax | Example |
|---|---|---|
| `length` | `length(array)` | `length([1;2;3])` → `3` |
| `first` | `first(array)` | `first([1;2;3])` → `1` |
| `last` | `last(array)` | `last([1;2;3])` → `3` |
| `merge` | `merge(array1; array2)` | Combines arrays |
| `contains` | `contains(array; value)` | Check if value in array |
| `remove` | `remove(array; value)` | Remove matching values |
| `add` | `add(array; value)` | Append to array |
| `map` | `map(array; key)` | Extract field from object array |
| `sort` | `sort(array)` | Sort ascending |
| `reverse` | `reverse(array)` | Reverse order |
| `flatten` | `flatten(array)` | Flatten nested arrays |
| `deduplicate` | `deduplicate(array)` | Remove duplicates |
| `distinct` | `distinct(array; key)` | Unique by key |

### General / Flow Functions

| Function | Syntax | Example |
|---|---|---|
| `if` | `if(condition; then; else)` | `if(1.status = "CPL"; "Done"; "Open")` |
| `ifempty` | `ifempty(value; default)` | `ifempty(1.name; "No Name")` |
| `switch` | `switch(value; case1; result1; ...)` | Status mapping |
| `omit` | `omit(object; key1; key2)` | Remove keys from object |
| `pick` | `pick(object; key1; key2)` | Keep only specified keys |
| `get` | `get(object; path)` | `get(1.data; "user.name")` |
| `typeof` | `typeof(value)` | Returns data type |
| `toNumber` | `toNumber(value)` | Convert to number |
| `toString` | `toString(value)` | Convert to string |

## Data Stores

```
Data Store Architecture:
├── Purpose: Persistent key-value storage across executions
├── Size limit: Based on subscription (usually 10MB-1GB)
├── Structure: Defined by Data Structure (schema)
├── Operations:
│   ├── Add/Replace a Record
│   ├── Update a Record
│   ├── Get a Record (by key)
│   ├── Search Records (with filters)
│   ├── Delete a Record
│   ├── Delete All Records
│   └── Check Record Existence
└── Use Cases:
    ├── Cross-scenario state sharing
    ├── Lookup tables (user mappings, status mappings)
    ├── Deduplication (track processed IDs)
    ├── Rate limit tracking
    ├── Caching API responses
    └── Configuration storage
```

### Data Store vs Variables

| Feature | Data Store | Variables |
|---|---|---|
| **Persistence** | Across executions | Within single execution only |
| **Scope** | Cross-scenario accessible | Current scenario only |
| **Performance** | Slower (API call) | Faster (in-memory) |
| **Size** | Large datasets | Small values |
| **Use case** | Lookup tables, state tracking | Temp calculations, counters |

---

### Practice Scenarios

**Scenario 1:** You need to convert a date from "05/15/2026" (US format) to "2026-05-15" (ISO format) and add 30 business days. Which function chain would you use?

<details><summary>Answer</summary>

```
formatDate(
  addDays(
    parseDate("05/15/2026"; "MM/DD/YYYY"); 
    42  // ~30 business days ≈ 42 calendar days
  ); 
  "YYYY-MM-DD"
)
```

Note: Fusion doesn't have a native `addBusinessDays` function. You'd either approximate (multiply by 1.4) or use a more complex approach with a helper scenario that skips weekends.

For exact business day calculation, use a custom function or iterate day-by-day checking `formatDate(date; "E")` for weekdays.
</details>

**Scenario 2:** A scenario processes 100 records from a webhook. You need to count how many have status "Active" and store the count. What approach?

<details><summary>Answer</summary>

1. **Iterator** to process the array (if webhook sends array)
2. **Router** with filter: `status = "Active"` on one path
3. **Numeric Aggregator** on the Active path to count bundles
4. **Data Store: Add/Replace** to store the count with today's date as key

Or simpler:
1. Use `length(filter(1.data; "item"; item.status = "Active"))` in a Set Variable module
2. Store in Data Store
</details>

---

*[Back to AD0-E902 Index](../README.md)*
