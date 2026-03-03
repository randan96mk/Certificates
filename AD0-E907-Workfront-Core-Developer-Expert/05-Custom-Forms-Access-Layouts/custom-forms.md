# Custom Forms & Calculated Fields

[Back to Section Index](./README.md) | [Back to AD0-E907 Index](../README.md)

---

## Custom Form Architecture

### Form Design Components

```
Custom Form:
├── Object Type(s)
│   ├── Single-object form (e.g., Project only)
│   └── Multi-object form (e.g., Project + Task + Issue)
├── Sections
│   ├── Section Break
│   │   ├── Name & Description
│   │   └── Permissions (View/Edit per access level)
│   └── Fields within section
├── Field Types
│   ├── Data fields (capture input)
│   ├── Calculated fields (auto-compute)
│   ├── Display fields (descriptive text)
│   └── External lookup fields (API-driven)
├── Logic Rules
│   ├── Display Logic (show/hide fields)
│   └── Skip Logic (jump to field/section)
└── Sharing
    ├── Who can see the form
    └── Who can attach the form
```

### Field Types Reference

| Field Type | Input | Reportable | Filterable | Notes |
|---|---|---|---|---|
| **Single Line Text** | Free text | Yes | Yes | Max 255 chars |
| **Paragraph Text** | Long text | Yes | Limited | Supports rich text |
| **Number** | Numeric | Yes | Yes | Decimal precision configurable |
| **Date** | Date picker | Yes | Yes | Date only or Date + Time |
| **Dropdown** | Single select | Yes | Yes | Predefined options |
| **Multi-Select Dropdown** | Multi select | Yes | Contains filter | Comma-separated storage |
| **Checkbox** | Boolean | Yes | Yes | True/False |
| **Radio Buttons** | Single select | Yes | Yes | Visible options |
| **Typeahead** | Search select | Yes | Yes | User, Project, etc. |
| **Calculated** | Auto-computed | Yes | Depends on type | Formula-based |
| **External Lookup** | API-driven | Yes | Limited | REST API endpoint |
| **Descriptive Text** | Display only | No | No | Instructions/guidance |

### Multi-Object Forms

```
Multi-Object Form Example:
  "Project Metadata" form available on:
  ├── Project → All fields available
  ├── Task → Some fields may not apply
  └── Issue → Some fields may not apply

Key Rules:
  • Fields shared across objects: same field works everywhere
  • Object-specific fields: some fields only available on certain objects
  • Calculated fields: must use fields common to all selected objects
    OR use IF(ISBLANK()) to handle missing fields
  • Section permissions: may differ by object type
```

## Calculated Fields — Deep Dive

### Syntax Rules

```
Basic Syntax:
  {fieldName}              → Native field reference
  {DE:Custom Field Name}   → Custom field reference
  {owner}.{name}           → Related object field
  $$USER                   → Current user
  $$TODAY                  → Today's date
  $$NOW                    → Current timestamp
```

### Common Calculated Field Patterns

#### Date Calculations

```
# Business days between two dates
WEEKDAYDIFF({actualStartDate},{actualCompletionDate})

# Calendar days between dates
DATEDIFF({plannedCompletionDate},{actualCompletionDate})

# Days until deadline
DATEDIFF({plannedCompletionDate},$$TODAY)

# Add business days
ADDDAYS({entryDate},5)  -- calendar days
ADDWEEKDAYS({entryDate},5)  -- business days

# Is overdue?
IF({plannedCompletionDate}<$$TODAY AND {status}!="CPL","OVERDUE","ON TRACK")
```

#### Text Manipulation

```
# Full name
CONCAT({owner}.{firstName}," ",{owner}.{lastName})

# Project code from name
LEFT({name},3)

# Truncate long text
IF(LEN({description})>100,
  CONCAT(LEFT({description},100),"..."),
  {description})

# Status label mapping
SWITCH({status},
  "CUR","Active",
  "PLN","Planning",
  "CPL","Complete",
  "DED","Cancelled",
  "Unknown")
```

#### Conditional Logic

```
# Priority color
IF({priority}=1,"#FF0000",
  IF({priority}=2,"#FFA500",
    IF({priority}=3,"#008000","#808080")))

# Budget variance
ROUND(({actualCost}-{plannedCost})/{plannedCost}*100,1)

# Risk assessment
IF({DE:Budget}>100000 AND {DE:Timeline Days}>90,"HIGH RISK",
  IF({DE:Budget}>50000 OR {DE:Timeline Days}>60,"MEDIUM RISK","LOW RISK"))
```

#### Multi-Object Calculated Fields

```
# Field that works on both Project and Task
IF(ISBLANK({project}.{name}),
  {name},
  CONCAT({project}.{name}," > ",{name}))

# This avoids errors when the form is on a Project
# (where {project}.{name} would be blank/self-referencing)
```

### Common Calculated Field Errors and Fixes

| Error | Cause | Fix |
|---|---|---|
| `WHOOPS!` | Invalid expression | Check syntax, field references |
| `Invalid expression` | Wrong field name | Verify field API names |
| Blank result | Field has no value | Add `ISBLANK()` checks |
| `#VALUE` | Type mismatch | Ensure matching types in comparisons |
| Circular reference | Field references itself | Restructure formula |

## Display Logic

```
Display Logic Rule:
  WHEN [Trigger Field] = [Specific Value]
  THEN SHOW [Target Field]

  Example:
    WHEN "Request Type" = "Hardware"
    THEN SHOW "Hardware Specification" field

  Rules:
  • Trigger field must be: Dropdown, Radio, Checkbox
  • Target field: any field type
  • Multiple rules can apply to same field (OR logic)
  • Hidden fields retain their values
  • Logic is per-form, not cross-form
```

## Skip Logic

```
Skip Logic Rule:
  WHEN [Trigger Field] = [Specific Value]
  THEN SKIP TO [Target Field/Section]

  Example:
    WHEN "Is External Vendor" = "Yes"
    THEN SKIP TO "Vendor Details" section

  Rules:
  • Skipped fields are visible but cursor jumps
  • Does NOT hide fields (use Display Logic for that)
  • Applies to tab-key navigation order
  • Less commonly used than Display Logic
```

## Section Break Permissions

```
Section Break Permissions:
├── View Access Level → View / No Access
├── Edit Access Level → View / Edit / No Access
├── Admin Access Level → Full control
└── Team/Role-based → Custom permission sets

Example Architecture:
  Form: "Project Details"
  ├── Section: "General Info" (View: All, Edit: All)
  ├── Section: "Financial Details" (View: Finance Team, Edit: Finance Admin)
  ├── Section: "Technical Specs" (View: All, Edit: IT Team)
  └── Section: "Executive Summary" (View: Directors+, Edit: PMO)
```

## Value Passing: Issue → Project/Task Conversion

```
When an issue is converted to a project or task:
├── Native fields: Mapped automatically
│   ├── Name → Name
│   ├── Description → Description
│   └── Assignee → Assignee (optional)
├── Custom form fields: Mapped if forms are configured
│   ├── Same form attached to both objects → values transfer
│   ├── Different forms → map matching field names
│   └── No matching form → data may be lost
└── Best Practice:
    ├── Use multi-object forms (Issue + Project)
    ├── Ensure field names match across forms
    └── Test conversion thoroughly
```

---

### Practice Scenarios

**Scenario 1:** A calculated field on a multi-object form (Project + Task) shows "WHOOPS!" when attached to a task. The formula references `{portfolio}.{name}`. What's the issue?

<details>
<summary>Answer</summary>

Tasks don't have a direct `{portfolio}` relationship — only projects do. On a task, the reference fails.

**Fix:** Use a path through the project:
```
IF(ISBLANK({project}.{portfolio}.{name}),
  "No Portfolio",
  {project}.{portfolio}.{name})
```

Or handle the multi-object scenario:
```
IF(ISBLANK({portfolio}.{name}),
  IF(ISBLANK({project}.{portfolio}.{name}),
    "No Portfolio",
    {project}.{portfolio}.{name}),
  {portfolio}.{name})
```
</details>

**Scenario 2:** Users in the Finance department need to see and edit financial fields on a project custom form, but other users should not see those fields at all. How do you implement this?

<details>
<summary>Answer</summary>

Use **Section Break permissions**:

1. Create a section break titled "Financial Details"
2. Set permissions:
   - Plan license users: **No Access** (default)
   - Finance group users: **View and Edit**
   - System Admins: **View and Edit**
3. Place all financial custom fields below this section break
4. Users without access won't see the section at all

This is more robust than Display Logic because it's access-level based, not field-value based.
</details>

---

*[Back to Section Index](./README.md) | [Next: Access Levels →](./access-levels.md)*
