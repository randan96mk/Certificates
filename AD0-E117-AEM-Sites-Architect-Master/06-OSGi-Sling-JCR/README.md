# OSGi, Apache Sling & JCR Deep Dive

[Back to AD0-E117 Index](../README.md)

---

## OSGi Framework (Apache Felix)

### Core Concepts

```
OSGi Architecture:
├── Bundles (deployment units, .jar files)
│   ├── Active — running normally
│   ├── Resolved — dependencies met, not started
│   ├── Installed — loaded but dependencies unresolved
│   └── Fragment — extends a host bundle
├── Services (runtime service registry)
│   ├── Register services from bundles
│   ├── Discover services by interface
│   └── Dynamic: services can come and go
├── Life Cycle (bundle state management)
│   └── install → resolve → start → stop → uninstall
└── Configuration (ConfigAdmin)
    ├── PID-based configuration
    ├── Factory configurations
    └── Run mode-based configs
```

### OSGi Configuration Management

```
Configuration Locations:
├── AEM 6.5 (On-Premise/AMS)
│   ├── /apps/myproject/config/           (all run modes)
│   ├── /apps/myproject/config.author/    (author only)
│   ├── /apps/myproject/config.publish/   (publish only)
│   ├── /apps/myproject/config.dev/       (dev run mode)
│   └── /apps/myproject/config.author.dev/ (author + dev)
│
└── AEM as a Cloud Service
    ├── Must be in codebase (no runtime changes)
    ├── Environment variables for sensitive values
    ├── config/ (all environments)
    ├── config.author/ (author tier)
    ├── config.publish/ (publish tier)
    ├── config.dev/ (dev environment)
    ├── config.stage/ (stage environment)
    └── config.prod/ (production environment)
```

### OSGi Configuration: AEM 6.5 vs AEMaaCS

| Aspect | AEM 6.5 | AEMaaCS |
|---|---|---|
| Runtime config via Felix Console | Yes | **No** (immutable) |
| Config file location | Repository or filesystem | **Codebase only** |
| Sensitive values | Plain text or crypto | **Environment variables** |
| Deployment | Manual or package | **CI/CD pipeline** |
| Run modes | Custom run modes supported | **Fixed**: dev, stage, prod |

### Key OSGi Services to Know

| Service | PID | Purpose |
|---|---|---|
| Apache Sling Referrer Filter | `org.apache.sling.security.impl.ReferrerFilter` | CSRF protection |
| Apache Sling Resource Resolver | `org.apache.sling.jcr.resource.internal.JcrResourceResolverFactoryImpl` | URL mapping |
| Day CQ Link Externalizer | `com.day.cq.commons.impl.ExternalizerImpl` | External URL generation |
| Adobe Granite SAML Handler | `com.adobe.granite.auth.saml.SamlAuthenticationHandler` | SAML SSO |
| Apache Sling Service User Mapper | `org.apache.sling.serviceusermapping.impl.ServiceUserMapperImpl` | Service user mapping |

---

## Apache Sling

### Resource Resolution

```
Request Processing Pipeline:
1. URL → Resource Resolution
   /content/mysite/en/page.html
   └── resource: /content/mysite/en/page
   └── selector: (none)
   └── extension: html

2. Resource → Script Resolution
   sling:resourceType = "myproject/components/page"
   Search path: /apps → /libs
   └── /apps/myproject/components/page/page.html

3. Script → Rendering
   HTL template processes Sling Model
   └── HTML output returned to client
```

### Sling URL Decomposition

```
/content/mysite/en/page.selector1.selector2.html/suffix?key=value

Parts:
├── Resource Path: /content/mysite/en/page
├── Selectors: selector1, selector2
├── Extension: html
├── Suffix: /suffix
└── Query: key=value
```

### Sling Resource Type Hierarchy

```
Component Inheritance:
/apps/myproject/components/page
  ├── sling:resourceType = "myproject/components/page"
  ├── sling:resourceSuperType = "core/wcm/components/page/v3/page"
  └── Falls back to: /libs/core/wcm/components/page/v3/page

Search Path Priority:
  1. /apps (project overlays)
  2. /libs (product defaults)
  → /apps ALWAYS takes precedence over /libs
```

### Sling Models (Preferred Pattern)

```java
@Model(adaptables = Resource.class,
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
public class HeroComponent {

    @ValueMapValue
    private String title;

    @ValueMapValue
    private String description;

    @ChildResource
    private Resource image;

    // Getters...
}
```

### Sling Model Exporter (JSON API)

```java
@Model(adaptables = SlingHttpServletRequest.class,
       resourceType = "myproject/components/hero",
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
@Exporter(name = "jackson", extensions = "json")
public class HeroModel {
    // Automatically exports as JSON at:
    // /content/mysite/en/page/jcr:content/hero.model.json
}
```

### Sling Context-Aware Configurations

```
Use Case: Multi-site configuration management

Structure:
/conf/brand-a/sling:configs/myproject/SiteConfig
  ├── headerLogo = "/content/dam/brand-a/logo.png"
  ├── primaryColor = "#FF0000"
  └── analyticsId = "UA-12345"

/conf/brand-b/sling:configs/myproject/SiteConfig
  ├── headerLogo = "/content/dam/brand-b/logo.png"
  ├── primaryColor = "#0000FF"
  └── analyticsId = "UA-67890"

Content links to config:
/content/brand-a-site/jcr:content
  └── sling:configRef = "/conf/brand-a"

Usage: caConfigManager.get(resource).as(SiteConfig.class)
```

### Sling Content Distribution (AEMaaCS)

```
Traditional Replication (AEM 6.5):
  Author ──replication agent──▶ Publish

Sling Content Distribution (AEMaaCS):
  Author ──publish event──▶ Distribution Pipeline
    └── Subscriber 1 (Publish Instance 1)
    └── Subscriber 2 (Publish Instance 2)
    └── Subscriber N (Auto-scaled instances)

Key Differences:
├── Event-based (not agent-based)
├── Supports auto-scaling (new instances auto-subscribe)
├── No reverse replication
├── Binaryless by default (binaries in shared blob store)
└── More reliable (guaranteed delivery)
```

---

## JCR (Java Content Repository) — Apache Jackrabbit Oak

### David's Model (Content Modeling Principles)

| Principle | Description |
|---|---|
| **Content is king** | Drive hierarchy by content structure, not application |
| **Data first** | Design content model before code |
| **No same-name siblings** | Each node has a unique name at its level |
| **References are harmful** | Avoid cross-path references (use paths/strings) |
| **IDs are evil** | Don't rely on UUIDs; use paths |
| **Mixins are key** | Use `mix:` node types for cross-cutting metadata |
| **Build for humans** | Structure should be intuitive and navigable |

### JCR Node Types

```
Common Node Types:
├── nt:unstructured — flexible, no schema (most common for AEM content)
├── nt:folder — basic folder structure
├── sling:Folder — Sling-managed folder
├── sling:OrderedFolder — ordered children
├── cq:Page — AEM page (contains jcr:content child)
├── cq:PageContent — page content node
├── dam:Asset — DAM asset
├── dam:AssetContent — asset metadata
└── rep:User / rep:Group — user management

Common Mixins:
├── mix:title — adds jcr:title, jcr:description
├── mix:created — adds jcr:created, jcr:createdBy
├── mix:lastModified — adds jcr:lastModified, jcr:lastModifiedBy
├── mix:versionable — enables versioning
└── rep:AccessControllable — enables ACL management
```

### Oak Indexing

```
Index Types:
├── Property Index
│   ├── Fast for equality queries (WHERE property = value)
│   ├── Configuration: /oak:index/myPropertyIndex
│   └── type = "property"
├── Lucene Index (Full-text + Property)
│   ├── Full-text search and complex queries
│   ├── Configuration: /oak:index/myLuceneIndex
│   ├── type = "lucene"
│   └── Most flexible, recommended for custom queries
└── Solr Index (External)
    ├── External Solr instance
    └── Rarely used; Lucene preferred

Key Rules:
• ALWAYS create indexes for custom queries
• Traversal queries are DANGEROUS (scan entire repo)
• Use Query Builder with index-backed properties
• In AEMaaCS: custom indexes deployed via CI/CD
```

### Traversal Query Prevention

```
DANGER: Traversal Query
  → Scans entire repository
  → Causes performance degradation
  → Can crash instances under load

Prevention:
├── Create custom Oak indexes for all queries
├── Monitor traversal queries in logs
│   └── org.apache.jackrabbit.oak.query: "Traversal query"
├── Set oak.query.limit.reads to prevent runaway queries
└── Use EXPLAIN to verify query uses index

Example: Query Builder with Index
  path=/content/mysite
  type=cq:Page
  property=jcr:content/jcr:title
  property.value=My Page
  p.limit=100
  → Ensure a Lucene index covers these properties
```

### Offline Compaction

```
TarMK Compaction:
├── Purpose: Reclaim disk space from old revisions
├── Online compaction: Runs while AEM is active (AEM 6.5+)
├── Offline compaction: oak-run tool (instance stopped)
│   └── java -jar oak-run.jar compact /path/to/segmentstore
├── Pre-migration requirement for AEMaaCS
│   └── Run before Content Transfer Tool extraction
└── Regular maintenance: scheduled via Maintenance Window
```

---

### Quick Reference: Architecture Decision Table

| Decision | Choose A | Choose B |
|---|---|---|
| **Component logic** | Sling Models (preferred) | WCMUse (legacy) |
| **Templating** | HTL (preferred) | JSP (deprecated) |
| **Template type** | Editable Templates (preferred) | Static Templates (legacy) |
| **Search path** | /apps overlays /libs | Never modify /libs directly |
| **Resource resolution** | sling:resourceType (preferred) | Absolute paths (avoid) |
| **Persistence** | TarMK (default) | MongoMK (100+ authors) |
| **Binary storage** | S3 DataStore (recommended) | File DataStore (simple setups) |

---

*[Back to AD0-E117 Index](../README.md)*
