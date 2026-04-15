# AD0-E902 — Domain Breakdown & Detailed Objectives

[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E902 Index](../README.md)

---

## Domain 1: Foundational Technical Concepts (39% — ~20 questions) **HEAVIEST**

*Measures the skills of Fusion Developers.*

### Official Objectives (Scenario-Based)

1. **Given a Fusion scenario with one field format that needs to be in another field format**, provide the possible functions that could be used to transform the data correctly
2. **Given a Fusion scenario that requires function nesting**, select the expression that was formed correctly
3. **Identify ways to manage team access and connections**, including managing organization teams and users
4. **Given a Fusion scenario**, identify the correct way to manipulate time zone settings
5. **Given a Fusion scenario**, identify the correct Workfront module(s) and/or action(s) (CRUD: Create, Read, Update, Delete, Search)
6. **Given a Fusion scenario**, identify the mapping panel expression and/or module(s) that would appropriately transform input data to output data
7. **Identify the correct ways to utilize the Fusion Dev Tool** to troubleshoot errors in execution or determine calls and responses made to third-party systems
8. **Given a Fusion scenario where data is needed on another routing path**, provide a set-get variable solution for making that data available on additional routing paths
9. **Identify the ways to use or produce a Fusion template**
10. **Given a Fusion scenario**, identify what to do to view executions and/or resolve errors
11. **Identify the different options in the Fusion scenario settings** to address scenario needs (including scheduling, data processing, and execution behavior)
12. **Given a Fusion scenario**, select the appropriate flow control and/or determine the correct combination of flow control module(s) (routes, iterators, aggregators, repeaters)

### Key Topics Covered

- Fusion function categories: String, Math, Date/Time, Array, General, Text
- Data mapping between modules via mapping panel
- Nested function calls: e.g., `formatDate(addDays(now; 7); "YYYY-MM-DD")`
- Data transformation and format conversion
- Data stores: CRUD operations, key-value storage, cross-scenario sharing
- Set Variable / Get Variable pattern across routes
- Fusion Dev Tool for debugging
- Fusion execution model: operations, cycles, data transfer
- Bundles and bundle processing
- Organization, teams, scenario management, and user permissions
- Scenario settings (scheduling, incomplete executions, error limits)
- Fusion templates: using, creating, and sharing
- Flow control: routers, iterators, aggregators, repeaters
- Timezone manipulation across scenarios
- Workfront modules and CRUD operations

---

## Domain 2: Scenario Design & Architecture (35% — ~18 questions)

*Measures the skills of Solution Architects.*

### Official Objectives (Scenario-Based)

1. **Given a Fusion scenario**, determine the correct steps to parse JSON and/or convert to bundles
2. **Given a Fusion scenario**, identify the correct way to look up data (data lookups between modules)
3. **Given a Fusion scenario**, identify the trigger that correctly addresses a given business outcome, distinguishing between instant and scheduled triggers
4. **Given a Fusion scenario**, identify the correct Fusion module, approach, or feature for uploading documents
5. **Given a Fusion scenario**, identify the correct way to track that a record was processed (marking records to avoid reprocessing)
6. **Given a Fusion scenario**, identify ways to simplify the design or optimize for maintenance (best practices, documentation)
7. **Identify best practices for documenting scenarios** (notes, labels, naming conventions)
8. **Given a Fusion scenario**, identify an opportunity to reduce data flow and/or simplify (limiting bundles, reducing operations)
9. **Given a Fusion scenario**, explain data that flows through the scenario using the bundle inspector
10. **Given a Fusion scenario**, identify how to archive and/or restore the scenario (versioning, scenario history)
11. **Given a Fusion scenario**, identify an approach to handle an Error 403: Forbidden and correctly select the origin of the error message and cause
12. **Given a Fusion scenario**, identify the usable ways to effectively use scheduling and execution management

### Key Topics Covered

- JSON parsing: Parse JSON module, JSON to bundles conversion
- Data lookup methods between modules
- Trigger selection: Watch (polling) vs Webhook (instant) vs Manual
- Instant triggers (webhooks) vs scheduled triggers (polling) -- when to use which
- Document upload approaches
- Record tracking / deduplication patterns
- Scenario optimization and design simplification
- Naming conventions and documentation best practices (labeling modules, inserting Note modules, adding notes)
- Bundle inspector analysis
- Scenario archiving and version history
- Error 403: Forbidden -- origin identification and resolution
- Scheduling types and execution management
- System limitations and guardrails
- Module selection for optimal design

---

## Domain 3: Testing & Error Handling (16% — ~8 questions)

*Measures the skills of Quality Assurance / Troubleshooting professionals.*

### Official Objectives (Scenario-Based)

1. **Identify the elements of a test plan / test cases** for Fusion scenarios
2. **Given a Fusion scenario with an unreliable service**, correctly identify the appropriate error-handling directive
3. **Given a Fusion scenario**, identify the process to add custom error handling
4. **Given a Fusion scenario**, identify how to track, read, and resolve **incomplete executions**
5. **Given a Fusion scenario with missing required data**, select ways to handle the invalid data

### Key Topics Covered

- Test planning: what elements belong in a test plan for Fusion scenarios
- Error handler directives: **Commit, Rollback, Ignore, Break, Resume**
  - **Break**: stores incomplete execution for manual review, stops the route
  - **Rollback**: stops execution immediately, rolls back all modules
  - **Commit**: processes all modules, stops at the error handler
  - **Ignore**: ignores the error, continues execution
  - **Resume**: specifies a substitute output, continues execution
- Custom error handling mechanisms
- "Store Incomplete Executions" scenario setting: captures failed runs for manual correction and reprocessing
- Incomplete executions queue: tracking, reading, resolving, and reprocessing
- Handling invalid / missing required data
- Run Once vs scheduled execution for testing
- Execution history and inspection
- Common error patterns and resolution
- The Fusion Dev Tool for troubleshooting

---

## Domain 4: Working with APIs (10% — ~5 questions)

*Measures the skills of Integration Specialists.*

### Official Objectives (Scenario-Based)

1. **Given a third-party API that returns a 429: Too Many Requests**, identify a solution to prevent or handle the error (rate limiting)
2. **Given a Fusion scenario where new functionality is not available in the Workfront module**, identify the correct reference and module type to use the new functionality (Custom API Call)
3. **Given a third-party system that does not have a dedicated app**, identify the HTTP app and select the appropriate module
4. **Distinguish CRUD operations and other common capabilities of REST APIs**

### Key Topics Covered

- HTTP module types: Make a Request, Make a Basic Auth Request, Make an OAuth 2.0 Request
- Custom API Call module within Workfront and other apps
- Rate limiting: HTTP 429 error handling, retry strategies, throttling
- REST API fundamentals: GET, POST, PUT, PATCH, DELETE
- CRUD operations mapping to HTTP methods
- Using HTTP app for systems without dedicated connectors
- OAuth2 universal connector for API integrations
- JSON parsing and creation for API requests/responses
- Authentication methods: API Key, Basic Auth, OAuth 2.0, Client Credentials
- Connection management and credential storage

---

## Study Priority Matrix

| Priority | Domain | Weight | Recommendation |
|---|---|---|---|
| **P0 -- Critical** | Foundational Technical Concepts | **39%** | Master functions, data mapping, Dev Tool, flow control |
| **P0 -- Critical** | Scenario Design & Architecture | **35%** | Build many scenarios, know triggers, JSON parsing, optimization |
| **P1 -- High** | Testing & Error Handling | **16%** | Know all 5 error handler directives and incomplete executions |
| **P2 -- Important** | Working with APIs | **10%** | HTTP module, 429 handling, CRUD operations |

> **Combined:** Domains 1 + 2 = **74%** of the exam. These two areas must be your primary focus.

---

## Sample Questions From Public Sources

### Question 1: Best Practices (Domain 2)
**Q:** Which two actions are best practices for making a Fusion scenario easier to read, share, and understand? (Choose two)
- A. Name all modules by providing short but relevant labels
- B. Insert Note Modules at the beginning of the scenario
- C. Add notes where applicable to clarify what is happening
- D. Use only the default module names

**Answer:** A and C. Naming modules with relevant labels and adding explanatory notes are the recommended documentation best practices.

### Question 2: Trigger Selection (Domain 2)
**Q:** Which Workfront module monitors project updates in real-time and triggers scenarios immediately upon detecting changes?
- A. Watch Events
- B. Watch Records
- C. Search
- D. Read a Record

**Answer:** A. The Watch Events module monitors project updates in real-time and triggers scenarios immediately upon detecting changes.

### Question 3: Incomplete Executions (Domain 3)
**Q:** An administrator needs to handle errors that occur during scenario execution without stopping the entire process. What should they do?
- A. Enable "Store Incomplete Executions" in scenario settings
- B. Add a Break directive after every module
- C. Use the Ignore directive on all modules
- D. Disable error notifications

**Answer:** A. Enable "Store Incomplete Executions" in scenario settings to capture failed runs for manual error correction and reprocessing.

### Question 4: String Construction (Domain 1)
**Q:** A project name needs to follow the format: "Two Digit Year - Reference Number - Project Name". Which expression is correct?
- A. `formatDate(now;YY) - referenceNumber - name`
- B. `join(formatDate(now;YY) - referenceNumber - name)`
- C. `concat(formatDate(now;YY), " - ", referenceNumber, " - ", name)`

**Answer:** C. The `concat()` function properly combines multiple strings with separators, while `join()` works only with arrays.

### Question 5: Module Execution Order (Domain 2)
**Q:** In a scenario with a Router that splits into two paths, what is the execution order?
- A. All paths execute simultaneously
- B. Modules execute left-to-right and top-to-bottom; Router directs execution along separate branches sequentially
- C. Only the first matching path executes
- D. Random order

**Answer:** B. Fusion executes modules left-to-right and top-to-bottom. Routers split execution into separate branches that are completed sequentially (top branch first, then bottom).

### Question 6: Error Handling (Domain 3)
**Q:** Which error handling mechanism supports conditional/nested error handling capabilities?
- A. Text Parser
- B. Filters
- C. Workfront app
- D. Routers

**Answer:** D. Routers enable conditional and nested error handling by splitting error flow into multiple paths with filter conditions.

### Question 7: API Rate Limiting (Domain 4)
**Q:** The HTTP module in your scenario returns a Custom API Call module. Which module type does this represent?
- A. Custom API Call
- B. Standard HTTP Request
- C. Webhook Trigger
- D. Action module

**Answer:** A. The HTTP module shown with dynamic parameters for Workfront API interaction represents a Custom API Call.

### Question 8: Aggregator Bundle Counts (Domain 1)
**Q:** A scenario has two routes after a Router. Route 1 has an aggregator showing 1 bundle; Route 2 has an aggregator showing 10 bundles. Why the difference?
- A. First route processes only the 1st bundle; second processes all bundles
- B. Different source modules feed each aggregator
- C. Aggregator set to repeat 10 times
- D. Router filters projects vs tasks differently

**Answer:** A. Route filters determine how many bundles pass through to each aggregator. If Route 1's filter matches only one bundle, its aggregator receives only one.

### Question 9: Variable Reuse Across Routes (Domain 1)
**Q:** How do you make data from one module available on multiple routing paths?
- A. Use Set Multiple Variables after the Workfront module; retrieve with Get Multiple Variables on other routing paths
- B. Map the data directly in each path
- C. Use a Data Store for every variable
- D. Duplicate the source module on each path

**Answer:** A. Set Variable / Get Variable modules allow you to store data before a router and retrieve it on any downstream routing path.

### Question 10: Timezone Handling (Domain 2)
**Q:** An organization needs all task due dates adjusted to a specific timezone. What is the correct approach?
- A. Change the Fusion organization's time zone
- B. Change the scenario's time zone default
- C. Set variables for every date using the formatDate function with timezone parameter
- D. Change computer localization settings

**Answer:** The correct approach involves understanding Fusion's timezone settings at the scenario level and using `formatDate()` with timezone parameters when needed.

---

*[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E902 Index](../README.md)*
