# Content Architecture & Information Models

[Back to AD0-E117 Index](../README.md)

---

## Content Hierarchy Design

### Best Practices (from Adobe Guidelines)

1. **Drive the content hierarchy intentionally** — Don't let it happen organically
2. **Content is king** — Structure should reflect content, not technology
3. **Store everything in the repository** — Content, binaries, code, configurations
4. **Design JCR structure first** — Then implement servlets/scripts
5. **Follow David's Model** — No same-name siblings, references are harmful

### Standard AEM Content Structure

```
/
├── content/
│   ├── mysite/                    # Site root
│   │   ├── en/                    # Language root
│   │   │   ├── jcr:content        # Page properties
│   │   │   ├── home/              # Homepage
│   │   │   ├── products/          # Products section
│   │   │   ├── about/             # About section
│   │   │   └── contact/           # Contact page
│   │   ├── fr/                    # French language
│   │   │   └── (mirror structure)
│   │   └── de/                    # German language
│   ├── dam/                       # Digital Asset Management
│   │   ├── mysite/
│   │   │   ├── images/
│   │   │   ├── documents/
│   │   │   └── videos/
│   │   └── shared/                # Cross-site shared assets
│   ├── experience-fragments/      # Experience Fragments
│   │   └── mysite/
│   └── cq:tags/                   # Taxonomy tags
│       └── mysite/
├── conf/                          # Configurations
│   └── mysite/
│       ├── settings/
│       │   ├── wcm/templates/     # Editable templates
│       │   ├── cloudconfigs/      # Cloud service configs
│       │   └── dam/               # Asset processing profiles
│       └── sling:configs/         # Context-aware configs
├── apps/                          # Application code
│   └── mysite/
│       ├── components/            # Component definitions
│       ├── templates/             # Template definitions
│       └── clientlibs/            # Client libraries
├── libs/                          # Product code (DO NOT MODIFY)
├── etc/                           # Misc configuration (legacy)
│   └── designs/ (deprecated)
├── var/                           # Runtime data
├── tmp/                           # Temporary data
└── oak:index/                     # Oak indexes
```

## Content Fragments vs Experience Fragments

### Comparison Table

| Feature | Content Fragments | Experience Fragments |
|---|---|---|
| **Purpose** | Structured content for headless/omnichannel | Layout + content for reuse across pages |
| **Layout** | No layout (content only) | Full layout with components |
| **Authoring** | CF Editor (structured fields) | Page editor (visual) |
| **Delivery** | API (JSON via Assets API/GraphQL) | HTML (rendered fragment) |
| **Variations** | Content variations (e.g., mobile, summary) | Visual variations (social, email, web) |
| **Translation** | Translatable as structured content | Translatable with layout |
| **Use Case** | Product descriptions, articles, API content | Headers, footers, promotional banners |
| **Components** | Used within CF components on pages | Drag into any page like a component |
| **Target/Personalization** | Via API | Direct use with Adobe Target |

### When to Use What

```
Decision Tree:
Need to reuse content across channels (web, mobile app, kiosk)?
├── YES → Need layout/presentation?
│   ├── YES → Experience Fragment
│   └── NO → Content Fragment (headless)
└── NO → Regular page components

Additional considerations:
├── API delivery needed? → Content Fragment
├── A/B testing with Target? → Experience Fragment
├── Same content, different layouts? → Content Fragment + rendering
└── Same layout + content across pages? → Experience Fragment
```

### Content Fragment Models

```
Content Fragment Model: "Product"
├── Fields:
│   ├── productName (Text, single-line, required)
│   ├── description (Text, multi-line, rich text)
│   ├── price (Number, decimal)
│   ├── sku (Text, unique identifier)
│   ├── category (Enumeration: Electronics, Clothing, Home)
│   ├── images (Content Reference, multi-value)
│   ├── specifications (JSON Object)
│   └── relatedProducts (Fragment Reference, multi-value)
├── Variations:
│   ├── master (default)
│   ├── summary (short version for cards)
│   └── mobile (optimized for mobile display)
└── Delivery:
    ├── GraphQL: /graphql/execute.json/mysite/products
    └── Assets API: /api/assets/content/dam/mysite/products
```

## Content Taxonomy

```
Taxonomy Architecture:
/content/cq:tags/
└── mysite/
    ├── product-type/
    │   ├── electronics/
    │   │   ├── phones
    │   │   ├── laptops
    │   │   └── tablets
    │   ├── clothing/
    │   └── home/
    ├── audience/
    │   ├── consumer
    │   ├── business
    │   └── enterprise
    ├── region/
    │   ├── north-america
    │   ├── europe
    │   └── asia-pacific
    └── campaign/
        ├── spring-2026
        └── summer-2026

Best Practices:
├── Flat + shallow hierarchy (max 3-4 levels)
├── Consistent naming convention (lowercase, hyphenated)
├── Namespace tags by site (avoid global tag pollution)
├── Use for: search, filtering, personalization, reporting
└── Apply to: pages, assets, content fragments
```

## i18n (Internationalization)

### Multi-Language Architecture

```
Approach 1: Language Copies (Recommended)
/content/mysite/
├── en/ (master language)
├── fr/ (French translation)
├── de/ (German translation)
└── ja/ (Japanese translation)

Translation Workflow:
1. Create Language Copy from master
2. Launch Translation Project
3. Send to translation provider (connector)
4. Review translated content
5. Approve and publish

Approach 2: MSM + Language Copies
/content/mysite/
├── en/ (blueprint)
│   └── Live copies auto-created for each region
├── en-us/ (US English - live copy + local changes)
├── en-gb/ (UK English - live copy + local changes)
└── fr-fr/ (French - language copy)
```

### i18n Dictionary

```
/apps/mysite/i18n/
├── en.json    # English strings
├── fr.json    # French strings
└── de.json    # German strings

Usage in HTL:
${'Log In' @ i18n}
${properties.title @ i18n, locale='fr'}
```

## DAM Content Architecture

### Folder Structure Best Practices

```
/content/dam/
├── mysite/
│   ├── brand-assets/        # Logos, brand guidelines
│   ├── marketing/           # Campaign assets
│   │   ├── 2026/
│   │   │   ├── spring-campaign/
│   │   │   └── summer-campaign/
│   ├── product-images/      # Product photography
│   │   ├── electronics/
│   │   └── clothing/
│   ├── documents/           # PDFs, whitepapers
│   └── videos/              # Video assets
└── shared/                  # Cross-site shared assets
    ├── icons/
    ├── stock-photos/
    └── templates/

Best Practices:
├── Mirror content site structure where logical
├── Use metadata schemas for consistent tagging
├── Apply processing profiles per folder
├── Set up linked folders for Workfront integration
├── Implement folder-level permissions
└── Use Smart Tags for AI-powered categorization
```

---

### Practice Scenarios

**Scenario 1:** A client with an existing AEM site using WCM Core Components (proxy pattern) wants to reuse existing page content on a new marketing channel with minimal refactoring. What should the Architect recommend?

<details>
<summary>Answer</summary>

**Content Fragments** — They provide structured, headless content delivery that can be consumed via API (GraphQL or Assets HTTP API) by any channel.

- Content Fragments separate content from presentation
- The existing page content would need to be restructured into Content Fragment Models
- New channels consume CFs via API without needing AEM rendering
- Minimal refactoring: create CF models matching existing content structure, migrate content, expose via GraphQL

Alternative considered but less optimal:
- Experience Fragments: These include layout, which ties them to web rendering
- Sling Model Exporter: Exposes component data as JSON, but still coupled to page structure
</details>

**Scenario 2:** A university produces multiple courses each semester. Each course consists of multiple pages, some common among courses. Each version must be archived. What primary AEM feature should the Architect recommend?

<details>
<summary>Answer</summary>

**Multi-Site Manager (MSM)** — MSM handles content reuse through Live Copies:

1. Create a **Blueprint** for the master course template
2. Create **Live Copies** for each course instance
3. Common pages (shared content) inherit from blueprint
4. Course-specific pages can be customized independently (inheritance broken)
5. Archiving: Use versioning and launches to capture point-in-time snapshots
6. Each semester: create new live copies from updated blueprint

MSM is specifically designed for this "shared base + customization" pattern at scale.
</details>

---

*[Back to AD0-E117 Index](../README.md)*
