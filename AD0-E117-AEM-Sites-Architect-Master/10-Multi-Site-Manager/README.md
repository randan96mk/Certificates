# Multi-Site Manager (MSM)

[Back to AD0-E117 Index](../README.md)

---

## MSM Architecture

```
Multi-Site Manager Hierarchy:
┌──────────────────────┐
│     Blueprint         │  (Source/Master)
│  /content/mysite/en  │
└──────────┬───────────┘
           │ Live Copy
    ┌──────┼──────────────────┐
    ▼      ▼                  ▼
┌────────┐ ┌────────┐ ┌────────┐
│ en-us  │ │ en-gb  │ │ en-au  │  (Live Copies)
│(US Eng)│ │(UK Eng)│ │(AU Eng)│
└────────┘ └────────┘ └────────┘
    │
    │ Language Copy
    ▼
┌────────┐
│ fr-us  │  (Language Copy + Live Copy)
│(US Fr) │
└────────┘
```

## Core Concepts

### Blueprint
- **Source** content that serves as the master
- Changes to blueprint can roll out to live copies
- Multiple live copies can share one blueprint
- Blueprint can also be a live copy of another blueprint (chained)

### Live Copy
- **Linked copy** of a blueprint that maintains a relationship
- Inherits content and structure from blueprint
- Can have **local customizations** (inheritance broken per component)
- Inheritance can be cancelled/reinstated per component or page

### Rollout Configurations

| Configuration | Trigger | Behavior |
|---|---|---|
| **Standard rollout config** | Manual rollout or on blueprint activation | Synchronizes content on rollout action |
| **Push on modify** | Automatic on blueprint change | Every blueprint change immediately rolls out |
| **Activate on rollout** | On rollout action | Activates (publishes) live copy pages |
| **Deactivate on rollout** | On rollout action | Deactivates live copy pages |

### Synchronization Actions

| Action | Description |
|---|---|
| **contentUpdate** | Updates page content from blueprint |
| **contentCopy** | Copies content (overwrites local changes) |
| **contentDelete** | Deletes content not in blueprint |
| **referencesUpdate** | Updates internal references to point to live copy |
| **orderChildren** | Synchronizes child page order |
| **markLiveRelationship** | Marks the live relationship on created items |

### Performance Warning: Push on Modify

```
⚠️ PERFORMANCE CRITICAL:

"Push on modify" rollout config triggers on EVERY blueprint change.
With large site structures (30+ languages, 150+ sites):
├── Each blueprint change → triggers rollout to ALL live copies
├── Multiplied by synchronization actions per page
├── Can cause: replication queue buildup, author performance degradation
└── Mitigation:
    ├── Remove 'referencesUpdate' action (most expensive)
    ├── Use scheduled rollout instead of push-on-modify
    ├── Limit push-on-modify to critical content only
    └── Consider manual rollout for large-scale changes
```

## MSM + Language Copies

### Combined Architecture

```
Multi-Region, Multi-Language:
/content/global-brand/
├── en/ (Blueprint - English Master)
│   ├── home
│   ├── products/
│   └── about/
├── en-us/ (Live Copy: US English)
│   ├── home (inherited + local US content)
│   ├── products/ (inherited)
│   └── about/ (local US page)
├── en-gb/ (Live Copy: UK English)
│   ├── home (inherited + UK localizations)
│   └── products/ (inherited)
├── fr/ (Language Copy: French Master)
│   ├── home (translated from en)
│   └── products/ (translated from en)
├── fr-ca/ (Live Copy of fr: Canadian French)
│   ├── home (inherited from fr + CA customizations)
│   └── products/ (inherited from fr)
└── de/ (Language Copy: German)
    └── (translated from en)
```

### MSM vs Language Copies

| Feature | MSM (Live Copy) | Language Copy |
|---|---|---|
| **Purpose** | Regional content variation | Language translation |
| **Relationship** | Live, ongoing synchronization | Point-in-time copy (translation project) |
| **Updates** | Rollout synchronizes changes | Re-launch translation project |
| **Inheritance** | Component-level inheritance control | No ongoing inheritance |
| **Use Case** | Same language, regional differences | Different language entirely |

## Practical Implementation Patterns

### Pattern 1: Global Brand, Regional Customization

```
Use MSM when:
├── Same language across regions
├── 80%+ content is shared
├── Regional variations are minor (pricing, contact info, legal)
├── Central team controls master content
└── Regional teams customize locally

Architecture:
├── Blueprint: /content/brand/master-en
├── Live Copy: /content/brand/en-us (US customizations)
├── Live Copy: /content/brand/en-gb (UK customizations)
└── Live Copy: /content/brand/en-au (AU customizations)
```

### Pattern 2: Multi-Brand, Shared Components

```
Use MSM when:
├── Multiple brands share structural elements
├── Core components are shared (header, footer, navigation)
├── Brand-specific styling (via Style System, not content)
└── Shared product catalog with brand-specific presentation

Architecture:
├── Blueprint: /content/shared/templates
├── Live Copy: /content/brand-a (Brand A site)
├── Live Copy: /content/brand-b (Brand B site)
└── Each brand: inheritance on structure, broken on branding
```

---

### Practice Scenarios

**Scenario 1:** A global company has 30+ language sites and 150+ regional variations. They're using "Push on modify" rollout configuration and experiencing severe author performance issues. What should the Architect recommend?

<details>
<summary>Answer</summary>

**Optimization steps:**

1. **Replace "Push on modify" with manual/scheduled rollout** for most content
   - Push on modify triggers on every blueprint change, causing cascading updates across 150+ sites

2. **Remove `referencesUpdate` action** from rollout configurations
   - This is the most expensive action (scans all references across the tree)
   - Only add it when cross-references genuinely need updating

3. **Implement tiered rollout strategy:**
   - Critical content (pricing, legal): Use push-on-modify with limited scope
   - Standard content: Scheduled rollout (daily/weekly)
   - Marketing content: Manual rollout by regional teams

4. **Optimize replication:**
   - Configure replication agents for batch activation
   - Use tree activation for bulk publishes
   - Stagger rollouts across regions to distribute load

5. **Consider content sharding** if regions are truly independent
</details>

---

*[Back to AD0-E117 Index](../README.md)*
