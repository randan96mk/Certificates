# AD0-E117 — Practice Q&A Bank

[Back to AD0-E117 Index](../README.md)

---

## Section 1: Discovery (18%)

### Q1 [Architect]
A company has a global business distributed around the globe using Adobe Managed Services with on-premise setup. For legal reasons, no content can be shared between regions. Editors must work globally without being affected by maintenance or deployments. What setup should the Architect use?

- a) An author active-active cluster spread across multiple availability zones
- b) An author MongoDB cluster using horizontal scaling and regional distribution
- c) An author cold standby cluster using sling distribution
- d) An author content sharding setup regionally distributed

<details><summary>Answer</summary>

**d) An author content sharding setup regionally distributed**

Content sharding provides:
- Regional isolation (no content sharing — meets legal requirement)
- Independent maintenance windows per region
- Editors unaffected by other regions' deployments
- Each region operates as an independent AEM author + publish stack
</details>

### Q2 [Advanced]
When translating business goals to functional requirements for an AEM architecture, which factors must the Architect prioritize for defining non-functional requirements?

- a) Number of content authors, page templates, and component types
- b) Scalability, security, performance, and availability targets
- c) Design mockups and brand guidelines
- d) CMS feature comparison with competitors

<details><summary>Answer</summary>

**b) Scalability, security, performance, and availability targets**

Non-functional requirements (NFRs) define HOW the system should perform, not WHAT it should do. Key NFRs: expected concurrent users, page load times, uptime SLA, data sovereignty, and security compliance requirements.
</details>

---

## Section 2: Solution Design (44%)

### Q3 [Architect]
Performance testing shows slow response times for HTML requests to static content pages. What should the Architect confirm first?

- a) JVM memory of the AEM instance is sized sufficiently
- b) Dispatcher caching is enabled for the request URLs
- c) Image requests are excluded from performance test
- d) All OSGi bundles are in active state

<details><summary>Answer</summary>

**b) Dispatcher caching is enabled for the request URLs**

Dispatcher caching is the most impactful and most common performance issue. If static pages aren't being served from cache, every request hits the publish instance, causing unnecessary load. Check: cache rules, statfileslevel, query string handling, and authentication cookies.
</details>

### Q4 [Architect]
A site hosted on AEM needs to integrate a new third-party REST service with 99.5% uptime SLA. The AEM site currently has a 99.5% uptime SLA. What integration approach should the Architect take?

- a) Implement a fallback in case the service is unavailable
- b) Add retries with exponential backoff, fail after 1 minute
- c) Retrieve and store recommendations in JCR nightly
- d) Improve the AEM production setup to comply with 99.9% uptime

<details><summary>Answer</summary>

**a) Implement a fallback in case the service is unavailable**

Two 99.5% services combined = ~99.0% compound availability. A fallback mechanism (circuit breaker, cached last-known-good data, default content) ensures the site remains functional even when the third-party service is down.
</details>

### Q5 [Advanced]
Which two factors would lead the Architect to pick a multi-author instance architecture setup? (Select two)

- a) Number of parallel authors
- b) Website traffic handled by servers
- c) Fail-safeness of the servers
- d) Types of actions performed by authors
- e) Cache efficiency on the dispatcher

<details><summary>Answer</summary>

**a) Number of parallel authors** — More concurrent authors require more author capacity (horizontal scaling via MongoMK)

**d) Types of actions performed by authors** — Heavy operations like DAM uploads, bulk activations, and complex workflows require more resources per author, potentially necessitating multiple instances
</details>

### Q6 [Advanced]
For a client with existing AEM site using WCM Core Components proxy pattern who wants to reuse page content on another marketing channel with minimal refactoring, what should the Architect recommend?

- a) Experience Fragments
- b) Content Fragments
- c) Assets API
- d) Sling Model Exporter

<details><summary>Answer</summary>

**b) Content Fragments**

Content Fragments provide structured, headless content delivery via GraphQL/Assets API. They separate content from presentation, allowing the same content to be consumed by any channel without AEM rendering. This requires the least refactoring — create CF models matching existing content structure and expose via API.
</details>

### Q7 [Advanced]
A customer wants to simplify and automate publishing press release pages on specific dates. What should the Architect recommend?

- a) Create a custom workflow
- b) Use Launches
- c) Implement a Sling event listener on page creation
- d) Configure MSM with 'Activate on Blueprint activation' option

<details><summary>Answer</summary>

**b) Use Launches**

Launches allow content authors to prepare future content changes and schedule them for automatic publication on a specific date. This is the AEM-native solution for scheduled publishing without custom code.
</details>

### Q8 [Advanced]
Users report data inaccuracies in an AEM component that relies on search functionality. AEM uses Lucene as its search engine. How should the Architect resolve this?

- a) Scale up server resources
- b) Change the search engine to Property Search
- c) Migrate search engine to external Solr instance
- d) Add search indexes to Lucene search engine

<details><summary>Answer</summary>

**b) Change the search engine to Property Search**

Lucene indexes are asynchronous — there can be a delay between content changes and index updates, causing data inaccuracies. Property indexes are synchronous and return accurate results immediately. For components that need real-time accuracy over full-text capabilities, Property Search is more appropriate.
</details>

### Q9 [Architect]
What determines the choice between TarMK and MongoMK persistence?

- a) Number of publish instances
- b) CDN configuration requirements
- c) Number of concurrent authors and authoring load
- d) Dispatcher cache size

<details><summary>Answer</summary>

**c) Number of concurrent authors and authoring load**

TarMK: Default, recommended for < 100 concurrent authors (single author + cold standby)
MongoMK: Required for 100+ concurrent authors (horizontal scaling via MongoDB replica set)

Publish instances always use TarMK regardless of author topology.
</details>

---

## Section 3: Implementation (22%)

### Q10 [Advanced]
An existing AEM customer wants to reduce dependency on the development team for website overhaul. Which approach should the Architect choose?

- a) Use Editable Templates and leverage Core Components
- b) Develop custom components and Style Systems on static templates
- c) Create programmatic templates for multiple pages
- d) Implement static templates with configurable styles

<details><summary>Answer</summary>

**a) Use Editable Templates and leverage Core Components**

Editable Templates empower content authors to create/modify page structures. Core Components are production-ready, reducing custom development. Style System allows visual customization via CSS without code. This combination minimizes developer dependency.
</details>

### Q11 [Advanced]
An existing AEM sites platform receives the latest service pack. Which action should the Architect take?

- a) Identify affected packages after installing in production and monitor
- b) Install in staging, ensure all bundles are running, then deploy to production
- c) Do a full staging including regression test; if passed, deploy to production
- d) Advise customer to request Adobe to install directly on production

<details><summary>Answer</summary>

**c) Do a full staging including regression test; if passed, deploy to production**

Service pack procedure: Install in staging → Full regression testing → Verify all functionality → Only then deploy to production. Never skip regression testing, never install directly in production.
</details>

### Q12 [Advanced]
A client is preparing to launch a new website on AEM. Which two types of testing should the Architect recommend to identify system weaknesses? (Select two)

- a) Load Testing
- b) Penetration Testing
- c) Regression Testing
- d) Unit Testing
- e) Functional Testing

<details><summary>Answer</summary>

**a) Load Testing** — Identifies performance bottlenecks under expected and peak traffic conditions

**b) Penetration Testing** — Identifies security vulnerabilities that could be exploited

These two specifically target "system weaknesses" — performance limits and security holes — which are the architect's primary concerns pre-launch.
</details>

---

## Section 4: Maintenance (16%)

### Q13 [Advanced]
What is the recommended approach for handling publishing automation for scheduled content in AEM?

- a) Custom workflow with cron trigger
- b) Launches with scheduled promotion
- c) Sling scheduled jobs
- d) External CI/CD pipeline trigger

<details><summary>Answer</summary>

**b) Launches with scheduled promotion**

Launches is AEM's native feature for creating future versions of content and scheduling their promotion (activation) at a specific date/time. It's author-friendly and doesn't require custom code.
</details>

### Q14 [Advanced]
Publisher server crashes randomly after deploying a new static FAQ component. Heap dump shows large numbers of FAQ component instances. What is the likely cause?

- a) Page not cached in Dispatcher
- b) Component triggers a traversal query
- c) JVM garbage collection is not running
- d) Component has a cyclic dependency to itself

<details><summary>Answer</summary>

**d) Component has a cyclic dependency to itself**

Large numbers of the same component instance in a heap dump indicates infinite recursion — the component includes itself. Each recursion creates a new instance until OutOfMemoryError crashes the JVM.
</details>

### Q15 [Architect]
Content authors need to manage configuration per brand across a multi-brand AEM platform. The configuration includes header logos, colors, analytics IDs, and social links. What AEM feature should the Architect recommend?

- a) Custom OSGi configurations per brand
- b) Sling Context-Aware Configurations
- c) Separate /etc/designs per brand
- d) Custom JCR properties on site root pages

<details><summary>Answer</summary>

**b) Sling Context-Aware Configurations**

Context-Aware Configurations (CA Configs) are specifically designed for multi-brand/multi-site configuration management. They:
- Store configs under /conf/{brand}
- Resolve automatically based on content hierarchy
- Can be authored by content authors (no developer needed)
- Support structured configuration models
- Follow the standard /conf pattern
</details>

---

*Total: 15 questions covering all 4 domains. [Back to AD0-E117 Index](../README.md)*
