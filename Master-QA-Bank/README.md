# Master Q&A Bank — Cross-Certification

[Back to Main README](../README.md)

---

## About This Bank

This consolidated Q&A bank covers topics that span both certifications and your architect role, with emphasis on integration, architecture, and decision-making.

---

## Architecture Decision Questions

### Q1 [Architect] — Cross-Certification
Your organization uses both AEM and Workfront. A creative team needs to produce marketing assets with approval workflows in Workfront and publish them via AEM. What integration architecture should you design?

<details><summary>Answer</summary>

**Use the native AEM-Workfront connector with Fusion for orchestration:**

1. **Native Integration:** Linked folders sync between WF projects and AEM DAM
2. **Metadata Mapping:** WF custom fields → AEM metadata schema
3. **Proof Workflow:** Approval in WF proofing → status update in AEM
4. **Fusion Scenario:** On proof approval → trigger AEM publishing workflow
5. **AEM Delivery:** Dynamic Media serves optimized renditions via CDN

This leverages native capabilities where available and Fusion for custom orchestration.
</details>

### Q2 [Architect] — AEM
You're architecting a new AEM implementation. The client has these requirements: 50 concurrent authors, 10 country sites in 5 languages, 99.9% uptime, and needs to launch in 6 months. Which AEM deployment model do you recommend?

<details><summary>Answer</summary>

**AEM as a Cloud Service**

Reasoning:
- **50 authors:** Well within TarMK capacity (auto-scaled in AEMaaCS)
- **10 countries, 5 languages:** MSM + Language Copies, built-in CDN for global delivery
- **99.9% uptime:** Adobe SLA for AEMaaCS meets this; on-prem would require complex HA setup
- **6-month timeline:** Cloud eliminates infrastructure setup time, Cloud Manager provides CI/CD out of the box

Architecture:
- MSM: English blueprint → regional live copies
- Language Copies for 5 languages per region
- Sling Context-Aware Configs for per-country settings
- Fastly CDN for global performance
- Cloud Manager for automated deployments
</details>

### Q3 [Architect] — Workfront
A company is expanding from 50 Workfront users to 300 across 5 departments. What governance and technical recommendations do you provide?

<details><summary>Answer</summary>

**Governance:**
1. Establish change advisory board (1 representative per department)
2. Create tiered governance: system admins (2-3) → group admins (1 per dept) → power users (2-3 per dept)
3. Document naming conventions, template standards, and form guidelines
4. Create onboarding training tracks per department

**Technical:**
1. Create group per department with group admins
2. Design 1-2 new access levels if needed (aim for ≤10 total)
3. Create department-specific layout templates
4. Build department-specific project templates
5. Set up department request queues
6. Design cross-department reporting dashboards
7. Plan resource pools per department for Resource Planner
</details>

### Q4 [Architect] — AEM
Content authors report that search results are inaccurate (missing recently published content). The AEM instance uses Lucene indexes. How do you diagnose and fix this?

<details><summary>Answer</summary>

**Root Cause:** Lucene indexes are **asynchronous** — there's a delay between content changes and index updates.

**Diagnosis:**
1. Check Oak indexing logs for lag
2. Verify index definitions include the queried properties
3. Check if index reindex is needed
4. Verify property is configured as `propertyIndex=true` in the Lucene index definition

**Solutions (by use case):**
- **Need real-time accuracy:** Use Property Index (synchronous) for the specific query
- **Full-text search is required:** Keep Lucene but inform authors of indexing delay
- **Custom query:** Create a dedicated Lucene index with the needed properties, ensure `async` is properly configured
- **Large-scale:** Consider if the query is properly constrained (path, nodetype) to limit index scope
</details>

### Q5 [Architect] — Both
An organization wants to track creative asset production from request to publication across Workfront, AEM, and Adobe Target. Design the end-to-end workflow.

<details><summary>Answer</summary>

```
1. REQUEST (Workfront)
   └── Request Queue → Creative Brief form
       └── Routing: Creative Team

2. PRODUCE (Workfront + AEM)
   └── Tasks: Design → Copy → Review
       └── Assets uploaded to AEM linked folder
       └── Metadata syncs from WF custom fields

3. APPROVE (Workfront Proofing)
   └── Automated proof: Internal → Legal → Client
       └── On approval: Fusion updates AEM metadata

4. PREPARE (AEM)
   └── Content author builds page/Experience Fragment
       └── References approved assets from DAM
       └── Creates variations for A/B testing

5. TARGET (AEM + Adobe Target)
   └── Experience Fragment exported to Target
       └── A/B test configured in Target
       └── Personalization rules defined

6. PUBLISH (AEM)
   └── Launch scheduled for go-live date
       └── CDN cache warmed
       └── Fusion notifies stakeholders in Slack

7. MEASURE (Analytics + Workfront)
   └── Target reports A/B winner
       └── Analytics tracks engagement
       └── Fusion syncs KPIs to WF custom fields
       └── Post-mortem task completed in WF
```
</details>

---

## Quick-Fire Questions

### Workfront Quick-Fire

| # | Question | Answer |
|---|---|---|
| 1 | What's the API code for Issues? | `OPTASK` (not "issue") |
| 2 | Max baselines per project? | 10 |
| 3 | Duration type where adding people shortens timeline? | Effort Driven |
| 4 | Wildcard for current user's home group? | `$$USER.homeGroupID` |
| 5 | What is a "resolving object"? | Task/Project/Issue that auto-resolves an issue on completion |
| 6 | Text mode: merge two columns? | `sharecol=true` on first column |
| 7 | Proof role that can markup + decide? | Reviewer & Approver |
| 8 | Which update type recalculates timelines in real time? | Automatic and On Change |
| 9 | Default task constraint when scheduling from start date? | ASAP |
| 10 | How are billing records locked? | Once marked "Billed" — hours/expenses locked |

### AEM Quick-Fire

| # | Question | Answer |
|---|---|---|
| 1 | TarMK vs MongoMK decision factor? | Number of concurrent authors (< 100 = TarMK) |
| 2 | AEMaaCS replication method? | Sling Content Distribution (not agents) |
| 3 | Is reverse replication supported in AEMaaCS? | **No** |
| 4 | Preferred component logic pattern? | Sling Models (not WCMUse) |
| 5 | Preferred templating language? | HTL (not JSP) |
| 6 | What invalidates Dispatcher cache? | .stat file timestamp update |
| 7 | What is /statfileslevel? | Controls granularity of cache invalidation |
| 8 | SAML 2.0 handler PID? | `com.adobe.granite.auth.saml.SamlAuthenticationHandler` |
| 9 | Service pack installation order? | Staging + regression test → then production |
| 10 | What causes infinite component instances in heap dump? | Cyclic dependency (self-referencing component) |
| 11 | Content delivery for headless channel? | Content Fragments (via GraphQL) |
| 12 | Layout + content reuse across pages? | Experience Fragments |
| 13 | Scheduled content publishing feature? | Launches |
| 14 | How to manage per-brand config in multi-site? | Sling Context-Aware Configurations |
| 15 | Pre-CTT migration steps? | Disable replication + datastore consistency check |

---

*[Back to Main README](../README.md)*
