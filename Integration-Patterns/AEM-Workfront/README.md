# AEM + Workfront Integration Patterns

[Back to Integration Patterns](../README.md) | [Back to Main README](../../README.md)

---

## Native Integration Overview

Adobe provides a **native connector** between AEM and Workfront for seamless asset management and project workflows.

## Architecture

```
┌────────────────────┐         ┌────────────────────┐
│    Workfront        │  Native │    AEM as a Cloud   │
│                     │ Connector│    Service          │
│  Projects           │◀───────▶│  Assets             │
│  Tasks              │         │  Content Fragments  │
│  Documents          │         │  Metadata           │
│  Proofs             │         │  Workflows          │
│  Custom Forms       │         │  Linked Folders      │
└────────────────────┘         └────────────────────┘
```

## Key Integration Capabilities

### 1. Linked Folders

```
Configuration:
  Workfront Project → AEM Linked Folder
  ├── Auto-create AEM folder when project is created
  ├── Folder structure mirrors project hierarchy
  ├── Documents added in Workfront → appear in AEM
  ├── Assets in AEM folder → accessible from Workfront
  └── Bi-directional: changes sync both ways

Setup:
  1. Configure AEM cloud service in Workfront
  2. Map Workfront project template → AEM folder template
  3. Define folder naming convention
  4. Set metadata mapping rules
```

### 2. Metadata Mapping

```
Workfront Field          →    AEM Metadata Field
──────────────────            ─────────────────
Project Name             →    dc:title
Project Description      →    dc:description
Custom Form: Brand       →    dam:brand
Custom Form: Campaign    →    dam:campaign
Custom Form: Region      →    dam:region
Proof Status             →    dam:approvalStatus
Owner                    →    dam:creator

Mapping Configuration:
  Setup > Documents > Metadata Mapping
  ├── Define field pairs
  ├── Set sync direction (WF→AEM, AEM→WF, bidirectional)
  └── Map custom fields to AEM metadata schemas
```

### 3. Document/Proof Workflow

```
Workflow Pattern:
1. Creative brief created in Workfront
2. Designer uploads asset to linked AEM folder
3. Asset appears in Workfront document list
4. PM creates proof from AEM asset
5. Stakeholders review proof in Workfront
6. Proof decisions sync to AEM metadata
7. Approved asset triggers AEM publishing workflow
```

### 4. Event-Driven Integration

```
Workfront Events → AEM Actions:
├── Project Created → Create AEM folder structure
├── Task Completed → Trigger AEM workflow
├── Proof Approved → Update AEM asset status
├── Document Uploaded → Sync to AEM DAM
└── Project Completed → Archive AEM assets

AEM Events → Workfront Actions:
├── Asset Published → Update WF document status
├── Metadata Updated → Sync WF custom form fields
└── Workflow Completed → Update WF task status
```

## Architect Design Patterns

### Pattern 1: Creative Production Pipeline

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│ Workfront │    │ Creative  │    │   AEM    │    │  Publish  │
│ Brief     │ →  │ Work in   │ →  │ Asset    │ →  │  to       │
│           │    │ Workfront │    │ in DAM   │    │ Channels  │
└──────────┘    └──────────┘    └──────────┘    └──────────┘
     │                │                │               │
     ▼                ▼                ▼               ▼
  Request        Task assign       Linked folder    Dynamic Media
  Queue          Proofing          Metadata sync    Experience Frag
  Approval       Review cycle      AEM Workflow     CDN delivery
```

### Pattern 2: Content Governance

```
Governance Model:
├── Workfront: Project management, approvals, timelines
├── AEM: Content creation, asset management, delivery
├── Integration: Metadata bridges the two systems
│
│   Workfront custom form "Content Metadata"
│   ├── Brand (dropdown) → mapped to AEM dam:brand
│   ├── Campaign (typeahead) → mapped to AEM dam:campaign
│   ├── Usage Rights (text) → mapped to AEM dam:usageTerms
│   ├── Expiry Date (date) → mapped to AEM prism:expirationDate
│   └── Region (multi-select) → mapped to AEM dam:region
│
└── Governance ensures: consistent metadata, audit trail, compliance
```

---

*[Back to Integration Patterns](../README.md)*
