# Performance Optimization & Maintenance

[Back to AD0-E117 Index](../README.md)

---

## Performance Architecture

### Performance Layers

```
Performance Optimization Stack:
┌──────────────────────────────────────┐
│ Layer 1: Browser/Client              │
│ ├── Browser cache (Cache-Control)    │
│ ├── Image optimization (WebP, lazy)  │
│ ├── JS/CSS minification             │
│ ├── Code splitting                   │
│ └── Critical CSS                     │
├──────────────────────────────────────┤
│ Layer 2: CDN                         │
│ ├── Edge caching (global PoPs)       │
│ ├── Image transformation            │
│ ├── Compression (gzip/brotli)       │
│ └── HTTP/2 push                     │
├──────────────────────────────────────┤
│ Layer 3: Dispatcher                  │
│ ├── HTML page caching               │
│ ├── Static resource caching         │
│ ├── /statfileslevel optimization    │
│ └── gracePeriod configuration        │
├──────────────────────────────────────┤
│ Layer 4: AEM Application             │
│ ├── Query optimization (indexes)     │
│ ├── Component caching (SDI/ESI)     │
│ ├── Sling Model efficiency          │
│ └── Client library optimization     │
├──────────────────────────────────────┤
│ Layer 5: JVM / Infrastructure        │
│ ├── JVM heap sizing                  │
│ ├── GC tuning (G1GC recommended)    │
│ ├── Thread pool configuration       │
│ └── I/O optimization                │
└──────────────────────────────────────┘
```

### Caching Strategy Architecture

```
Multi-Layer Cache Decision:
┌──────────────────────────────────────────┐
│ Is content static and cacheable?         │
│ ├── YES → Cache at CDN (longest TTL)     │
│ │   └── Versioned assets: 1 year         │
│ │   └── HTML pages: 5-15 minutes         │
│ │                                        │
│ ├── PARTIALLY → Cache at Dispatcher      │
│ │   └── Personalized? → SSI/ESI/AJAX     │
│ │   └── Authenticated? → No cache or     │
│ │       token-based invalidation         │
│ │                                        │
│ └── NO → No cache (dynamic/real-time)    │
│     └── API responses: max-age=0         │
│     └── Form submissions: no-store       │
└──────────────────────────────────────────┘
```

### Query Performance

```
Query Optimization Rules:
├── ALWAYS create Oak indexes for custom queries
├── NEVER allow traversal queries
│   └── Monitor: "Traversal query (query without index)"
│   └── Log: org.apache.jackrabbit.oak.plugins.index
├── Use EXPLAIN to verify index usage
│   └── Query Builder: &p.explain=true
├── Limit result sets
│   └── p.limit=100 (never unlimited)
├── Avoid orderby on non-indexed properties
├── Prefer path restrictions
│   └── path=/content/mysite (not path=/)
└── Use node type restrictions
    └── type=cq:Page (not type=nt:base)

Index Design:
├── Property Index: equality queries
│   └── WHERE property = value
├── Lucene Index: full-text + complex queries
│   └── WHERE CONTAINS(text, 'keyword')
│   └── ORDER BY property
│   └── Multiple property combinations
└── Custom Index Example:
    /oak:index/myCustomIndex
    ├── type = "lucene"
    ├── compatVersion = 2
    ├── async = "async"
    ├── includedPaths = ["/content/mysite"]
    └── indexRules/cq:Page/properties/
        ├── title: name=jcr:content/jcr:title, propertyIndex=true
        └── category: name=jcr:content/category, propertyIndex=true
```

### Adaptive Image Serving

```
Problem: DAM stores high-resolution images
         Serving originals wastes bandwidth

Solution: Adaptive Image Servlet
├── Serves appropriately sized renditions
├── Based on client viewport/device
├── Cached at Dispatcher level
├── Configuration:
│   └── com.adobe.cq.wcm.core.components.internal.servlets.AdaptiveImageServlet
│       ├── Supported widths: [320, 480, 640, 960, 1280, 1920]
│       ├── Quality: 82 (JPEG quality)
│       └── Format: JPEG, WebP (if supported)

Alternative: Dynamic Media
├── On-the-fly image transformation
├── Smart Crop (AI-powered)
├── CDN-integrated delivery
└── Best for large-scale asset delivery
```

## Maintenance Operations

### Service Pack Update Procedure

```
Service Pack Update Process:
1. ✅ Review release notes for breaking changes
2. ✅ Create full backup (repository + code)
3. ✅ Install in STAGING environment first
4. ✅ Verify all bundles are active
5. ✅ Run full REGRESSION TEST suite
6. ✅ Test critical business workflows
7. ✅ Verify custom code compatibility
8. ✅ If all pass → Deploy to PRODUCTION
9. ✅ Monitor post-deployment
10. ✅ Keep rollback plan ready

❌ NEVER install directly in production
❌ NEVER skip regression testing
❌ NEVER skip staging verification
```

> **Exam Tip:** The correct answer for service pack procedure is ALWAYS: "Full staging including regression test; if passed, deploy to production."

### Operations Dashboard

```
Health Checks:
├── Sling Jobs → Queue depth, failed jobs
├── Maintenance Tasks → Last run, next run
├── Replication Agents → Queue status, errors
├── Bundle Status → Active, resolved, fragment
├── Memory → Heap usage, GC frequency
├── CPU → System and process CPU
├── Thread Count → Active threads
├── Disk Space → Repository and filesystem
├── Query Performance → Slow queries, traversals
└── Security → Security Health Check
```

### Backup & Disaster Recovery

```
Backup Strategy:
├── Repository Backup
│   ├── Online backup (while instance running)
│   │   └── curl http://localhost:4502/system/console/backup
│   ├── TarMK: filesystem-level backup of segmentstore
│   └── MongoMK: MongoDB replica set backup
├── Code Backup
│   ├── Git repository (primary)
│   ├── Package manager exports
│   └── CI/CD pipeline artifacts
├── Configuration Backup
│   ├── OSGi configurations (in code)
│   ├── Dispatcher configurations
│   └── Infrastructure configs
└── Disaster Recovery
    ├── TarMK Cold Standby → automatic failover
    ├── AEMaaCS → Adobe-managed DR
    ├── RPO target: varies by business
    └── RTO target: varies by business
```

### JVM Tuning

```
Recommended JVM Settings (AEM 6.5):
├── Heap Size
│   ├── Author: -Xms4g -Xmx8g (typical)
│   ├── Publish: -Xms2g -Xmx4g (typical)
│   └── Scale based on content volume
├── Garbage Collector
│   ├── G1GC recommended: -XX:+UseG1GC
│   ├── -XX:MaxGCPauseMillis=200
│   └── -XX:+ParallelRefProcEnabled
├── Metaspace
│   └── -XX:MaxMetaspaceSize=512m
└── Monitoring
    ├── Enable GC logging
    ├── Monitor via Operations Dashboard
    └── JMX monitoring for heap analysis
```

## Testing Strategy

### Pre-Launch Testing Matrix

| Test Type | Purpose | When |
|---|---|---|
| **Unit Testing** | Component-level correctness | During development |
| **Integration Testing** | Cross-component interaction | Sprint completion |
| **Functional Testing** | Business requirement validation | UAT phase |
| **Regression Testing** | No existing features broken | Every SP/release |
| **Load Testing** | Performance under expected traffic | Pre-launch |
| **Stress Testing** | System limits and breaking point | Pre-launch |
| **Penetration Testing** | Security vulnerability discovery | Pre-launch |
| **Accessibility Testing** | WCAG compliance | Pre-launch |

> **Exam Tip:** For pre-launch testing, the two types an Architect should specifically recommend to identify system weaknesses are: **Load Testing** and **Penetration Testing**.

---

### Practice Scenarios

**Scenario 1:** A client using AEM 6.5 on-premise experiences 100GB daily repository growth caused by DAM (100 renditions per image). Also seeing stale content on article pages. How should the Architect address these issues?

<details>
<summary>Answer</summary>

**Leverage AEM Adaptive Image Servlet, cache image variations in Dispatcher, and reimplement article components to use AJAX.**

1. **DAM Growth:**
   - Reduce number of renditions by customizing DAM processing profiles
   - Use Adaptive Image Servlet (serves correct size on-demand, fewer stored renditions)
   - Cache image renditions at Dispatcher layer
   - Consider external binary storage (S3 DataStore) to offload repository

2. **Stale Content:**
   - Implement AJAX-based loading for dynamic article components
   - This allows the page HTML to be cached at Dispatcher while dynamic content loads client-side
   - Set appropriate TTLs and invalidation strategies

3. **Long-term:**
   - Evaluate migration to AEMaaCS (asset microservices handle renditions)
   - Implement regular repository maintenance (online compaction)
</details>

**Scenario 2:** Publisher server crashes randomly after deploying a new static FAQ component. Heap dump shows large number of FAQ component instances. What is likely causing this?

<details>
<summary>Answer</summary>

**The component has a cyclic dependency to itself.**

This is a classic AEM issue where:
1. The FAQ component's HTL/script includes itself (via `data-sly-resource`)
2. This creates infinite recursion
3. Each recursion creates a new component instance in memory
4. Eventually the JVM runs out of heap memory → crash

Fix:
- Check the component's HTL for self-referencing `data-sly-resource` calls
- Add a recursion guard (e.g., Sling include depth limit)
- Ensure the component doesn't reference its own resource type without a termination condition
</details>

---

*[Back to AD0-E117 Index](../README.md)*
