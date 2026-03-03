# Issue Management & Conversion

[Back to Domain 1](./README.md) | [Back to AD0-E907 Index](../README.md)

---

## Issue Types

| Type | Default Use | Customizable |
|---|---|---|
| **Bug Report** | Software defects | Name, icon |
| **Change Order** | Scope changes | Name, icon |
| **Issue** | General problems | Name, icon |
| **Request** | Service requests | Name, icon |

> Issue type names can be customized at Setup > Project Preferences > Issue Types. Each project can enable/disable which issue types appear.

## Issue Lifecycle

```
New → In Progress → Closed/Resolved
         │
         ├── Won't Resolve
         └── Cannot Reproduce

Custom statuses can extend this workflow per group.
```

## Issue Severity & Priority

### Severity (Impact Assessment)
| Level | Meaning |
|---|---|
| Cosmetic | Minor visual issue |
| Causes Confusion | Usability problem |
| Bug with Workaround | Functional issue, alternative exists |
| Bug with No Workaround | Functional issue, no alternative |
| Fatal Error | System-breaking issue |

### Priority
| Level | Meaning |
|---|---|
| None | Not prioritized |
| Low | Can wait |
| Normal | Standard priority |
| High | Needs attention soon |
| Urgent | Needs immediate attention |

## Issue Conversion — Critical Exam Topic

### Converting Issues to Tasks

```
Issue → Convert to Task
├── Source issue can be:
│   ├── Kept (linked as resolving object)
│   ├── Deleted
│   └── Kept but status locked to resolved
├── Task inherits:
│   ├── Issue name (editable)
│   ├── Issue description
│   ├── Custom form data (if mapped)
│   └── Assignee (optional)
├── Configuration options:
│   ├── Keep original issue
│   ├── Allow primary contact to access task
│   └── Keep planned completion date
└── Resolving Object:
    └── Task becomes the resolving object for the issue
    └── When task completes → issue auto-resolves
```

### Converting Issues to Projects

```
Issue → Convert to Project
├── Source issue can be:
│   ├── Kept (linked as resolving object)
│   └── Deleted
├── Project creation:
│   ├── From template (recommended)
│   ├── Blank project
│   └── Name defaults to issue name
├── Custom form mapping:
│   └── Map issue fields → project fields
├── Resolving Object:
│   └── Project becomes the resolving object
│   └── When project completes → issue auto-resolves
└── Financial:
    └── Issue hours can transfer to project
```

### Resolving Objects — Key Concept

```
An issue can be resolved by:
├── Task (within same or different project)
├── Project (separate project)
└── Another Issue (duplicate resolution)

Resolution chain:
  Issue (status: Resolved) ←──resolves── Task/Project/Issue

When resolving object completes:
  → Issue status automatically changes to "Closed - Resolved"
  → Issue cannot be reopened unless resolving object is reopened

Key Rules:
  • Only ONE resolving object per issue
  • Resolving object overrides manual status changes
  • Issue inherits the resolution status from its resolving object
```

## Issue Routing

### Queue-Based Routing

```
Request Queue
├── Topic Group (category)
│   ├── Queue Topic A
│   │   └── Routing Rule → Team Alpha
│   ├── Queue Topic B
│   │   └── Routing Rule → User: John
│   └── Queue Topic C
│       └── Routing Rule → Role: Developer
└── Default Routing Rule
    └── Applied when no topic-specific rule matches
```

### Routing Rule Options

| Route To | Behavior |
|---|---|
| **User** | Directly assigned to specific user |
| **Role** | Assigned to job role (resolved later) |
| **Team** | Appears in team's work queue |
| **Project** | Issue created in specified project |
| **Default Route** | Fallback when no rule matches |

## Issue Settings & Preferences

### System-Level Preferences (Setup > Project Preferences)
- Default issue status for new issues
- Automatically convert issues when status changes
- Lock/unlock issue preferences for groups
- Default severity and priority

### Project-Level Settings
- Queue setup (if project is a request queue)
- Issue types enabled for the project
- Custom forms for issues
- Issue statuses (group-specific)

---

### Practice Scenarios

**Scenario 1:** A help desk team receives a bug report via a request queue. After investigation, they determine it requires a development project. The original submitter needs to track the resolution. What's the best approach?

<details>
<summary>Answer</summary>

**Convert the issue to a project** using a development project template:
1. Keep the original issue (don't delete)
2. Map custom form fields from the issue to the project
3. The project becomes the **resolving object**
4. Grant the original contact (submitter) **View access** to the new project
5. When the project completes, the issue auto-resolves
6. Submitter can track progress on the original issue, which reflects the project status
</details>

**Scenario 2:** Users report that resolved issues keep reopening. What's causing this?

<details>
<summary>Answer</summary>

The issues likely have **resolving objects** (tasks or projects) that are being reopened or reverted:

1. If a task is the resolving object and its status changes from Complete back to In Progress → the issue reopens
2. If someone removes the resolving object relationship
3. If the resolving object is deleted

**Fix:** Check the resolving object for each issue. Ensure the linked task/project remains complete. Set up notifications to alert when resolving objects change status.
</details>

---

*[← Task Management](./task-management.md) | [Next: Object Model →](./object-model.md)*
