# AD0-E117 — Architecture Scenario Questions

[Back to AD0-E117 Index](../README.md)

---

## Scenario 1: Global E-Commerce Platform [Architect]

**Situation:** A retail company with operations in 15 countries needs to build a global web platform on AEM. Requirements:
- 15 country sites with 8 languages
- Product catalog integration with PIM system
- Personalized content based on user behavior
- 99.9% uptime SLA
- PCI DSS compliance for payment pages
- 2M monthly unique visitors per region

**Question:** Design the complete AEM architecture.

<details>
<summary>Detailed Solution</summary>

### Architecture Decision: AEM as a Cloud Service

**Rationale:** Auto-scaling handles 2M monthly UV per region, built-in CDN (Fastly), continuous updates, Adobe-managed infrastructure.

### Content Architecture

```
/content/global-retail/
├── master-en/        (Blueprint - English Master)
│   ├── home/
│   ├── products/     (CF-based, fed from PIM)
│   ├── categories/
│   └── support/
├── en-us/            (Live Copy + local customization)
├── en-gb/            (Live Copy)
├── fr-fr/            (Language Copy from en)
├── de-de/            (Language Copy from en)
├── ja-jp/            (Language Copy from en)
└── ... (15 countries)
```

### Integration Architecture

```
┌──────────────┐  GraphQL    ┌──────────────┐
│ PIM System    │ ──────────▶ │ AEM Content  │
│ (Products)    │             │ Fragments    │
└──────────────┘             └──────────────┘

┌──────────────┐  REST API   ┌──────────────┐
│ Adobe Target  │ ◀────────▶ │ AEM Sites    │
│ (Personalize) │             │ (Experience  │
└──────────────┘             │  Fragments)  │
                              └──────────────┘

┌──────────────┐  Redirect   ┌──────────────┐
│ Payment GW    │ ◀────────▶ │ External PCI │
│ (PCI DSS)     │             │ Compliant    │
└──────────────┘             │ Payment Page │
                              └──────────────┘
```

### Security Design
- **Author:** Adobe IMS authentication
- **Publish:** SAML 2.0 SSO for account management
- **PCI DSS:** Payment handled by external PCI-compliant service (AEM pages redirect to payment gateway — AEM itself NOT in PCI scope)
- **Dispatcher:** Full security hardening per checklist
- **CDN:** Fastly with WAF rules enabled

### Performance Design
- CDN: Fastly (built-in with AEMaaCS)
- Dispatcher: statfileslevel=3 (country-level invalidation)
- Product pages: Cache with 5-min TTL + stale-while-revalidate
- Personalized content: Client-side via Adobe Target (page shell cached)
- Images: Dynamic Media for responsive delivery
</details>

---

## Scenario 2: Enterprise Intranet Migration [Architect]

**Situation:** A corporation has an existing AEM 6.4 intranet with:
- 500 content authors
- 50,000 pages
- Custom workflows for content approval
- LDAP integration for authentication
- Heavy use of reverse replication for user-generated content
- Custom OSGi bundles (30+)

They want to migrate to AEM as a Cloud Service.

**Question:** Develop the migration strategy and identify risks.

<details>
<summary>Detailed Solution</summary>

### Phase 1: Assessment (4 weeks)

```
Assessment Activities:
├── Run Best Practices Analyzer (BPA)
│   ├── Identify incompatible code
│   ├── Flag deprecated APIs
│   └── Document content issues
├── Custom Code Audit
│   ├── 30 custom bundles → review each
│   ├── Identify admin session usage
│   ├── Check for reverse replication dependencies
│   └── Catalog OSGi configuration approach
├── Reverse Replication Analysis
│   ├── CRITICAL: Not supported in AEMaaCS
│   ├── Identify all UGC features
│   └── Design external storage alternatives
└── Author Scaling Assessment
    ├── 500 authors → AEMaaCS auto-scaling handles this
    └── Validate with load testing
```

### Phase 2: Architecture Redesign (6 weeks)

**Reverse Replication Replacement:**
```
Current: User comment → Publish JCR → Reverse replication → Author JCR
New:     User comment → External service (API) → Database → Read via AJAX

Options:
├── Custom microservice + database (MongoDB/DynamoDB)
├── Third-party service (Disqus, custom)
└── Adobe I/O Events + custom function
```

**Authentication Migration:**
```
Current: LDAP integration (direct)
New (AEMaaCS):
├── Author: Adobe IMS (mandatory)
├── Publish: SAML 2.0 with corporate IdP
│   └── LDAP → IdP → SAML → AEM
└── Group mapping via SAML attributes
```

**Custom Code Remediation:**
```
For each of 30 bundles:
├── Replace admin sessions → service users
├── Replace replication API → Sling Distribution API
├── Replace Felix Console configs → codebase configs
├── Replace custom run modes → env variables
├── Test with AEMaaCS SDK locally
└── Validate in Cloud Manager pipeline
```

### Phase 3: Content Migration (4 weeks)

```
Content Transfer:
├── Pre-migration: offline compaction (50K pages = large repo)
├── Phase 1: Full extraction → ingestion
├── Phase 2: Top-up (delta) for changes during migration
├── Validation: Content verification report
└── Parallel: Set up redirects for any URL changes
```

### Phase 4: Testing & Go-Live (4 weeks)

### Risk Register

| Risk | Impact | Mitigation |
|---|---|---|
| Reverse replication incompatibility | High | External UGC service (designed in Phase 2) |
| Custom bundle failures | High | Early SDK testing, phased bundle migration |
| 500 authors: training | Medium | Phased rollout, author training program |
| Content migration data loss | High | Validation scripts, parallel running period |
| Performance regression | Medium | Load testing against AEMaaCS before go-live |
</details>

---

## Scenario 3: Multi-Brand Content Platform [Architect]

**Situation:** A media company operates 5 distinct brands, each with its own website. Requirements:
- Shared content components across brands
- Brand-specific styling and templates
- Central DAM for all brands
- Each brand has its own editorial team
- Content sharing between brands for syndicated articles
- Performance: 500ms TTFB for all pages

**Question:** Design the content architecture and template strategy.

<details>
<summary>Detailed Solution</summary>

### Content Architecture

```
/content/
├── brand-a/
│   ├── en/ (Brand A English)
│   └── fr/ (Brand A French)
├── brand-b/
│   ├── en/
│   └── de/
├── brand-c/
│   └── en/
├── brand-d/
│   └── en/
├── brand-e/
│   └── en/
└── shared/           (Syndicated content)
    └── articles/     (Content Fragments)
```

### Configuration Architecture (Context-Aware)

```
/conf/
├── brand-a/
│   ├── settings/wcm/templates/    (Brand A templates)
│   ├── sling:configs/
│   │   └── com.brand.SiteConfig
│   │       ├── logo: "/content/dam/brand-a/logo.svg"
│   │       ├── primaryColor: "#FF0000"
│   │       ├── analyticsId: "UA-BRAND-A"
│   │       └── socialLinks: [...]
│   └── settings/cloudconfigs/
├── brand-b/
│   └── ... (Brand B config)
├── brand-c/
│   └── ...
├── shared/
│   └── settings/wcm/templates/    (Shared templates)
└── global/
    └── settings/                   (Global defaults)
```

### Component Strategy

```
/apps/
├── shared/
│   └── components/           (Shared across all brands)
│       ├── page/             (Base page, proxies Core Component)
│       ├── header/           (Configurable header)
│       ├── footer/           (Configurable footer)
│       ├── article/          (Article component)
│       └── navigation/       (Site navigation)
├── brand-a/
│   └── components/           (Brand A specific)
│       ├── page/             (extends shared/page)
│       └── hero/             (Brand A unique component)
└── clientlibs/
    ├── shared/               (Shared CSS/JS)
    ├── brand-a/              (Brand A theme)
    ├── brand-b/              (Brand B theme)
    └── ...
```

### Template Strategy

| Template | Location | Used By |
|---|---|---|
| Article Page | /conf/shared | All brands |
| Landing Page | /conf/shared | All brands |
| Home Page | /conf/{brand} | Brand-specific |
| Product Page | /conf/{brand} | Brand-specific |

### Content Sharing via Content Fragments

```
Syndicated Article Flow:
1. Author creates article as Content Fragment in /content/dam/shared/articles/
2. CF Model: title, body, author, category, tags
3. Brand sites reference CF via Content Fragment component
4. Each brand renders CF with own styling (Style System)
5. GraphQL API available for headless channels

Benefit: Single source of truth, multiple presentations
```

### Performance Architecture (500ms TTFB)

```
Performance Stack:
├── CDN: Cache HTML at edge (5-min TTL)
│   └── 95% of requests served from CDN cache
├── Dispatcher: statfileslevel=2 (per brand/language)
│   └── Remaining 5% served from Dispatcher cache
├── AEM: Optimized queries with custom indexes
│   └── < 1% requests reach AEM publish
├── Images: Dynamic Media (on-demand renditions)
├── Client Libraries: Versioned, immutable cache (1 year)
└── Monitoring: New Relic APM for TTFB tracking
```
</details>

---

## Scenario 4: Headless + Hybrid Architecture [Architect]

**Situation:** A technology company needs:
- Marketing website (traditional AEM)
- Developer documentation portal (static site)
- Mobile app (native iOS/Android)
- Interactive product configurator (SPA)
- All consuming the same product content

**Question:** Design a headless/hybrid architecture.

<details>
<summary>Detailed Solution</summary>

### Architecture Overview

```
                    ┌──────────────────────┐
                    │    AEM Author         │
                    │  (Content Hub)        │
                    └──────────┬───────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
    ┌─────────▼──────┐ ┌──────▼──────┐ ┌──────▼──────┐
    │ AEM Publish     │ │ GraphQL API │ │ Assets API  │
    │ (Full Pages)    │ │ (Headless)  │ │ (CF/Assets) │
    └─────────┬──────┘ └──────┬──────┘ └──────┬──────┘
              │                │                │
    ┌─────────▼──────┐ ┌──────▼──────┐ ┌──────▼──────┐
    │ Marketing Site  │ │ Mobile App  │ │ Dev Docs    │
    │ (Server-render) │ │ (Native)    │ │ (Static Gen)│
    └────────────────┘ └─────────────┘ └─────────────┘
              │
    ┌─────────▼──────┐
    │ Product Config  │
    │ (SPA Editor)    │
    └────────────────┘
```

### Content Model

```
Content Fragments (Headless):
├── Product (CF Model)
│   ├── name, description, specs, pricing
│   ├── images (DAM references)
│   └── Consumed by: all 4 channels
├── Documentation Article (CF Model)
│   ├── title, body (markdown), category, version
│   └── Consumed by: dev docs portal, mobile app
└── FAQ (CF Model)
    ├── question, answer, category
    └── Consumed by: marketing site, mobile app

Pages (Full-Stack):
├── Marketing pages (Editable Templates + Core Components)
└── Product Configurator (SPA Editor + React)
```

### Delivery Pattern Per Channel

| Channel | Delivery | Technology |
|---|---|---|
| Marketing Site | Full-stack AEM | Editable Templates, Dispatcher cache |
| Mobile App | GraphQL API | CF + persisted queries |
| Dev Docs Portal | Static site generation | CF export → build pipeline |
| Product Configurator | SPA Editor | React + AEM Model JSON |
</details>

---

*[Back to AD0-E117 Index](../README.md)*
