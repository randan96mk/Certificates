# AD0-E117 — Extended Practice Questions (Internet-Sourced)

[Back to AD0-E117 Index](../README.md) | [Back to Practice QA](./README.md)

---

> **Source:** These questions are compiled from publicly available certification preparation resources, Adobe community forums, official readiness materials, and recent community discussions.

---

## Domain 1: Discovery (18%)

### Q1 [Architect]
A client operates in the financial services industry with strict regulatory requirements. They need content approval workflows, audit trails, and guaranteed content versioning. Which AEM feature combination addresses these requirements?

- A) MSM with Live Copies and automatic rollout
- B) Workflows with approval steps, versioning enabled on all pages, and audit logging via Operations Dashboard
- C) Content Fragments with GraphQL delivery
- D) Edge Delivery Services with document-based authoring

<details><summary>Answer</summary>

**B) Workflows with approval steps, versioning enabled on all pages, and audit logging via Operations Dashboard**

Regulatory requirements need:
- **Approval workflows:** Multi-step, role-based content approval before publication
- **Versioning:** Every page change creates a version (rollback capability)
- **Audit trail:** Operations Dashboard tracks who changed what and when
- **Access control:** ACLs restrict who can create, edit, approve, publish

MSM is for content reuse, not governance. CFs/GraphQL is for headless delivery. EDS doesn't provide the deep workflow capabilities needed.
</details>

### Q2 [Architect]
During discovery, you identify that a client needs to support 200 concurrent content authors across 3 time zones, with real-time collaborative editing. Which topology is most appropriate?

- A) TarMK with Cold Standby
- B) MongoMK with MongoDB Replica Set across regions
- C) AEM as a Cloud Service with auto-scaling
- D) Content sharding with regional author instances

<details><summary>Answer</summary>

**C) AEM as a Cloud Service with auto-scaling**

200 concurrent authors exceeds typical TarMK capacity (< 100). While MongoMK could handle this, AEMaaCS is the recommended approach for new implementations because:
- Auto-scales author tier based on demand
- Supports collaborative editing natively
- No infrastructure management burden
- Built-in multi-region CDN

Content sharding would isolate regions (preventing cross-region collaboration), which contradicts the real-time collaboration requirement.
</details>

### Q3 [Advanced]
Which of the following are valid non-functional requirements an Architect should document during discovery? (Select three)

- A) The site must support 10,000 concurrent users with < 2s page load
- B) The homepage must have a carousel component
- C) All content changes must be auditable for 7 years
- D) The system must achieve 99.9% uptime
- E) The product catalog must display 500 products per category

<details><summary>Answer</summary>

**A) The site must support 10,000 concurrent users with < 2s page load** (Performance NFR)
**C) All content changes must be auditable for 7 years** (Compliance NFR)
**D) The system must achieve 99.9% uptime** (Availability NFR)

B is a functional requirement (specific UI element). E is a functional requirement (specific feature behavior). NFRs define HOW the system should perform, not WHAT it should do.
</details>

---

## Domain 2: Solution Design (44%)

### Q4 [Architect]
A university produces multiple courses each semester. Each course consists of multiple pages, some common among courses. Each version must be archived. What primary AEM feature should the Architect recommend?

- A) Content Fragments with versioning
- B) Multi-Site Manager (MSM) with Live Copies
- C) Page versioning with Timewarp
- D) Launches for each semester

<details><summary>Answer</summary>

**B) Multi-Site Manager (MSM) with Live Copies**

MSM handles the "shared base + customization" pattern:
- **Blueprint:** Master course template with common pages
- **Live Copies:** Per-course instances that inherit common pages
- **Local customization:** Course-specific pages break inheritance
- **Archiving:** Version history + point-in-time snapshots via Timewarp
- **Each semester:** Create new Live Copies from updated Blueprint

CFs don't provide page-level structure. Versioning alone doesn't handle content reuse. Launches are for scheduling, not content reuse.
</details>

### Q5 [Architect]
An architect needs to design authentication for a multi-brand AEM publish tier. Brand A requires enterprise SSO (Okta). Brand B requires social login (Google/Facebook). Brand C is fully public. What authentication architecture should be recommended?

- A) SAML 2.0 for all brands
- B) Separate publish instances per brand
- C) Adobe Granite Authentication Handler chain with brand-specific paths and SAML for Brand A, OAuth for Brand B, and anonymous for Brand C
- D) CUG on all pages with different groups per brand

<details><summary>Answer</summary>

**C) Adobe Granite Authentication Handler chain with brand-specific paths and SAML for Brand A, OAuth for Brand B, and anonymous for Brand C**

AEM supports chained authentication handlers:
- Configure SAML handler for `/content/brand-a/*` paths → Okta IdP
- Configure OAuth handler for `/content/brand-b/*` paths → Google/Facebook
- No authentication required for `/content/brand-c/*` paths
- Each handler activates based on the request path
- CUGs can additionally restrict content within authenticated brands
</details>

### Q6 [Architect]
A client has an existing REST API that returns product data. They want to display this data on AEM pages with caching. The API has a 99.5% SLA and the AEM site requires 99.9%. What integration pattern should the Architect design?

- A) Direct server-side API call from Sling Model
- B) Sling Model with circuit breaker pattern, fallback to cached data, and client-side AJAX for freshness
- C) Import all API data into JCR nightly
- D) Use Edge Side Includes (ESI) at the Dispatcher level

<details><summary>Answer</summary>

**B) Sling Model with circuit breaker pattern, fallback to cached data, and client-side AJAX for freshness**

Compound SLA: 99.5% × 99.9% ≈ 99.4% (below requirement). Solution:
- **Circuit breaker:** Prevents cascading failures; opens after N failures
- **Fallback:** Serve cached/default data when API is down (maintains 99.9%)
- **AJAX loading:** Page renders from Dispatcher cache; product data loads async
- **Timeouts:** Short timeouts prevent slow API responses from blocking page load
- **Dispatcher caching:** Cache the page shell; only dynamic data is uncached
</details>

### Q7 [Advanced]
Which persistence model should an Architect select for a deployment with 500GB of binary assets (images, PDFs, videos)?

- A) Default File DataStore on local disk
- B) S3 DataStore with binary-less replication
- C) MongoDB for all content including binaries
- D) Store binaries in a CDN directly

<details><summary>Answer</summary>

**B) S3 DataStore with binary-less replication**

For large binary volumes:
- **S3 DataStore:** Offloads binaries to scalable cloud storage
- **Binary-less replication:** Only references are replicated (not full binaries), drastically reducing replication load
- **Scalability:** S3 scales to petabytes without impacting repository performance
- **Cost:** S3 storage is cheaper than local SSD/disk expansion

File DataStore works for small deployments but doesn't scale. MongoDB is for author scaling, not binary storage. CDN serves binaries but doesn't replace the datastore.
</details>

### Q8 [Architect]
A client needs to implement a commenting feature on their AEM as a Cloud Service publish tier where end users can post comments on articles. What architecture should the Architect recommend?

- A) Store comments as child nodes under article pages in JCR
- B) Use reverse replication to sync comments from publish to author
- C) Integrate a third-party commenting service with client-side JavaScript
- D) Use AEM Communities for comment management

<details><summary>Answer</summary>

**C) Integrate a third-party commenting service with client-side JavaScript**

Critical constraints in AEMaaCS:
- **No reverse replication** — Comments cannot flow from publish to author
- **No AEM Communities** — Removed from AEMaaCS
- **JCR on publish** — Not designed for user-generated content at scale
- **Auto-scaling** — UGC in JCR wouldn't sync across scaled instances

Solution: Third-party service (Disqus, custom microservice) stores comments in an external database, loaded via client-side AJAX. The AEM page shell is cached; comments are dynamic.
</details>

### Q9 [Advanced]
When should an Architect recommend Content Fragments over Experience Fragments? (Select two)

- A) When content needs to be delivered as JSON via API
- B) When content needs a specific visual layout for reuse across pages
- C) When the same content must be consumed by a mobile app and a website
- D) When content needs to be exported to Adobe Target for A/B testing

<details><summary>Answer</summary>

**A) When content needs to be delivered as JSON via API** — CFs are headless; EFs are rendered HTML
**C) When the same content must be consumed by a mobile app and a website** — CFs provide structured data that any channel can render differently

B → Experience Fragment (includes layout)
D → Experience Fragment (exported with layout to Target)
</details>

### Q10 [Architect]
A customer has 30+ language sites and uses "Push on modify" rollout configuration. Authors report severe performance degradation when editing blueprint pages. What should the Architect recommend?

- A) Upgrade to MongoMK for better write performance
- B) Replace "Push on modify" with scheduled rollout, and remove the referencesUpdate action
- C) Add more publish instances
- D) Enable async indexing

<details><summary>Answer</summary>

**B) Replace "Push on modify" with scheduled rollout, and remove the referencesUpdate action**

"Push on modify" triggers on every blueprint change, cascading to 30+ Live Copies:
- Each change → 30+ rollout operations
- `referencesUpdate` action (most expensive) scans all references across the tree
- Combined effect: author becomes unresponsive

Fix:
1. Remove "Push on modify" → use manual/scheduled rollout
2. Remove `referencesUpdate` from rollout config (add only when genuinely needed)
3. Stagger rollouts across regions to distribute load
4. Consider if content sharding is appropriate for truly independent regions
</details>

### Q11 [Advanced]
What is the correct approach for implementing custom Oak indexes in AEM as a Cloud Service?

- A) Create indexes via CRXDE on the author instance
- B) Define indexes in the project codebase and deploy via Cloud Manager CI/CD pipeline
- C) Use the Felix Console to add index definitions
- D) Request Adobe Support to create indexes

<details><summary>Answer</summary>

**B) Define indexes in the project codebase and deploy via Cloud Manager CI/CD pipeline**

In AEMaaCS (immutable infrastructure):
- No runtime changes via CRXDE or Felix Console
- Custom indexes must be defined as content packages in the codebase
- Deployed through Cloud Manager pipelines
- Validated during the code quality gate
- As of 2026.3.0, simplified JSON-based index management is available
</details>

### Q12 [Advanced]
A client wants to implement A/B testing on their AEM site using Adobe Target. Which AEM feature should the Architect use to export content variations to Target?

- A) Content Fragments with GraphQL
- B) Experience Fragments exported as Target offers
- C) MSM Live Copies for each variation
- D) Launches with different content versions

<details><summary>Answer</summary>

**B) Experience Fragments exported as Target offers**

Experience Fragments can be exported directly to Adobe Target as HTML offers:
- Create EF variations (e.g., Hero Banner v1, Hero Banner v2)
- Export to Target via cloud configuration
- Target manages the A/B test allocation
- Reporting in Target/Analytics

CFs don't include layout (needed for visual A/B tests). MSM is for content reuse, not testing. Launches are for scheduling, not A/B testing.
</details>

---

## Domain 3: Implementation (22%)

### Q13 [Advanced]
During implementation, a developer reports that a custom component works correctly in the author but displays incorrectly on publish. What should the Architect investigate first?

- A) Check if the Dispatcher is caching an old version of the page
- B) Verify that all OSGi bundles are active on publish
- C) Check if the component uses author-specific APIs that don't exist on publish
- D) All of the above, in the order listed

<details><summary>Answer</summary>

**D) All of the above, in the order listed**

Troubleshooting author-vs-publish discrepancies (priority order):
1. **Dispatcher cache:** Most common — stale cached version served. Flush and retest.
2. **Bundle status:** Ensure all bundles are Active on publish (not just author)
3. **Author-specific APIs:** Some WCM APIs behave differently or are unavailable on publish. Check `WCMMode.DISABLED` behavior.
4. **Permissions:** Service user may have different ACLs on publish
5. **Content sync:** Verify content was replicated/distributed to publish
</details>

### Q14 [Advanced]
What is the recommended approach for creating a reusable component in AEM that needs to work across multiple sites with different visual styles?

- A) Create separate components per site
- B) Use Core Components with the proxy pattern and Style System for per-site visual variations
- C) Use conditional logic in HTL to check the site and render differently
- D) Create one component with all style variations embedded

<details><summary>Answer</summary>

**B) Use Core Components with the proxy pattern and Style System for per-site visual variations**

The Style System is specifically designed for this:
- **Proxy component:** Each site has a proxy pointing to the same Core Component
- **Template policies:** Per-site Style System classes configured in template policies
- **CSS:** Site-specific clientlibs define the visual styles
- **No Java/logic duplication:** Same Sling Model, different CSS
- **Author experience:** Content authors select styles from a dropdown
</details>

### Q15 [Advanced]
A client's AEM implementation uses Sling Model Exporter to expose component data as JSON. The JSON responses include sensitive internal paths. How should the Architect address this?

- A) Disable Sling Model Exporter entirely
- B) Implement a custom Sling Model with `@Exporter` that explicitly controls which fields are exported, excluding sensitive paths
- C) Block JSON extensions at the Dispatcher
- D) Use AEM security features to encrypt the JSON

<details><summary>Answer</summary>

**B) Implement a custom Sling Model with `@Exporter` that explicitly controls which fields are exported, excluding sensitive paths**

Defense in depth:
1. **Model level:** Use `@JsonIgnore` or explicit `@JsonProperty` to control exported fields
2. **Dispatcher level:** Additionally, restrict `.json` access to only approved paths
3. **Never expose:** Internal paths, author URLs, JCR structure, user details

Option C alone is insufficient (may block legitimate JSON APIs). Option A removes useful functionality. Encryption doesn't solve the exposure of internal structure.
</details>

---

## Domain 4: Maintenance (16%)

### Q16 [Advanced]
A production AEM instance shows increasing response times over several weeks. Log analysis reveals frequent "Traversal query" warnings. What is the root cause and fix?

- A) JVM heap is too small; increase memory
- B) Queries are running without custom Oak indexes, causing full repository scans
- C) Too many publish instances; reduce the count
- D) Dispatcher cache is too large

<details><summary>Answer</summary>

**B) Queries are running without custom Oak indexes, causing full repository scans**

Traversal queries scan the entire repository (or large subtrees), degrading performance as content grows:
- **Diagnosis:** Search logs for "Traversal query" warnings
- **Root cause:** Missing custom Oak index for the query's property/path combination
- **Fix:** Create custom Lucene or Property index covering the queried properties
- **Prevention:** Use `EXPLAIN` on all custom queries to verify index usage
- **Monitoring:** Set `oak.query.limit.reads` to prevent runaway queries
</details>

### Q17 [Advanced]
An AEM 6.5 instance needs to be upgraded to the latest service pack. The operations team proposes installing directly in production during a maintenance window. What should the Architect advise?

- A) Approve the plan — maintenance windows are designed for this
- B) Reject — install in a staging environment first, run full regression tests, then deploy to production only if all tests pass
- C) Install on one publish instance first, then roll to others
- D) Wait for Adobe to release an automated upgrade tool

<details><summary>Answer</summary>

**B) Reject — install in a staging environment first, run full regression tests, then deploy to production only if all tests pass**

Service pack procedure (always):
1. Review release notes for breaking changes
2. Install in **staging** environment
3. Verify all bundles are active
4. Run **full regression test suite**
5. Test critical business workflows
6. Verify custom code compatibility
7. Only then → deploy to **production**
8. Monitor post-deployment

Never install directly in production. Never skip regression testing.
</details>

### Q18 [Architect]
A client asks about disaster recovery options for their AEM as a Cloud Service implementation. What should the Architect explain?

- A) The client must configure their own disaster recovery
- B) Adobe manages disaster recovery as part of the AEMaaCS service; the client should document RPO/RTO requirements and validate they align with Adobe's SLA
- C) Disaster recovery is not available for AEMaaCS
- D) The client needs to maintain a parallel on-premise instance

<details><summary>Answer</summary>

**B) Adobe manages disaster recovery as part of the AEMaaCS service; the client should document RPO/RTO requirements and validate they align with Adobe's SLA**

In AEMaaCS:
- Adobe manages infrastructure DR (multi-region, automatic failover)
- The client's responsibility: understand RPO/RTO in the SLA
- Content backup: Adobe manages repository backups
- Code: Stored in Git (customer-managed)
- Configuration: In codebase (deployed via Cloud Manager)
- The client should validate that Adobe's SLA meets their business continuity requirements
</details>

### Q19 [Advanced]
After deploying a new version of the codebase to AEMaaCS via Cloud Manager, authors report that some pages display incorrectly. What is the fastest way to diagnose?

- A) SSH into the author instance and check logs
- B) Check Cloud Manager logs and the Operations Dashboard health checks; review deployment quality gate results
- C) Roll back to the previous version immediately
- D) Wait for Adobe support to investigate

<details><summary>Answer</summary>

**B) Check Cloud Manager logs and the Operations Dashboard health checks; review deployment quality gate results**

In AEMaaCS (no SSH access):
1. **Cloud Manager:** Check pipeline execution logs, quality gate results
2. **Operations Dashboard:** Bundle status, health checks
3. **Log analysis:** Cloud Manager provides log access
4. **Specific errors:** Check for bundle activation failures, OSGi config issues
5. **If critical:** Cloud Manager supports rollback to previous deployment

Don't jump to rollback without diagnosis — the same issue may recur.
</details>

### Q20 [Advanced]
What are the two most critical pre-launch tests an Architect should recommend for a new AEM website? (Select two)

- A) Load testing
- B) Penetration testing
- C) Unit testing
- D) Code review
- E) User preference survey

<details><summary>Answer</summary>

**A) Load testing** — Validates the system handles expected and peak traffic without degradation. Identifies bottlenecks in caching, query performance, and infrastructure sizing.

**B) Penetration testing** — Identifies security vulnerabilities before the site is publicly accessible. Tests: XSS, injection, authentication bypass, Dispatcher filter gaps, sensitive path exposure.

Unit testing and code review happen during development (pre-launch testing is specifically about system-level validation). User surveys are not technical testing.
</details>

---

*Total: 20 additional questions. Combined with the original 15, the AD0-E117 bank now has 35 practice questions.*

*[Back to AD0-E117 Index](../README.md)*
