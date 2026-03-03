# Access Levels & Permissions

[Back to Section Index](./README.md) | [Back to AD0-E907 Index](../README.md)

---

## Access Level Architecture

### License Types

| License | Access | Typical Roles |
|---|---|---|
| **Plan** | Full access (configurable) | PMs, admins, power users |
| **Work** | Create tasks/issues, log time | Team members, contributors |
| **Review** | View projects, review proofs | Stakeholders, reviewers |
| **Request** | Submit requests, view own work | End users, requesters |
| **External** | Minimal; proof review only | Vendors, external reviewers |

### Access Level Components

```
Access Level: "Marketing Manager"
├── Projects
│   ├── View: Yes
│   ├── Create: Yes
│   └── Delete: No (fine-tuned)
├── Tasks
│   ├── View: Yes
│   ├── Create: Yes
│   └── Delete: Own only
├── Issues
│   ├── View: Yes
│   ├── Create: Yes
│   └── Delete: Own only
├── Portfolios: View Only
├── Programs: View Only
├── Reports
│   ├── View: Yes
│   ├── Create: Yes
│   └── Share: Yes
├── Dashboards: View + Create
├── Documents
│   ├── View: Yes
│   ├── Upload: Yes
│   └── Share: Yes
├── Users: View Only
├── Teams: View Only
├── Templates: View Only
├── Financial Data: View Only
├── Resource Management: View Only
├── Custom Forms: View Only
├── Scenario Planner: View Only
└── Administrative Functions
    ├── Group admin: No
    ├── Custom forms admin: No
    └── Approval processes admin: No
```

### Fine-Tuning Access

Within each object type, you can fine-tune:

| Permission | Options |
|---|---|
| **Create** | Yes / No |
| **Delete** | None / Things they create / All |
| **Share** | None / Things they create / All |
| **View** | No Access / View / Edit |
| **System-wide share** | Yes / No |
| **Public share** | Yes / No |

### Access Level vs Object Permissions

```
Access Level (cap)          Object Permission (specific)
──────────────              ─────────────────────────────
Sets maximum possible       Sets actual access per object
Applied to user profile     Applied per shared object
Admin-controlled            Owner-controlled sharing
Cannot exceed access level  Can be less than access level

Example:
  Access Level: Can Edit Projects
  Object Permission: View on specific project
  Result: User can only View that project
  (Object permission cannot exceed access level)
```

## Object-Level Permissions (Sharing)

### Permission Levels

| Level | Capabilities |
|---|---|
| **View** | See object details, updates |
| **Contribute** | View + Add updates, hours, documents, log time |
| **Manage** | Contribute + Edit, delete, share the object |

### Sharing Sources (Priority Order)

```
User's effective permission = HIGHEST of:
├── 1. Direct sharing (explicit share to user)
├── 2. Team sharing (any team user belongs to)
├── 3. Role sharing (user's primary/other roles)
├── 4. Group sharing (user's home group)
├── 5. Company sharing (user's company)
├── 6. System-wide default sharing
└── 7. Inherited permissions (from parent project)

Key: Permissions COMBINE — user gets the highest level
     from any source. They never reduce.
```

### Permission Inheritance

```
Project → Task/Issue Inheritance:
  Project: Manage → Task: Contribute (default)
  Project: Manage → Issue: Contribute (default)

  These defaults are configurable in:
  Setup > Access Levels > Fine-tune

  Override: Tasks/issues can have explicit sharing
  that is LESS than inherited (e.g., View only)
```

## Auto-Provisioning

### SSO/SCIM Auto-Provisioning

```
User Auto-Provisioning:
├── SAML SSO Provisioning
│   ├── Create user on first login
│   ├── Map IdP attributes to Workfront fields
│   ├── Assign access level based on group mapping
│   └── Set home group from IdP group claim
├── SCIM Provisioning
│   ├── Automated user lifecycle management
│   ├── Create/Update/Deactivate users
│   └── Sync from identity provider
└── Kickstart Import
    ├── Bulk user creation via Excel
    ├── Map fields to columns
    └── Include: access level, group, role, schedule
```

### Kickstart Data Import/Export

```
Kickstarts:
├── Export existing data as Excel templates
├── Modify data in Excel
├── Import to create/update objects in bulk
├── Supported objects:
│   ├── Users
│   ├── Access Levels
│   ├── Groups
│   ├── Custom Forms
│   ├── Projects / Templates
│   ├── Tasks / Template Tasks
│   └── Many more
└── Best practices:
    ├── Always export first to get correct format
    ├── Test in sandbox before production
    ├── Use for migration and bulk operations
    └── Back up before import
```

## Sharing Best Practices for Architects

| Practice | Description |
|---|---|
| **Template sharing** | Set up sharing on templates; projects inherit |
| **Team-based sharing** | Share with teams, not individuals |
| **Group-based access** | Leverage groups for department-level access |
| **Inherited permissions** | Use project-level sharing; tasks/issues inherit |
| **System defaults** | Configure system-wide sharing for new objects |
| **Audit regularly** | Review sharing periodically for over-permissions |

---

### Practice Scenarios

**Scenario 1:** A user with a "Work" license reports they can't create project templates. What's the explanation?

<details>
<summary>Answer</summary>

**Work** license does not have access to create Templates. Template creation requires a **Plan** license.

Work license capabilities:
- Create tasks and issues: Yes
- Create projects: No (can be granted View)
- Create templates: No
- Create reports: Yes (if enabled)
- View portfolios/programs: No

The user needs either a Plan license upgrade or must request a Plan-licensed user to create the template.
</details>

**Scenario 2:** A group admin needs to set up custom statuses for their group but reports they can't access the Setup area. What's wrong?

<details>
<summary>Answer</summary>

Check two things:

1. **Access Level:** The user needs "Group Administration" checked in their access level settings. This grants access to group-level Setup.

2. **Group Admin assignment:** The user must be designated as a Group Administrator for their specific group (Setup > Groups > select group > Group Members > set as admin).

Both conditions must be met:
- Access level permits group admin functions
- User is assigned as admin of the specific group

Group admins see a limited Setup area with only group-level options.
</details>

---

*[← Custom Forms](./custom-forms.md) | [Next: Layout Templates →](./layout-templates.md)*
