# Layout Templates & Group Administration

[Back to Section Index](./README.md) | [Back to AD0-E907 Index](../README.md)

---

## Layout Templates

### What Layout Templates Control

```
Layout Template Customization:
├── Main Menu
│   ├── Show/hide menu items
│   ├── Reorder items
│   └── Custom labels (terminology)
├── Left Panel
│   ├── Show/hide panel items per object
│   ├── Reorder panel items
│   └── Default collapsed/expanded state
├── Home Page
│   ├── Widget configuration
│   ├── Default filters
│   └── Summary panel customization
├── Landing Page
│   └── First page user sees on login
├── Details View
│   ├── Show/hide detail sections
│   ├── Reorder sections
│   └── Custom groupings
├── Terminology
│   ├── Rename objects (e.g., "Project" → "Campaign")
│   └── Applies globally to that layout template
└── Pins
    └── Pre-pinned pages for quick access
```

### Layout Template Assignment Priority

```
Assignment hierarchy (highest priority wins):
1. User-level assignment (directly assigned to user)
2. Job Role assignment
3. Home Team assignment
4. Home Group assignment
5. System default layout template

If multiple templates apply, the MOST SPECIFIC wins.
```

### Layout Template Design Patterns

| Audience | Key Customizations |
|---|---|
| **System Admins** | Full menu, all panels, Setup access |
| **Project Managers** | Projects, Portfolios, Reports, Resource Mgmt |
| **Team Members** | Home, Tasks, Timesheets, Documents |
| **Requestors** | Requests, Home (limited), Updates |
| **Executives** | Dashboards, Analytics, Portfolios |

## Group Administration

### Group Admin Capabilities

```
Group Admin Can:
├── Manage group members
│   ├── Add/remove users from group
│   ├── Set subgroup assignments
│   └── Cannot change access levels
├── Create group-level objects
│   ├── Statuses
│   ├── Layout templates
│   ├── Approval processes
│   ├── Schedules
│   ├── Timesheet profiles
│   └── Custom forms (if permitted)
├── Manage subgroups
│   ├── Create subgroups
│   ├── Set subgroup admins
│   └── Inherit parent group settings
└── View group reports
    └── Users, projects, hours in group

Group Admin CANNOT:
├── Change system-wide settings
├── Modify access levels
├── Delete users from system
├── Access Setup > System area
└── Manage other groups' settings
```

### Group Hierarchy

```
Company
└── Group: "Marketing"
    ├── Subgroup: "Brand Team"
    │   ├── Users
    │   ├── Group statuses
    │   └── Layout templates
    ├── Subgroup: "Digital Team"
    │   ├── Users
    │   └── Own approval processes
    └── Subgroup: "Events Team"
        └── Users

Inheritance:
  • Subgroups inherit parent group statuses (can add more)
  • Subgroups can have own layout templates
  • Subgroup admins manage their scope only
```

### Boards Configuration

```
Boards (Kanban-Style):
├── Board Types
│   ├── Basic Board (standalone)
│   └── Connected Board (linked to Workfront objects)
├── Columns
│   ├── Custom column names
│   ├── Column policies (WIP limits)
│   └── Column mapping to statuses
├── Cards
│   ├── Ad-hoc cards (standalone)
│   ├── Connected cards (linked to tasks/issues)
│   └── Card fields: assignee, due date, tags, checklist
├── Filters
│   ├── By assignee, tag, status
│   └── Saved filter sets
└── Workstreams
    └── Group boards into collections
```

> **Exam Tip:** Boards is a relatively new feature. Know how to recommend Boards configuration for visual work tracking, especially connected cards that sync with tasks/issues.

---

### Practice Scenarios

**Scenario 1:** A company with 3 departments (Marketing, IT, Finance) needs different interfaces for each. Marketing wants "Projects" renamed to "Campaigns". IT needs full Setup access. Finance only needs Reports and Dashboards. How do you configure this?

<details>
<summary>Answer</summary>

Create **3 layout templates:**

1. **Marketing Layout:**
   - Terminology: Rename "Projects" to "Campaigns"
   - Main Menu: Campaigns, Requests, Home, Reports
   - Landing page: Campaigns list
   - Assign to: Marketing group

2. **IT Layout:**
   - Main Menu: All items including Setup
   - Left Panel: All panels visible
   - Landing page: Home
   - Assign to: IT group

3. **Finance Layout:**
   - Main Menu: Reports, Dashboards, Home only
   - Left Panel: Minimal panels
   - Landing page: Finance Dashboard
   - Assign to: Finance group

Each group's admin manages their group-specific statuses and approval processes.
</details>

---

*[← Access Levels](./access-levels.md) | [Back to Section Index](./README.md)*
