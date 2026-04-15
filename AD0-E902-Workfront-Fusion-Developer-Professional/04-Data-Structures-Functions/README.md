# Data Structures & Advanced Functions

[Back to AD0-E902 Index](../README.md)

---

## Data Structures — Schema Definitions

### What Are Data Structures?

Data Structures define the **shape** of data — like a schema or type definition. They tell Fusion what fields to expect and how to map them.

### Where Data Structures Are Used

| Context | Purpose |
|---|---|
| **Webhook trigger** | Define expected payload format |
| **Data Store** | Define record schema |
| **Parse JSON module** | Define expected JSON structure |
| **Create JSON module** | Define output JSON format |
| **HTTP request body** | Define API payload structure |
| **Array aggregator** | Define output structure |

### Data Structure Field Types

| Type | Description | Example |
|---|---|---|
| **Text** | String value | `"John Doe"` |
| **Number** | Integer or decimal | `42`, `3.14` |
| **Boolean** | True/False | `true` |
| **Date** | Date/DateTime | `2026-05-15T10:30:00Z` |
| **Collection** | Nested object | `{ name: "John", age: 30 }` |
| **Array** | List of items | `[1, 2, 3]` or `[{...}, {...}]` |

### Creating a Data Structure

```
Data Structure: "Invoice"
├── invoiceNumber (text, required)
├── date (date, required)
├── customer (collection)
│   ├── name (text)
│   ├── email (text)
│   └── company (text)
├── lineItems (array of collections)
│   ├── sku (text)
│   ├── description (text)
│   ├── quantity (number)
│   └── unitPrice (number)
├── subtotal (number)
├── tax (number)
├── total (number)
└── paid (boolean)
```

### Auto-Detection (Generator)

```
Generate Data Structure from Sample:
1. Create new Data Structure
2. Click "Generator"
3. Paste sample JSON or CSV data
4. Fusion auto-detects fields and types
5. Review and adjust field names/types
6. Save

Best for: webhook payloads, complex API responses
Tip: Use a COMPLETE sample with all possible fields
```

## Advanced Function Patterns

### Chaining Functions (Nested Calls)

```
Pattern: Inner function → feeds outer function

Example 1: Format future date
  formatDate(addDays(now; 14); "MMMM DD, YYYY")
  → "May 29, 2026"

Example 2: Clean and validate email
  lower(trim(1.email))
  → removes whitespace, converts to lowercase

Example 3: Extract domain from email
  last(split(lower(trim(1.email)); "@"))
  → "company.com"

Example 4: Conditional date formatting
  if(
    dateDifference(1.dueDate; now; "days") < 0;
    "OVERDUE";
    formatDate(1.dueDate; "MMM DD")
  )
```

### Switch for Multi-Value Mapping

```
switch(1.status;
  "NEW"; "Not Started";
  "INP"; "In Progress";
  "CPL"; "Complete";
  "DED"; "Cancelled";
  "Unknown Status"
)

Last value (no key) = default/fallback
```

### Working with Arrays

```
# Extract field from array of objects
map(1.tasks; "name")
→ ["Design", "Build", "Test"]

# Filter array (Fusion 2.0 pattern)
# Use Iterator → Router(filter) → Aggregator

# Count items matching condition
length(
  map(
    filter(1.items; "item"; item.status = "active");
    "id"
  )
)

# Deduplicate by specific field
distinct(1.records; "email")

# Flatten nested arrays
flatten([[1;2]; [3;4]; [5]])
→ [1, 2, 3, 4, 5]

# Sort objects by field (via Iterator + Aggregator with sort)
# No native object sort function — use Iterator pattern
```

### Handling Null and Empty Values

```
# Check for null/empty
ifempty(1.name; "Default Name")

# Explicit null check
if(1.field = null; "is null"; "has value")

# Chain multiple fallbacks
ifempty(1.preferredName; ifempty(1.firstName; "Unknown"))

# Handle missing nested fields
ifempty(get(1.data; "user.address.city"); "No City")

# Empty string vs null
if(1.field = ""; "empty string"; 
  if(1.field = null; "null"; "has value"))
```

### Date Manipulation Patterns

```
# Business hours check
if(
  toNumber(formatDate(now; "H")) >= 9 AND
  toNumber(formatDate(now; "H")) < 17 AND
  toNumber(formatDate(now; "E")) < 6;
  "Business Hours";
  "After Hours"
)

# First/Last day of month
parseDate(
  formatDate(now; "YYYY-MM") & "-01"; 
  "YYYY-MM-DD"
)

# Quarter calculation
ceil(toNumber(formatDate(now; "M")) / 3)
→ Q1=1, Q2=2, Q3=3, Q4=4

# ISO week number
formatDate(now; "W")
```

### Text Transformation Patterns

```
# Title Case (first letter uppercase)
upper(substring(1.name; 0; 1)) & lower(substring(1.name; 1))

# Slug from title
lower(replace(replace(trim(1.title); " "; "-"); "[^a-z0-9-]"; ""))

# Truncate with ellipsis
if(length(1.text) > 100;
  substring(1.text; 0; 97) & "...";
  1.text)

# Extract number from text
toNumber(replace(1.price; "[^0-9.]"; ""))
→ "$1,234.56" becomes 1234.56 (regex removes non-numeric)

# Pad number with zeros
right("000" & toString(1.number); 4)
→ 42 becomes "0042"
```

## Variables (Set/Get)

```
Variables Architecture:
├── Set Variable: store a value during execution
├── Get Variable: retrieve a stored value
├── Scope: current execution ONLY (not persistent)
├── Types: any (text, number, array, object)
└── Use cases:
    ├── Running counters
    ├── Accumulate values across bundles
    ├── Store intermediate calculations
    └── Pass data between router paths

Example: Count processed items
  Set Variable "counter" = 0 (before Iterator)
  [Iterator processes items]
  Set Variable "counter" = get(counter) + 1 (in loop)
  Get Variable "counter" (after Aggregator)
  → Total count
```

---

### Practice Scenarios

**Scenario 1:** You receive a webhook with a JSON array of employees. Each employee has `firstName`, `lastName`, and `department`. You need to:
1. Extract only employees in the "Engineering" department
2. Create a comma-separated list of their full names
3. Send it in an email

What modules and functions do you use?

<details><summary>Answer</summary>

```
Custom Webhook (trigger)
  │
  ▼
Iterator (split employees array)
  │
  ▼
Router:
  └── Filter: department = "Engineering"
      │
      ▼
      Text Aggregator (source: Iterator)
        Template: {{iterator.firstName}} {{iterator.lastName}}
        Separator: ", "
      │
      ▼
      Send Email
        Body: "Engineering team: {{textAggregator.text}}"
```

Alternative using functions (fewer modules):
```
Set Variable: "engNames"
  Value: join(
    map(
      filter(1.employees; "emp"; emp.department = "Engineering");
      "firstName"
    ); 
    ", "
  )
```
</details>

---

*[Back to AD0-E902 Index](../README.md)*
