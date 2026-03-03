# Domain 4: Document Management and Proofing (~13%)

[Back to AD0-E907 Index](../README.md)

---

## Overview

This domain tests knowledge of document management vs proofing workflows, proof viewer capabilities, external storage integrations, and AEM Assets connectivity.

## Key Exam Objectives

- Distinguish between document management and proofing use cases
- Apply best practices using the proofing viewer, settings, and markup
- Edit and set permissions on a file
- Configure external document storage with security restrictions
- Connect to AEM as a Cloud Service or AEM Assets Essentials

---

## Document Management vs Proofing

| Feature | Document Management | Proofing |
|---|---|---|
| **Purpose** | Store, organize, share files | Review, annotate, approve content |
| **Workflow** | Upload → Version → Approve | Upload → Proof → Review → Decision |
| **Collaboration** | Comments on document page | Inline markup on proof viewer |
| **Approval** | Document approval (simple) | Proof approval (structured workflow) |
| **Versioning** | Manual version upload | Version comparison in viewer |
| **Decision Types** | Approved / Rejected | Approved / Approved with Changes / Changes Required / Not Relevant |
| **Roles** | Viewer / Contributor / Manager | Read Only / Reviewer / Approver / Author / Moderator |

## Proofing Deep Dive

### Proof Roles

| Role | Can View | Can Comment | Can Make Decision | Can Edit Proof |
|---|---|---|---|---|
| **Read Only** | Yes | No | No | No |
| **Reviewer** | Yes | Yes | No | No |
| **Approver** | Yes | No | Yes | No |
| **Reviewer & Approver** | Yes | Yes | Yes | No |
| **Author** | Yes | Yes | Yes | Yes (own proofs) |
| **Moderator** | Yes | Yes | Yes | Yes (all proofs) |

### Proof Decisions

| Decision | Meaning | Result |
|---|---|---|
| **Approved** | Content is ready | Proof status: Approved |
| **Approved with Changes** | Minor changes needed, no re-review | Proof status: Approved with Changes |
| **Changes Required** | Significant changes needed, re-review | Proof status: Changes Requested |
| **Not Relevant** | Reviewer not applicable | No status change |

### Automated Workflow

```
Automated Proof Workflow:
├── Stage 1: Internal Review
│   ├── Reviewers: Design Team
│   ├── Deadline: 2 days
│   ├── Lock: No
│   └── When complete → activate Stage 2
├── Stage 2: Manager Approval
│   ├── Approvers: Creative Director
│   ├── Deadline: 1 day
│   ├── Lock: Previous stage locked
│   └── When complete → activate Stage 3
└── Stage 3: Client Approval
    ├── Approvers: Client Stakeholder
    ├── Deadline: 3 days
    ├── Lock: All previous stages
    └── Decision required to complete

Stage Activation Rules:
├── On proof creation (immediate)
├── When deadline passes
├── When all decisions made
├── On specific date
└── When previous stage complete
```

### Proof Viewer Features

```
Proofing Viewer:
├── Markup Tools
│   ├── Arrow, Line, Rectangle, Circle
│   ├── Highlight, Strikethrough
│   ├── Freehand drawing
│   ├── Text annotation
│   └── Measurement tool
├── Navigation
│   ├── Zoom in/out
│   ├── Page navigation
│   ├── Keyboard shortcuts
│   └── Fullscreen mode
├── Compare Mode
│   ├── Side-by-side comparison (2 versions)
│   └── Overlay comparison
├── Comment Thread
│   ├── Threaded replies
│   ├── Resolve comments
│   └── @mention users
└── Settings
    ├── Auto-play (video proofs)
    ├── Grid overlay
    └── Color picker
```

### Proof Viewer Types

| Type | Web Viewer | Desktop Viewer |
|---|---|---|
| **Supported Files** | Images, PDFs, Web | Images, PDFs, Web + Video + Interactive |
| **Access** | Browser-based | Installed application |
| **Video Proofing** | No | Yes |
| **Interactive Content** | Limited | Full support |
| **Comment/Markup** | Full | Full |

## External Document Storage

### Supported Integrations

| Integration | Type | Linked Folders |
|---|---|---|
| **AEM Assets as a Cloud Service** | Native | Yes |
| **AEM Assets Essentials** | Native | Yes |
| **Google Drive** | Cloud storage | Yes |
| **SharePoint Online** | Cloud storage | Yes |
| **OneDrive** | Cloud storage | Yes |
| **Box** | Cloud storage | Yes |
| **Dropbox** | Cloud storage | Yes |
| **WebDAM** | DAM | Yes |

### AEM Integration Architecture

```
AEM as a Cloud Service ←──── Native Integration ────→ Workfront
├── Linked Folders
│   ├── AEM folder mapped to Workfront project
│   ├── Documents sync automatically
│   └── Metadata mapping (custom fields)
├── Asset References
│   ├── View AEM assets from Workfront
│   ├── Download from AEM to Workfront
│   └── Push Workfront documents to AEM
├── Metadata Sync
│   ├── Workfront fields → AEM metadata
│   ├── Bi-directional sync (configurable)
│   └── Custom field mapping
└── Workflow Triggers
    ├── Workfront proof complete → AEM workflow start
    └── AEM asset approved → Workfront status update
```

### "Latest Version" Label

> **Exam Tip:** The "Latest Version" label on a document is the quickest way for a user to identify the most current version. This is a commonly tested concept. When a new version is uploaded, the label automatically moves to the newest version.

## Document Security

```
Document Permission Model:
├── Inherit from parent object (project/task)
├── Direct sharing to users/teams/roles
├── Access level controls (document access settings)
└── External storage permissions (provider-specific)

Security Restrictions:
├── File type restrictions (admin can block certain types)
├── Size limits (admin-configurable)
├── Download permissions (can restrict downloads)
├── Print permissions (on proofs)
└── External sharing controls
```

---

### Practice Scenarios

**Scenario 1:** A creative agency needs a review workflow where:
- Internal designers review first
- Creative director approves next
- Client reviews last
- Each stage must complete before the next starts
- Client should not see internal comments

How should this be configured?

<details>
<summary>Answer</summary>

Use an **Automated Proof Workflow Template:**

**Stage 1: Internal Design Review**
- Add: Design Team (Reviewer role)
- Deadline: 2 days
- Lock: No locking
- Activation: On proof creation

**Stage 2: Creative Director Approval**
- Add: Creative Director (Reviewer & Approver role)
- Deadline: 1 day
- Lock: Stage 1 (prevents further changes)
- Activation: When all Stage 1 decisions made

**Stage 3: Client Review**
- Add: Client Stakeholder (Reviewer & Approver role)
- Deadline: 3 days
- Lock: All previous stages
- Activation: When Stage 2 approved

**Privacy:** The automated workflow stages are separate — the client in Stage 3 will only see comments from their stage unless "sharing" is explicitly enabled. To ensure internal comments are hidden, verify the proof settings restrict comment visibility across stages.
</details>

---

*[Back to AD0-E907 Index](../README.md)*
