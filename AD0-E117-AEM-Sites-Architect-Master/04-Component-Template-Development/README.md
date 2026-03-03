# Component & Template Development

[Back to AD0-E117 Index](../README.md)

---

## Editable Templates vs Static Templates

| Feature | Editable Templates | Static Templates |
|---|---|---|
| **Created by** | Template authors (no code) | Developers (code) |
| **Location** | /conf/{site}/settings/wcm/templates | /apps/{project}/templates |
| **Policies** | Supports content policies | No policies |
| **Initial Content** | Pre-configured content | Fixed structure only |
| **Structure Lock** | Lock/unlock components | Always locked |
| **Author Flexibility** | High (authors customize) | Low (developer-defined) |
| **Recommendation** | **Preferred** | Legacy (avoid new) |

### Editable Template Architecture

```
Editable Template:
├── Template Type (developer-created)
│   ├── Structure page (defines allowed areas)
│   ├── Initial content
│   └── Policies
├── Template (author-created from type)
│   ├── Structure (locked components)
│   │   ├── Header (locked)
│   │   ├── Navigation (locked)
│   │   └── Footer (locked)
│   ├── Initial Content (default content)
│   │   └── Pre-placed components
│   ├── Layout (responsive grid)
│   │   └── Column configurations
│   └── Policies (component restrictions)
│       ├── Allowed components per container
│       ├── Style System options
│       └── Default component configurations
└── Page (created from template)
    ├── Inherits structure
    ├── Receives initial content (editable)
    └── Author adds/modifies content
```

### Template Best Practices

```
Template Design Guidelines:
├── Keep template count LOW
│   └── One per fundamentally different page structure
│   └── NOT one per page type (use policies instead)
├── Lock structural elements
│   └── Header, footer, navigation = locked
│   └── Content areas = unlocked
├── Use Initial Content wisely
│   └── Pre-place commonly needed components
│   └── Authors can remove if not needed
├── Define policies per template
│   └── Restrict allowed components per container
│   └── Prevents authors from misusing components
└── Naming: descriptive, site-prefixed
    └── "MySite - Article Page", "MySite - Landing Page"
```

## Core Components (WCM Core Components)

### Proxy Component Pattern

```
Product Component (in /libs):
/libs/core/wcm/components/text/v2/text
  └── Full implementation

Project Proxy (in /apps):
/apps/myproject/components/text
  ├── .content.xml
  │   └── sling:resourceSuperType = "core/wcm/components/text/v2/text"
  └── (optional) _cq_dialog/ or _cq_design_dialog/
      └── Customizations only (don't duplicate base dialog)

Why Proxy?
├── Insulates project from Core Component version changes
├── Allows project-specific customization
├── Enables Style System overrides
├── Follows /apps > /libs overlay principle
└── Easy to upgrade: change sling:resourceSuperType version
```

### Key Core Components to Know

| Component | Purpose | Common Customization |
|---|---|---|
| **Page** | Base page component | Add custom metadata fields |
| **Text** | Rich text content | Style System variants |
| **Image** | Responsive images | Lazy loading, custom renditions |
| **Title** | Heading component | Style System (H1-H6 mapping) |
| **List** | Content listing | Custom renderings |
| **Navigation** | Site navigation | Multi-level, mega-menu |
| **Breadcrumb** | Page hierarchy trail | Custom separator, max depth |
| **Experience Fragment** | Fragment reference | Layout variations |
| **Content Fragment** | CF display | Custom renderings |
| **Form Container** | Form handling | Submission actions |
| **Carousel** | Content carousel | Animation, timing |
| **Tabs / Accordion** | Tabbed content | Style variations |
| **Teaser** | Promotional content | CTA variations |
| **Container** | Layout container | Grid configurations |

### Component Versioning

```
Version Strategy:
├── Core Components use semantic versioning
│   └── v1 → v2 → v3 (non-breaking path changes)
├── Proxy always points to specific major version
│   └── sling:resourceSuperType = "core/wcm/components/text/v2/text"
├── Upgrading: Update sling:resourceSuperType
│   └── Test thoroughly — dialog may change
└── NEVER make breaking changes to existing components
    └── Create new version instead
```

## Style System

```
Style System Architecture:
├── Purpose: Visual variations without new components
├── Configured in: Template policies (not code)
├── Applied by: Content authors
├── Implementation: CSS classes added to component wrapper
│
├── Example: Button Component
│   ├── Style Group: "Size"
│   │   ├── Small (class: cmp-button--small)
│   │   ├── Medium (class: cmp-button--medium)
│   │   └── Large (class: cmp-button--large)
│   └── Style Group: "Color"
│       ├── Primary (class: cmp-button--primary)
│       ├── Secondary (class: cmp-button--secondary)
│       └── Ghost (class: cmp-button--ghost)
│
└── CSS Implementation:
    .cmp-button--small { padding: 4px 8px; font-size: 12px; }
    .cmp-button--large { padding: 12px 24px; font-size: 18px; }
    .cmp-button--primary { background: var(--brand-primary); }
```

## HTL (HTML Template Language)

### HTL vs JSP

| Feature | HTL | JSP |
|---|---|---|
| **Security** | Auto XSS escaping | Manual escaping needed |
| **Separation** | Strict separation of logic | Logic mixed with markup |
| **Learning** | HTML-like syntax | Java embedded in HTML |
| **Performance** | Compiled to servlets | Compiled to servlets |
| **Recommendation** | **Preferred** | **Deprecated** |

### Common HTL Patterns

```html
<!-- Conditional rendering -->
<div data-sly-test="${component.title}">
    <h1>${component.title}</h1>
</div>

<!-- Iteration -->
<ul data-sly-list.item="${component.items}">
    <li>${item.title}</li>
</ul>

<!-- Include another component -->
<div data-sly-resource="${'header' @ resourceType='myproject/components/header'}"></div>

<!-- Sling Model binding -->
<sly data-sly-use.model="com.myproject.models.HeroModel">
    <h1>${model.title}</h1>
    <p>${model.description}</p>
</sly>

<!-- i18n -->
<button>${'Submit' @ i18n}</button>

<!-- Context-aware escaping -->
<a href="${properties.link @ context='uri'}">${properties.linkText}</a>
```

## SPA Editor Architecture

```
SPA Editor Pattern:
├── React/Angular SPA app
├── AEM provides:
│   ├── Content editing in AEM author
│   ├── SPA SDK for component mapping
│   ├── Model JSON exported by AEM
│   └── Editable regions defined by author
├── Architecture:
│   ├── Author: SPA + AEM editing overlay
│   └── Publish: SPA served by AEM or external host
├── Key Concepts:
│   ├── EditableComponent (AEM SDK)
│   ├── ModelManager (fetches page model JSON)
│   ├── ComponentMapping (SPA component ↔ AEM resource type)
│   └── RemoteSPAPage (external SPA with AEM-managed regions)
└── Considerations:
    ├── SSR for SEO (server-side rendering)
    ├── Author experience must be preserved
    └── Performance: model JSON size management
```

---

### Practice Scenarios

**Scenario 1:** An existing AEM customer wants a complete overhaul of their websites with a large data set, wanting to reduce dependency on the development team. Which approach should the Architect choose?

<details>
<summary>Answer</summary>

**Use Editable Templates and leverage Core Components.**

- Editable Templates empower content authors to create and modify page structures without developer involvement
- Core Components provide production-ready, accessible, well-tested components
- Style System allows visual customization via CSS without new code
- Template policies control which components are available per context

This approach minimizes developer involvement for day-to-day content operations while maintaining quality and consistency.

Alternatives rejected:
- Static templates: Increase developer dependency (opposite of goal)
- Custom components: More dev time needed
- Programmatic templates: Still require code changes
</details>

---

*[Back to AD0-E117 Index](../README.md)*
