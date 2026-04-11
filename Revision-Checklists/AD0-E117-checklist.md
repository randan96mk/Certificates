# AD0-E117 — Last-Week Revision Checklist

[Back to Main README](../README.md)

---

## Exam Day Quick Facts
- **50 questions** | **100 minutes** | **58% passing (29/50)** | ~2 min/question

---

## Domain 1: Discovery (18%)

- [ ] Architecture assessment methodology
- [ ] Non-functional requirements: scalability, security, performance, availability
- [ ] Translating business goals to functional requirements
- [ ] i18n strategies: language copies, MSM, translation workflows
- [ ] Multi-site architecture patterns
- [ ] Stakeholder requirement prioritization

## Domain 2: Solution Design (44%) — CRITICAL (nearly half the exam)

### Information Architecture
- [ ] Content hierarchy design (David's Model principles)
- [ ] Content Fragments vs Experience Fragments — decision criteria
- [ ] Sling Context-Aware Configurations for multi-site config
- [ ] Content Fragment Models and GraphQL delivery
- [ ] DAM folder structure best practices
- [ ] Taxonomy and tagging architecture

### Security
- [ ] SAML 2.0 Authentication Handler — configuration and flow
- [ ] LDAP, OAuth, Adobe IMS — when to use each
- [ ] CUG (Closed User Groups) — how they work
- [ ] ACL design: deny-all default, group-based, path-level
- [ ] Service Users (replace admin sessions)
- [ ] CSRF protection framework
- [ ] Dispatcher security checklist items
- [ ] XSS prevention: HTL auto-escaping, CSP headers
- [ ] X-Frame-Options for clickjacking prevention

### Persistence & Storage
- [ ] TarMK vs MongoMK — decision criteria (< 100 vs 100+ authors)
- [ ] S3 DataStore vs File DataStore
- [ ] Binary storage strategies
- [ ] Oak compaction (online/offline)

### Cloud Architecture
- [ ] AEM 6.5 vs AEMaaCS — key differences table
- [ ] Immutable infrastructure concept
- [ ] Sling Content Distribution (replaces replication)
- [ ] No reverse replication in AEMaaCS
- [ ] Cloud Manager: environments, pipelines, quality gates
- [ ] OSGi config management (environment variables, codebase)
- [ ] Asset microservices (serverless processing)
- [ ] Auto-scaling behavior

### Integration
- [ ] Third-party REST integration with fallback (circuit breaker)
- [ ] SPA Editor architecture (React/Angular + AEM)
- [ ] Adobe Commerce integration pattern
- [ ] Dynamic Media integration
- [ ] Connected Assets (separate WCM + DAM)

### Caching & Performance
- [ ] Dispatcher caching architecture (.stat files, /statfileslevel)
- [ ] Cache invalidation: /gracePeriod, flush agents
- [ ] CDN integration (Fastly for AEMaaCS)
- [ ] Cache-Control header strategy per content type
- [ ] What is cached vs not cached

### Templates & Components
- [ ] Editable Templates vs Static Templates
- [ ] Core Components + proxy pattern
- [ ] Style System (CSS classes, no new components)
- [ ] Component versioning strategy
- [ ] HTL preferred over JSP
- [ ] Sling Models preferred over WCMUse

### Migration
- [ ] Content Transfer Tool (CTT) procedure
- [ ] Pre-migration: disable replication, datastore consistency check
- [ ] Best Practices Analyzer (BPA)
- [ ] Code migration: service users, distribution API, env variables

## Domain 3: Implementation (22%)

- [ ] Core Components implementation patterns
- [ ] Sling Model Exporter (JSON API)
- [ ] Custom workflow development
- [ ] Bundle troubleshooting (active, resolved, installed states)
- [ ] Resolving design issues during implementation
- [ ] Prototyping/POC approaches
- [ ] Sling resource resolution pipeline (URL → Resource → Script)
- [ ] /apps overlays /libs (never modify /libs)

## Domain 4: Maintenance (16%)

- [ ] Service pack procedure: staging + regression → production
- [ ] Backup strategies: online, filesystem, MongoDB
- [ ] Disaster recovery: cold standby (TarMK), Adobe-managed (AEMaaCS)
- [ ] Operations Dashboard health checks
- [ ] JVM tuning: heap sizing, G1GC
- [ ] Oak index monitoring (traversal query prevention)
- [ ] Log analysis for common issues

---

## Key Architecture Decision Tables

### Component Logic
| Choose | Over |
|---|---|
| Sling Models | WCMUse |
| HTL | JSP |
| Editable Templates | Static Templates |
| Core Components | Custom (when possible) |
| /apps overlay | Modifying /libs |

### Deployment
| Choose | Over |
|---|---|
| TarMK (< 100 authors) | MongoMK |
| MongoMK (100+ authors) | TarMK cluster |
| AEMaaCS (new projects) | On-premise |
| S3 DataStore | File DataStore (at scale) |

### Content Delivery
| Choose | Over |
|---|---|
| Content Fragments (headless) | EF for API delivery |
| Experience Fragments (omnichannel web) | CF for rendered content |
| Launches (scheduled publish) | Custom workflow |
| MSM (regional variations) | Manual content duplication |

## Red Flag Answers (Usually Wrong)

| Red Flag | Why |
|---|---|
| "Use MongoDB for publish instances" | Publish always uses TarMK |
| "Modify /libs directly" | Never — use /apps overlay |
| "Install SP directly in production" | Always stage + regress first |
| "Use reverse replication in AEMaaCS" | Not supported |
| "Store user data in JCR on publish" | Use external database |
| "Use JSP for new components" | Deprecated — use HTL |
| "Use admin session in services" | Use Service Users |
| "Unlimited traversal queries" | Always create indexes |

---

## NEW: 2025-2026 Features to Know

- [ ] **Edge Delivery Services (conceptual):** When to recommend EDS vs traditional AEM
- [ ] **Universal Editor:** New WYSIWYG editing for headless/EDS content
- [ ] **Content Fragment OpenAPI:** Programmatic CF management (2026.2.0)
- [ ] **Quiet Hours (GA):** Schedule update-free periods in AEMaaCS
- [ ] **JSON-based index management:** Simplified Oak index definitions (2026.3.0)
- [ ] **Cloud Manager MCP Server:** AI IDE interaction with Cloud Manager
- [ ] **Incremental builds:** Module-level caching in CI/CD pipeline
- [ ] **Optional publish tier:** When using Edge Delivery for delivery
- [ ] **TipTap editor:** Replacing TinyMCE for rich text (2026.3.0)
- [ ] **Java API deprecations:** March 30, 2026 deadline
- [ ] **Content Advisor:** AI-powered asset discovery in Sites
- [ ] **Automatic malware scanning:** For uploaded assets (2025.12.0)
- [ ] **Proctoring change:** Meazure Learning / ProctorU with Guardian browser
- [ ] **EDS-D200 separate cert:** AD0-E117 retains traditional AEM focus

---

*Confidence Check: Mark each item. Anything unchecked = study priority.*
