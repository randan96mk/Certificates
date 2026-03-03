# End-to-End Integration Scenarios

[Back to Integration Patterns](../README.md) | [Back to Main README](../../README.md)

---

## Scenario 1: Complete Marketing Campaign Lifecycle

### Architecture

```
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│  Workfront    │    │   Fusion     │    │     AEM      │
│  (Planning)   │◀──▶│  (Orchestr.) │◀──▶│  (Delivery)  │
└──────┬───────┘    └──────┬───────┘    └──────┬───────┘
       │                   │                   │
       ▼                   ▼                   ▼
   Campaign Plan      Automation         Content Delivery
   Resource Mgmt      Integration        Asset Management
   Approvals          Data Sync          Personalization
   Time Tracking      Notifications      Analytics
```

### Workflow

```
Phase 1: Campaign Initiation (Workfront)
├── Marketing Manager submits campaign request via Request Queue
├── Creative Director reviews and approves
├── Approved → Fusion triggers project creation from template
├── Workfront project created with:
│   ├── Task structure (strategy, design, content, launch)
│   ├── Role-based assignments
│   ├── Budget allocation
│   ├── Milestone path
│   └── AEM linked folder (auto-created via integration)

Phase 2: Content Creation (AEM + Workfront)
├── Designers upload assets to AEM linked folder
├── Assets appear in Workfront project documents
├── Metadata syncs: WF custom fields → AEM metadata
├── Proofing workflow in Workfront:
│   ├── Stage 1: Design review
│   ├── Stage 2: Copy review
│   └── Stage 3: Client approval
├── Approved assets → AEM workflow triggers
│   └── Generate renditions, tag, categorize

Phase 3: Campaign Assembly (AEM)
├── Content authors build campaign pages in AEM
├── Experience Fragments for cross-channel content
├── Content Fragments for headless delivery
├── Personalization rules via Adobe Target
├── Preview and internal review

Phase 4: Launch (AEM + Fusion)
├── Launch date → AEM Launches auto-promote content
├── Fusion scenario:
│   ├── Updates Workfront project status to "Launched"
│   ├── Sends Slack notification to stakeholders
│   ├── Starts analytics tracking
│   └── Creates post-launch review task

Phase 5: Measurement (Workfront + Analytics)
├── Campaign performance tracked in Adobe Analytics
├── Fusion syncs KPIs back to Workfront custom fields
├── Post-launch review task in Workfront
├── Lessons learned documented
└── Project closed → assets archived in AEM
```

## Scenario 2: Asset Production with DAM Governance

### Architecture

```
Request → Produce → Review → Approve → Publish → Archive

Workfront: Request, Produce, Review
Fusion: Orchestrate transitions
AEM: Approve (metadata), Publish (delivery), Archive
```

### Detailed Flow

```
1. Request (Workfront)
   └── Request Queue: "Creative Asset Request"
       ├── Custom Form: Asset type, brand, specs, deadline
       └── Routing: Creative Team

2. Produce (Workfront)
   └── Task assigned to designer
       ├── Designer works in creative tools
       └── Uploads draft to Workfront (auto-syncs to AEM)

3. Review (Workfront Proofing)
   └── Automated proof workflow
       ├── Internal: Creative Director review
       ├── Legal: Compliance check
       └── Client: Final sign-off

4. Approve → Metadata Enrichment (Fusion + AEM)
   └── Proof approved →
       ├── Fusion updates AEM metadata:
       │   ├── dam:approvalStatus = "Approved"
       │   ├── dam:approvedBy = approver name
       │   ├── dam:approvalDate = timestamp
       │   ├── dam:usageRights from WF form
       │   └── dam:expiryDate from WF form
       └── AEM workflow triggered:
           ├── Generate Dynamic Media renditions
           ├── Apply Smart Tags
           └── Move to "Approved" folder

5. Publish (AEM)
   └── Content author references asset in AEM pages
       ├── Dynamic Media serves optimized renditions
       ├── CDN caches globally
       └── Experience Fragments use asset across channels

6. Archive (Fusion + AEM)
   └── Expiry date reached →
       ├── Fusion: Notify asset owner
       ├── AEM: Move to archive folder
       └── AEM: Restrict access
```

## Scenario 3: Multi-System Project Workspace

### Integration Map

```
┌───────────┐
│ Salesforce │──┐
│(CRM/Sales) │  │
└───────────┘  │  ┌──────────────────┐
               ├──▶│                  │
┌───────────┐  │  │   WORKFRONT      │    ┌──────────┐
│   Jira    │──┤  │ (Central Hub)    │◀──▶│   AEM    │
│(Dev Ops)  │  │  │                  │    │ (Content) │
└───────────┘  │  └────────┬─────────┘    └──────────┘
               │           │
┌───────────┐  │  ┌────────▼─────────┐
│  Slack    │──┤  │    FUSION        │
│(Comms)    │  │  │ (Orchestrator)   │
└───────────┘  │  └──────────────────┘
               │
┌───────────┐  │
│  Finance  │──┘
│ System    │
└───────────┘

Data Flow:
├── Salesforce Opp closed → WF project created (Fusion)
├── WF task status → Jira issue update (Fusion)
├── WF document approved → AEM asset published (Fusion + native)
├── WF milestone complete → Slack notification (Fusion)
├── WF hours logged → Finance system sync (Fusion nightly)
├── AEM asset published → WF document status update (native)
└── Jira release complete → WF task marked done (Fusion)
```

---

## Integration Best Practices for Architects

| Practice | Description |
|---|---|
| **Single source of truth** | Each data element owned by one system |
| **Async over sync** | Use webhooks/events, not polling where possible |
| **Idempotent operations** | Design for safe retries |
| **Error handling** | Every integration point needs a failure strategy |
| **Monitoring** | Log all cross-system operations |
| **Rate limits** | Respect API limits; implement throttling |
| **Data mapping** | Document all field mappings between systems |
| **Testing** | Test integration paths with realistic data volumes |

---

*[Back to Integration Patterns](../README.md)*
