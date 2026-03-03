# Blueprints & Templates

[Back to Domain 2](./README.md) | [Back to AD0-E907 Index](../README.md)

---

## Blueprints

### What Are Blueprints?
Blueprints are pre-built, Adobe-provided configuration packages that can be installed to quickly set up common Workfront use cases.

### Blueprint Categories

| Category | Examples |
|---|---|
| **Project Templates** | Campaign management, product launch, IT onboarding |
| **Organizational Structures** | Department setup, role hierarchies |
| **Dashboards** | Executive dashboard, team performance |
| **Request Queues** | IT help desk, creative request intake |

### Blueprint Installation Process

```
1. Navigate: Main Menu → Blueprints
2. Browse catalog or search
3. Preview: Review what will be installed
4. Configure: Map roles, groups, preferences
5. Install: Creates objects in your instance
6. Customize: Modify installed objects as needed
```

### Blueprint Components (What Gets Installed)

```
Blueprint Package:
├── Project Template(s)
│   ├── Template Tasks
│   ├── Predecessors
│   ├── Duration estimates
│   └── Role assignments
├── Custom Forms
│   ├── Fields
│   └── Logic rules
├── Dashboards
│   └── Reports
├── Views / Filters / Groupings
├── Layout Templates (optional)
└── Milestones (optional)
```

### Key Blueprint Details for Exam

- Blueprints can be installed **multiple times**
- Each installation creates **new objects** (doesn't overwrite)
- Blueprint objects are **fully editable** after installation
- Only **System Admins** can install blueprints
- Blueprints don't include: users, access levels, groups (you map these)

## Project Template Architecture

### Template Design Hierarchy

```
Template Strategy for Enterprise:
├── Global Templates (company-wide standards)
│   ├── New Hire Onboarding
│   ├── Annual Budget Cycle
│   └── Compliance Audit
├── Department Templates
│   ├── Marketing: Campaign Launch
│   ├── IT: System Deployment
│   └── HR: Recruitment Process
└── Team Templates
    ├── Creative: Design Sprint
    ├── Development: Release Cycle
    └── PMO: Status Reporting
```

### Template vs Blueprint Comparison

| Feature | Template | Blueprint |
|---|---|---|
| **Source** | Created by admins | Adobe-provided |
| **Customization** | Full control | Post-install editing |
| **Sharing** | Configurable | System admin only |
| **Versioning** | Manual | Adobe may update catalog |
| **Includes** | Project structure only | Full configuration package |
| **Reusability** | Create projects from it | Install package of objects |

### Template Best Practices

1. **Use role-based assignments** — Never assign specific users in templates
2. **Include custom forms** — Pre-attach relevant forms
3. **Set default financial info** — Budget, cost types, revenue types
4. **Define risk templates** — Pre-populate risk registers
5. **Add document templates** — Attach standard documents
6. **Configure queue settings** — If the project accepts requests
7. **Version control naming** — `Template-Name-v2.1`
8. **Template sharing** — Restrict to appropriate groups

### Template Task Dependencies Pattern

```
Phase 1: Initiation
├── Task 1.1: Project Charter (5d)
├── Task 1.2: Stakeholder Analysis (3d, SS+2d with 1.1)
└── Task 1.3: Kickoff Meeting (1d, FS with 1.1)

Phase 2: Planning (FS with Phase 1)
├── Task 2.1: Requirements Gathering (10d)
├── Task 2.2: Solution Design (8d, FS with 2.1)
├── Task 2.3: Estimate & Schedule (5d, FS with 2.2)
└── Task 2.4: Planning Approval ★ (0d milestone, FS with 2.3)
    └── Approval Process: PM + Sponsor

Phase 3: Execution (FS with 2.4)
├── Task 3.1: Development Sprint 1 (10d)
├── Task 3.2: Development Sprint 2 (10d, FS with 3.1)
├── Task 3.3: Testing (10d, SS+5d with 3.2)
└── Task 3.4: UAT (5d, FS with 3.3)

Phase 4: Closure (FS with 3.4)
├── Task 4.1: Go-Live (2d)
├── Task 4.2: Post-Implementation Review (3d, FS+5d with 4.1)
└── Task 4.3: Project Closure ★ (0d milestone, FS with 4.2)
```

## Milestone Paths

### Milestone Path Architecture

```
Milestone Path: "Standard Project Lifecycle"
├── Milestone: Kickoff           (Color: Blue)
├── Milestone: Planning Complete (Color: Green)
├── Milestone: Build Complete    (Color: Yellow)
├── Milestone: UAT Complete      (Color: Orange)
├── Milestone: Go-Live          (Color: Red)
└── Milestone: Closure          (Color: Purple)

Association:
  Project → Milestone Path (one per project)
  Task → Milestone (one per task, from the project's path)
```

### Milestone Views
- **Milestone View** — Available on project lists and portfolios
- Shows progress against milestones
- Visual indicators: on time, late, at risk
- Useful for executive reporting

---

### Practice Scenarios

**Scenario 1:** An organization has 12 marketing campaign types, each with a slightly different process. How should templates be organized?

<details>
<summary>Answer</summary>

**Tiered template approach:**

1. **Base Campaign Template** — Common tasks shared across all campaigns:
   - Briefing, strategy, content creation, review, launch, measurement

2. **Variant Templates** — Built on the base with type-specific additions:
   - Email Campaign (adds: ESP setup, list segmentation)
   - Social Campaign (adds: platform-specific tasks)
   - Event Campaign (adds: venue, logistics, registration)
   - etc.

3. **Naming Convention:** `MKT-[Type]-Campaign-v[X]`
   - `MKT-Email-Campaign-v3`
   - `MKT-Social-Campaign-v2`

4. **Group Sharing:** Share templates only with the Marketing group

This scales better than one mega-template with display-logic-driven tasks.
</details>

---

*[← Approval Processes](./approval-processes.md) | [Back to Domain 2](./README.md)*
