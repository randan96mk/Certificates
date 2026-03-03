# AD0-E117 — Domain Breakdown & Detailed Objectives

[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E117 Index](../README.md)

---

## Domain 1: Discovery (18% — ~9 questions)

### Objectives
- **1.1:** Assess the current state of an architecture and determine non-functional technical requirements
- **1.2:** Translate high-level business goals to functional requirements; design scalable and resilient architecture

### Key Topics
- Assessing and evaluating existing AEM architectures
- Gathering and prioritizing stakeholder requirements
- Translating business goals into functional requirements
- Defining non-functional requirements (scalability, security, performance, availability)
- Internationalization (i18n) function and use cases
- Strategies for multi-site and multi-language

### What to Study
- Architecture assessment frameworks
- NFR documentation patterns
- AEM i18n capabilities and multi-site strategies
- Blueprint-to-live-copy patterns for globalization

---

## Domain 2: Solution Design (44% — ~22 questions) **CRITICAL**

### Objectives
- **2.1:** Incorporate integration requirements; determine performance and testing requirements
- **2.2:** Design detailed architecture and security solutions from business requirements
- **2.3:** Recommend migration strategies; prototype solutions for proof of concepts

### Key Topics

#### Information Architecture
- Information models for AEM applications
- Content Fragments vs Experience Fragments
- Content hierarchy design (David's Model)
- Sling Context-Aware Configurations

#### Security Architecture
- Authentication models: SAML 2.0, LDAP, OAuth, Adobe IMS
- Authorization: CUGs, ACLs, service users
- CSRF protection, XSS prevention
- Dispatcher security checklist

#### Persistence & Storage
- TarMK vs MongoMK selection criteria
- Binary storage decisions (File DataStore, S3 DataStore)
- Cloud-focused: auto-scaling, binaryless replication
- On-premises vs AMS vs AEMaaCS differences

#### Integration Design
- Third-party REST service integration with fallbacks
- SSO integration patterns
- Dynamic Media integration
- Adobe Commerce, Target, Launch integrations
- SPA Editor architecture

#### Caching & Performance
- Dispatcher caching strategies
- CDN integration (Fastly for AEMaaCS)
- Load balancing architecture
- Browser caching policies

#### Templates & Components
- When to use Core Components vs custom
- Editable Templates vs Static Templates
- Component versioning and backward compatibility
- Style System for visual variations

#### Migration
- Content Transfer Tool (for AEMaaCS migration)
- Migration pre-requirements (disable replication, datastore check)
- Upgrade strategies (in-place, side-by-side)

#### Cloud Considerations
- Auto-scaling implications
- Immutable infrastructure model
- Cloud Manager pipelines
- Package management in AEMaaCS

---

## Domain 3: Implementation (22% — ~11 questions)

### Objectives
- **3.1:** Identify and resolve design issues during implementation
- **3.2:** Diagnose and resolve implementation issues; recommend best practices

### Key Topics
- AEM component implementation (Editable Templates, Core Components)
- Developing reusable components and custom workflow models
- Resolving design issues during implementation
- Troubleshooting bundle, configuration, and deployment issues
- Prototyping solutions for proof of concepts
- HTL best practices (preferred over JSP)
- Sling Models (preferred over WCMUse)

---

## Domain 4: Maintenance (16% — ~8 questions)

### Objectives
- Diagnose and resolve production issues
- Service pack installation and regression testing
- Disaster recovery and business continuity
- New feature planning and optimization
- Monitoring and maintaining AEM instances
- Performance optimization (caching, load balancing, front-end)

### Key Topics
- Service pack update procedures (staging + regression first)
- Backup and restore strategies
- Operations Dashboard health checks
- TarMK vs Oak topology considerations
- Monitoring tools and alerting
- JVM tuning and garbage collection

---

## Study Priority Matrix

| Priority | Domain | Weight | Recommendation |
|---|---|---|---|
| **P0 — Critical** | Solution Design | 44% | Master thoroughly — nearly half the exam |
| **P1 — High** | Implementation | 22% | Deep hands-on knowledge required |
| **P1 — High** | Discovery | 18% | Architecture assessment skills |
| **P2 — Medium** | Maintenance | 16% | Operations and troubleshooting |

---

## Community-Sourced Exam Tips

1. **Questions are scenario-based** — "As an Architect, what do you recommend?"
2. **Multiple correct answers exist** — Pick the BEST per Adobe best practices
3. **Heavy on AEM 6.5 and AEMaaCS** — Know the differences cold
4. **Cloud Manager**: Understand conceptually (why/when), not hands-on CLI
5. **Dispatcher/caching** is heavily tested
6. **Don't attempt without real project experience** — Memorization won't pass this exam
7. **Time pressure**: 2 min/question — practice pacing

---

*[Back to Exam Overview](./exam-overview.md) | [Back to AD0-E117 Index](../README.md)*
