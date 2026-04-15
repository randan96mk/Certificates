# AD0-E902 — Domain Breakdown & Detailed Objectives

[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E902 Index](../README.md)

---

## Domain 1: Core Technical Concepts (~26% — ~9 questions)

### Objectives
- Understand mapping that requires multiple functions (nested, chained)
- Demonstrate ability to transform data using Fusion functions
- Understand the purpose and use of data stores
- Identify features and functionalities of Fusion

### Key Topics
- Fusion function categories (String, Math, Date, Array, General, Text, etc.)
- Data mapping between modules
- Nested function calls: `formatDate(addDays(now; 7); "YYYY-MM-DD")`
- Data stores: CRUD operations, key-value storage, cross-scenario sharing
- Fusion execution model: operations, cycles, data transfer
- Bundles and bundle processing
- Organization, teams, and scenario management

---

## Domain 2: Scenario Design & Architecture (~32% — ~11 questions) **HEAVIEST**

### Objectives
- Design and build complete Fusion scenarios
- Choose appropriate trigger types for use cases
- Implement routers, filters, and flow control
- Apply iterators and aggregators correctly
- Use scheduling and execution management
- Implement data structures for complex mappings

### Key Topics
- Trigger selection: Watch (polling) vs Webhook (instant) vs Manual
- Module types: action, search, aggregator, iterator, transformer
- Router: parallel paths with filter conditions
- Iterator: loop through arrays
- Aggregator: combine multiple bundles into one
- Repeater: execute N times
- Sleep: pause between actions
- Data structures: define schemas for complex data
- Scheduling: immediate, intervals, cron-like, on-demand
- Blueprint import/export
- Scenario versioning and history
- Flow control patterns for complex business logic

---

## Domain 3: Testing, Error Handling, & Troubleshooting (~22% — ~7 questions)

### Objectives
- Apply appropriate error handling strategies
- Debug and troubleshoot scenario failures
- Understand incomplete executions and retry mechanisms
- Test scenarios effectively

### Key Topics
- Error handler types: Commit, Rollback, Ignore, Break, Resume
- Incomplete executions queue and manual retry
- Execution history and inspection
- DevTool for debugging
- Run Once vs scheduled execution for testing
- Common error patterns and resolution
- Rate limiting and throttling
- Data validation before processing

---

## Domain 4: APIs, Connectors, & Webhooks (~20% — ~6 questions)

### Objectives
- Configure and use the HTTP module for REST API calls
- Implement webhooks (incoming and custom)
- Work with JSON parsing and creation
- Understand authentication methods for connections
- Use pre-built app connectors effectively

### Key Topics
- HTTP module: Make a Request, Make a Basic Auth Request, Make an OAuth 2.0 Request
- Webhook configuration: custom webhooks, instant triggers
- JSON: Parse JSON, Create JSON, Transform JSON
- Authentication: API Key, Basic Auth, OAuth 2.0, Client Credentials
- Connection management and credential storage
- Response parsing and error detection
- Common connectors: Workfront, Google Sheets, Slack, Jira, HTTP

---

## Study Priority Matrix

| Priority | Domain | Weight | Recommendation |
|---|---|---|---|
| **P0 — Critical** | Scenario Design & Architecture | 32% | Build many scenarios hands-on |
| **P0 — Critical** | Core Technical Concepts | 26% | Master functions and data stores |
| **P1 — High** | Testing & Error Handling | 22% | Know all 5 error handler types |
| **P1 — High** | APIs, Connectors, Webhooks | 20% | HTTP module + webhook patterns |

---

*[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E902 Index](../README.md)*
