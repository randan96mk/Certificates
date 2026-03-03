# Report Builder & Text Mode

[Back to Domain: Reporting](./README.md) | [Back to AD0-E907 Index](../README.md)

---

## Report Builder Fundamentals

### Report Components

```
Report Structure:
├── Filters (what data to include/exclude)
│   ├── Basic filters (AND logic within a rule)
│   ├── Advanced filters (AND/OR logic)
│   └── Text mode filters
├── View / Columns (what data to display)
│   ├── Standard columns
│   ├── Calculated columns
│   └── Text mode columns
├── Groupings (how to organize data)
│   ├── Up to 3 grouping levels
│   └── Text mode groupings
├── Chart (optional visual representation)
│   ├── Bar, Column, Line, Pie, Gauge
│   └── Dual-axis charts
└── Prompts (optional user input at runtime)
    ├── Filter-based prompts
    └── Custom prompts
```

### Filter Operators

| Operator | Meaning | Example |
|---|---|---|
| Equal | Exact match | Status = Current |
| Not Equal | Excludes match | Status ≠ Dead |
| Greater Than | Above value | Hours > 40 |
| Less Than | Below value | Duration < 5 |
| Contains | Substring match | Name contains "Campaign" |
| Between | Range | Date between Jan 1 - Mar 31 |
| Is Blank | Null/empty | Assignee is Blank |
| Is Not Blank | Has value | Completion Date is Not Blank |

## Text Mode — Expert-Level Skill

### Text Mode Basics

Text mode uses a key-value syntax for defining columns, filters, and groupings beyond what the UI supports.

### Column (View) Text Mode Syntax

```
# Basic column
valuefield=name
valueformat=HTML
displayname=Project Name
linkedname=project
namekey=view.relatedcolumn
namekeyargkey.0=project
namekeyargkey.1=name

# Simplified equivalent
valuefield=name
valueformat=HTML
displayname=Project Name
```

### Value Expression Columns (Calculated)

```
# Days overdue
displayname=Days Overdue
textmode=true
valueexpression=IF({plannedCompletionDate}<$$TODAY,DATEDIFF($$TODAY,{plannedCompletionDate}),0)
valueformat=int

# Concatenated field
displayname=Owner Full Name
textmode=true
valueexpression=CONCAT({owner}.{firstName}," ",{owner}.{lastName})
valueformat=HTML

# Conditional text
displayname=Risk Level
textmode=true
valueexpression=IF({percentComplete}<25,"HIGH",IF({percentComplete}<75,"MEDIUM","LOW"))
valueformat=HTML
```

### Wildcards in Text Mode

| Wildcard | Meaning | Example |
|---|---|---|
| `$$USER` | Current logged-in user | `$$USER.ID` |
| `$$USER.homeGroupID` | Current user's home group | Filter by group |
| `$$TODAY` | Today's date (midnight) | Date comparisons |
| `$$NOW` | Current date and time | Timestamp comparisons |
| `$$USER.roleID` | Current user's primary role | Role-based filters |
| `$$USER.companyID` | Current user's company | Company-based filters |

### Wildcard Filter Examples

```
# Tasks assigned to current user
valuefield=assignedTo:ID
valueformat=val
type=in
value=$$USER.ID

# Projects in current user's group
valuefield=groupID
valueformat=val
type=in
value=$$USER.homeGroupID

# Tasks due this week
valueexpression=WEEKOFYEAR({plannedCompletionDate})
valueformat=int
type=eq
value=$$TODAY(+0w)
```

### Shared Columns (Merging Columns)

```
# Merge two columns into one with line break
# Column 0: Project Name
column.0.valuefield=name
column.0.valueformat=HTML
column.0.displayname=Project & Owner
column.0.width=200
column.0.sharecol=true

# Column 1: Owner (merged into column 0)
column.1.valuefield=owner:name
column.1.valueformat=HTML
column.1.displayname=
column.1.width=0
column.1.linkedname=owner

# Result: "Project Name" and "Owner Name" appear in same column
```

### Conditional Formatting (styledef)

```
# Red text when overdue
column.0.styledef.0.textmode=true
column.0.styledef.0.valueexpression={plannedCompletionDate}<$$TODAY
column.0.styledef.0.valueformat=HTML
column.0.styledef.0.textcolor=#FF0000
column.0.styledef.0.fontstyle=bold
```

### Collection References

```
# Report on child objects (e.g., tasks in a project report)
# Show task names in a project report
valuefield=tasks:name
valueformat=HTML
displayname=Task Names
listdelimiter=<br>
listmethod=nested(tasks).lists
type=iterate
```

## Common Text Mode Formulas

### Date Calculations

```
# Business days between dates
WEEKDAYDIFF({actualStartDate},{actualCompletionDate})

# Days until due date
DATEDIFF({plannedCompletionDate},$$TODAY)

# Add days to date
ADDDAYS({entryDate},30)

# Add weeks
ADDWEEKS({plannedStartDate},2)

# Format date
FORMATDATE({plannedCompletionDate},"MM/dd/yyyy")
```

### String Functions

```
# Concatenation
CONCAT({firstName}," ",{lastName})

# Substring
LEFT({referenceNumber},3)
RIGHT({referenceNumber},4)

# Search and replace
REPLACE({name},"Draft","Final")

# Contains check
CONTAINS("keyword",{name})

# Length
LEN({name})

# Upper/Lower
UPPER({name})
LOWER({name})
```

### Conditional Logic

```
# Simple IF
IF({status}="CPL","Complete","In Progress")

# Nested IF
IF({percentComplete}=100,"Done",
  IF({percentComplete}>50,"Over Half",
    IF({percentComplete}>0,"Started","Not Started")))

# IN operator
IN({status},"CUR","PLN","ONH")

# ISBLANK check
IF(ISBLANK({assignedTo:name}),"Unassigned",{assignedTo:name})
```

### Mathematical Functions

```
# Rounding
ROUND({actualCost}/1000,2)

# Ceiling/Floor
CEILING({duration}/5)
FLOOR({plannedHours}/8)

# Percentage
ROUND({actualHours}/{plannedHours}*100,1)

# Absolute value
ABS(DATEDIFF({plannedCompletionDate},{actualCompletionDate}))
```

## Report Types for Specific Use Cases

| Use Case | Report Type | Key Fields |
|---|---|---|
| Project status overview | Project | Status, Condition, % Complete, PM |
| Team workload | Task | Assigned To, Planned Hours, Status |
| Time tracking | Hour | User, Project, Hours, Date |
| Overdue items | Task/Issue | Planned Completion, Status (filter overdue) |
| User adoption | Login | User, Last Login Date, Access Level |
| Budget tracking | Project | Budget, Actual Cost, Planned Cost |
| Approval pipeline | Approval Path | Status, Approver, Object |
| Resource allocation | Assignment | User, Role, Hours, Project |

## Matrix Reports

```
Matrix Report Structure:
├── Row grouping (e.g., Project)
├── Column grouping (e.g., Month)
└── Intersection value (e.g., Sum of Hours)

Use case: Hours logged per project per month
- Rows: Project Name
- Columns: Entry Date (grouped by month)
- Values: SUM(hours)
```

---

### Practice Scenarios

**Scenario 1:** Create a report showing all projects where the current user is the PM, grouped by status, with a column showing the count of overdue tasks.

<details>
<summary>Answer</summary>

**Report Type:** Project

**Filter (text mode):**
```
valuefield=ownerID
valueformat=val
type=in
value=$$USER.ID
```

**Calculated Column (text mode):**
```
displayname=Overdue Tasks
textmode=true
valueexpression=
  (SELECT COUNT(*) FROM tasks
   WHERE projectID = {ID}
   AND status != "CPL"
   AND plannedCompletionDate < $$TODAY)
```

Note: In practice, you'd use a sub-report or a separate tasks report grouped by project and filtered for overdue. Workfront doesn't support true subqueries in text mode — use collection references or separate reports on a dashboard.

**Grouping:** Status (first level)
</details>

**Scenario 2:** A manager wants a single column showing "Project Name - Owner Name". How do you configure this?

<details>
<summary>Answer</summary>

Use **shared columns (sharecol)** in text mode:

```
column.0.valuefield=name
column.0.valueformat=HTML
column.0.displayname=Project - Owner
column.0.sharecol=true

column.1.valueexpression=CONCAT(" - ",{owner}.{name})
column.1.valueformat=HTML
column.1.displayname=
```

Or use a single calculated column:
```
displayname=Project - Owner
textmode=true
valueexpression=CONCAT({name}," - ",{owner}.{name})
valueformat=HTML
```
</details>

---

*[Back to Domain: Reporting](./README.md) | [Next: Dashboards & Analytics →](./dashboards-analytics.md)*
