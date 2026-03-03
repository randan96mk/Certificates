# Dispatcher & CDN Architecture

[Back to AD0-E117 Index](../README.md)

---

## Dispatcher Overview

```
Client Request Flow:
┌────────┐     ┌────────┐     ┌────────────┐     ┌──────────┐
│ Browser │ ──▶ │  CDN   │ ──▶ │ Dispatcher │ ──▶ │ Publish  │
│         │     │(Fastly)│     │  (Apache)  │     │ Instance │
└────────┘     └────────┘     └────────────┘     └──────────┘
                                    │
                              ┌─────▼─────┐
                              │   Cache    │
                              │ (Filesystem)│
                              └───────────┘
```

## Dispatcher Functions

| Function | Description |
|---|---|
| **Caching** | Stores rendered HTML/assets on filesystem for fast delivery |
| **Load Balancing** | Distributes requests across multiple publish instances |
| **Security** | First line of defense; URL filtering, header management |
| **URL Rewriting** | Vanity URLs, redirects |

## Caching Architecture

### Cache Invalidation Mechanism

```
.stat File Invalidation:
├── When content is activated (published):
│   1. Author sends flush request to Dispatcher
│   2. Dispatcher creates/updates .stat file
│   3. .stat file timestamp updates at configured level
│   4. Next request: Dispatcher compares file timestamp vs .stat
│   │   ├── File newer than .stat → serve from cache
│   │   └── File older than .stat → re-fetch from publish
│   └── Result: Stale files are re-requested on next access
│
├── /statfileslevel setting:
│   ├── 0: Single .stat file at docroot (invalidates everything)
│   ├── 1: .stat files per first-level directory
│   ├── 2: .stat files per second-level directory
│   └── Higher = more granular invalidation = fewer cache misses
│
│   Example with level=2:
│   /content/mysite/en/.stat ← only en pages invalidated
│   /content/mysite/fr/.stat ← fr pages unaffected
│
└── /gracePeriod setting:
    ├── Allows serving stale content briefly during invalidation
    ├── Reduces thundering herd on publish
    └── Set in seconds (e.g., gracePeriod "2")
```

### Cacheable Content Rules

```
dispatcher.any Configuration:

/cache {
  /rules {
    # Cache HTML pages
    /0000 { /glob "*" /type "allow" }
    # Don't cache POST requests
    /0001 { /glob "* POST *" /type "deny" }
    # Don't cache URLs with query strings
    /0002 { /glob "* *?*" /type "deny" }
    # Don't cache authenticated content
    /0003 { /glob "*/login*" /type "deny" }
  }

  /invalidate {
    /0000 { /glob "*" /type "deny" }
    /0001 { /glob "*.html" /type "allow" }
    /0002 { /glob "*" /type "allow" }
  }

  /statfileslevel "2"
  /gracePeriod "2"
}
```

### What Gets Cached vs Not Cached

| Cached | Not Cached |
|---|---|
| Static HTML pages | POST requests |
| Images, CSS, JS | Authenticated/personalized content |
| PDF documents | URLs with query strings (by default) |
| Font files | Dynamic API responses |
| Client library files | Form submissions |
| Content Fragment JSON | Search results (unless configured) |

## Dispatcher Security

### Security Checklist (Exam Critical)

```
Security Hardening:
├── Restrict dangerous URL patterns
│   ├── Deny access to /crx/*
│   ├── Deny access to /system/*
│   ├── Deny access to /apps/* and /libs/*
│   ├── Deny access to /etc/* (selectively)
│   └── Deny access to /content/usergenerated/*
├── HTTP Headers
│   ├── X-Frame-Options: SAMEORIGIN (clickjacking prevention)
│   ├── X-Content-Type-Options: nosniff
│   ├── Content-Security-Policy (XSS prevention)
│   └── Strict-Transport-Security (HTTPS enforcement)
├── CSRF Protection
│   ├── Allow CSRF token endpoint
│   └── Validate tokens on form submissions
├── Referrer Filter
│   ├── Configure Apache Sling Referrer Filter
│   └── Whitelist allowed referrer domains
├── Deny Selectors/Extensions
│   ├── Block .json, .xml on sensitive paths
│   ├── Block .infinity.json (traversal)
│   └── Block .tidy.json, .sysview.xml
└── File Access
    ├── Deny .htaccess, .htpasswd
    ├── Deny .content.xml, .vlt files
    └── Deny OS-specific files (.DS_Store, etc.)
```

### Dispatcher Filter Rules (Priority)

```
/filter {
  # Deny everything by default
  /0001 { /type "deny" /url "*" }

  # Allow content paths
  /0010 { /type "allow" /url "/content/*" }

  # Allow client libraries
  /0020 { /type "allow" /url "/etc.clientlibs/*" }

  # Allow static resources
  /0030 { /type "allow" /url "/etc/designs/*" }

  # Deny CRX and system paths
  /0100 { /type "deny" /url "/crx/*" }
  /0101 { /type "deny" /url "/system/*" }
  /0102 { /type "deny" /url "/apps/*" }
  /0103 { /type "deny" /url "/libs/*" }

  # Deny dangerous extensions
  /0200 { /type "deny" /extension "json" /path "/content/*" }
  /0201 { /type "deny" /extension "xml"  /path "/content/*" }

  # Allow specific JSON endpoints
  /0210 { /type "allow" /url "/content/mysite/*/jcr:content.model.json" }
}
```

## CDN Integration

### AEM as a Cloud Service CDN (Fastly)

```
AEMaaCS CDN Architecture:
┌────────┐     ┌─────────────┐     ┌────────────┐     ┌──────────┐
│ Client  │ ──▶ │   Fastly    │ ──▶ │ Dispatcher │ ──▶ │ Publish  │
│         │     │   (CDN)     │     │  (Apache)  │     │ (AEM)    │
└────────┘     └─────────────┘     └────────────┘     └──────────┘

CDN Features:
├── Edge caching (global PoPs)
├── SSL/TLS termination
├── DDoS protection
├── HTTP/2 support
├── Image optimization (optional)
├── Custom domain support
└── CDN-level redirects

Cache Control:
├── CDN respects Cache-Control headers from Dispatcher
├── Default TTL configurable via CDN rules
├── Surrogate-Key based invalidation
└── Soft purge (serve stale while revalidating)
```

### Cache-Control Header Strategy

| Content Type | Cache-Control | TTL | Reasoning |
|---|---|---|---|
| HTML pages | `max-age=300, stale-while-revalidate=60` | 5 min | Balance freshness vs performance |
| CSS/JS (versioned) | `max-age=31536000, immutable` | 1 year | Hash in filename ensures uniqueness |
| Images (DAM) | `max-age=86400` | 24 hours | Moderate freshness needed |
| Fonts | `max-age=31536000` | 1 year | Rarely change |
| API responses | `no-cache` or `max-age=60` | 0-1 min | Data freshness critical |

## Dispatcher in AEMaaCS

```
Key Differences:
├── Configuration in codebase (not server filesystem)
├── Validated by Dispatcher Validator SDK before deployment
├── Simplified directory structure:
│   ├── conf.d/
│   │   ├── available_vhosts/
│   │   └── dispatcher_vhost.conf
│   └── conf.dispatcher.d/
│       ├── available_farms/
│       ├── cache/
│       ├── clientheaders/
│       ├── filters/
│       └── renders/
├── No direct server access for troubleshooting
├── Logs available via Cloud Manager
└── Cannot modify Apache modules
```

---

### Practice Scenarios

**Scenario 1:** Performance testing shows slow response times for static content pages. What should an Architect confirm first?

<details>
<summary>Answer</summary>

**Confirm Dispatcher caching is enabled for the request URLs.**

Check:
1. `/cache/rules` allow the content type
2. The URL doesn't contain query strings (cached only if configured)
3. No `Cache-Control: no-cache` header from AEM
4. `/statfileslevel` is appropriately set (not too low)
5. Cache files exist on filesystem (check Dispatcher cache directory)
6. No authentication cookies preventing caching

This is the most common and impactful performance fix — a misconfigured Dispatcher can cause every request to hit the publish instance.
</details>

**Scenario 2:** A site has 99.5% uptime SLA and needs to integrate a third-party REST service (also 99.5% SLA). What integration approach should the Architect take?

<details>
<summary>Answer</summary>

**Implement a fallback mechanism in case the recommendation service is unavailable.**

Reasoning:
- Two 99.5% SLA services combined = 99.0% compound availability
- This exceeds the acceptable downtime threshold
- Fallback ensures the site remains functional even if the third-party service is down

Implementation:
1. Implement circuit breaker pattern
2. Define fallback content (static recommendations, cached last-known-good data)
3. Set reasonable timeouts (don't let slow responses drag down page load)
4. Cache API responses at Dispatcher level where possible
5. Use asynchronous loading (AJAX) so page renders without blocking on the service
</details>

---

*[Back to AD0-E117 Index](../README.md)*
