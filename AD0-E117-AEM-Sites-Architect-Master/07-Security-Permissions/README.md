# Security & Permissions

[Back to AD0-E117 Index](../README.md)

---

## Authentication Architecture

### Authentication Methods Comparison

| Method | Use Case | Tier | Protocol |
|---|---|---|---|
| **SAML 2.0** | Enterprise SSO for publish | Publish | SAML assertions |
| **LDAP** | Directory-based auth + group sync | Author/Publish | LDAP/LDAPS |
| **OAuth 2.0** | Third-party identity providers | Publish | OAuth tokens |
| **Adobe IMS** | Adobe ecosystem authentication | Author | Adobe identity |
| **Generic SSO** | Custom SSO via HTTP headers | Both | Custom headers |
| **Form-based** | Default AEM login | Author | Username/password |

### SAML 2.0 Authentication (Exam Critical)

```
SAML 2.0 Flow:
┌────────┐     ┌────────────────────┐     ┌──────────────┐
│ Browser │ ──▶ │ AEM Publish (SP)    │ ──▶ │ IdP (Okta/   │
│         │     │                     │     │ Azure AD/     │
│         │     │ Adobe Granite SAML  │     │ ADFS)         │
│         │ ◀── │ Authentication      │ ◀── │              │
│         │     │ Handler             │     │              │
└────────┘     └────────────────────┘     └──────────────┘

Configuration (OSGi):
  PID: com.adobe.granite.auth.saml.SamlAuthenticationHandler
  ├── idpUrl: IdP SSO endpoint
  ├── idpCertAlias: IdP certificate in AEM truststore
  ├── spPrivateKeyAlias: AEM service provider private key
  ├── userIDAttribute: SAML attribute for user ID
  ├── groupMembershipAttribute: SAML attribute for groups
  ├── assertionConsumerServiceURL: AEM callback URL
  ├── serviceProviderEntityId: AEM SP entity ID
  ├── defaultRedirectUrl: Post-login redirect
  └── createUser: Auto-create users on first login

AEMaaCS SAML:
  • Same SAML 2.0 protocol
  • Configuration deployed via CI/CD (not Felix Console)
  • Certificates managed via Cloud Manager
  • Additional documentation at Experience League
```

### Authorization Architecture

```
Permission Model:
├── Users
│   ├── Local users (AEM internal)
│   ├── LDAP-synced users
│   └── SAML auto-provisioned users
├── Groups
│   ├── System groups (dam-users, content-authors, etc.)
│   ├── Custom groups (site-specific)
│   └── LDAP/SAML-synced groups
├── Service Users (no login, programmatic access)
│   ├── Mapped via Apache Sling Service User Mapper
│   ├── Principle of least privilege
│   └── No admin access for service users
└── Permissions (ACLs)
    ├── Allow / Deny
    ├── Applied at path level
    ├── Inherited by child nodes
    └── Deny overrides Allow (at same level)
```

### ACL (Access Control List) Best Practices

```
ACL Design Principles:
├── Deny all by default
├── Grant specific permissions at appropriate paths
├── Use groups (never per-user ACLs)
├── Keep ACL structure simple and maintainable
├── Test with "Test Access Control" in CRXDE
└── Document ACL matrix

Example ACL Structure:
/content/mysite
├── Allow: content-authors → jcr:read, rep:write
├── Allow: content-publishers → jcr:read, rep:write, crx:replicate
└── /content/mysite/restricted
    ├── Deny: everyone → jcr:read
    └── Allow: restricted-editors → jcr:read, rep:write
```

### Closed User Groups (CUG)

```
CUG Configuration:
├── Purpose: Restrict read access on publish to specific user groups
├── Applied at page level
├── All descendant pages inherit restriction
├── Users not in CUG group → 403 Forbidden (or redirect to login)
├── Requires authentication on publish tier
└── Configuration:
    Page Properties → Permissions → CUG → Add group(s)

Use Cases:
├── Partner portals (only partner users can access)
├── Intranet sections (only employees)
├── Premium content (only subscribers)
└── Preview environments (only reviewers)

CUG in AEMaaCS:
├── Supported but with limitations
├── Requires publish-side authentication
└── Consider edge authentication alternatives
```

## Security Hardening

### OWASP Top 10 in AEM Context

| Vulnerability | AEM Mitigation |
|---|---|
| **Injection** | Parameterized queries (Query Builder), HTL auto-escaping |
| **Broken Auth** | SAML/IMS, session management, CUG |
| **Sensitive Data** | HTTPS enforcement, data encryption at rest |
| **XXE** | Disabled by default in modern AEM |
| **Broken Access Control** | ACLs, service users, Dispatcher filters |
| **Security Misconfig** | Security Checklist, Dispatcher hardening |
| **XSS** | HTL context-aware escaping, CSP headers |
| **Insecure Deserialization** | OSGi framework isolation, bundle security |
| **Known Vulnerabilities** | Regular service pack updates |
| **Insufficient Logging** | Audit logging, Operations Dashboard |

### Security Checklist (Adobe Official)

```
AEM Security Checklist:
├── Default Admin Password
│   └── Change immediately after installation
├── CRX/DE Access
│   └── Disable on production (Dispatcher blocks /crx/*)
├── Apache Sling Referrer Filter
│   ├── Configure allowed hosts
│   └── Block empty referrer (configurable)
├── CSRF Protection
│   ├── AEM CSRF Framework enabled
│   ├── Token-based validation
│   └── Whitelist safe methods (GET, HEAD)
├── Clickjacking
│   └── X-Frame-Options: SAMEORIGIN (Dispatcher)
├── SSL/TLS
│   ├── HTTPS enforced
│   ├── HSTS header enabled
│   └── TLS 1.2+ only
├── Service Users
│   ├── Replace admin sessions with service users
│   ├── Minimal permissions per service
│   └── Mapped via Service User Mapper
├── Operations Dashboard
│   ├── Security Health Check
│   ├── Bundle health
│   └── Configuration checks
└── Dispatcher Security
    ├── Filter rules (deny by default)
    ├── Block sensitive paths
    └── Header management
```

### CSRF Protection

```
CSRF Framework:
├── AEM provides token-based CSRF protection
├── Token requested via: /libs/granite/csrf/token.json
├── Token included in form submissions
├── Validated server-side
├── Dispatcher must allow the token endpoint
└── Configuration:
    Adobe Granite CSRF Filter
    ├── Allowed paths (whitelist)
    ├── Safe methods (GET, HEAD, OPTIONS)
    └── Enabled: true (default)
```

---

### Practice Scenarios

**Scenario 1:** A company wants to implement SSO for their AEM publish tier using their existing Okta identity provider. What should the Architect configure?

<details>
<summary>Answer</summary>

**Implement SAML 2.0 Authentication with Okta as IdP:**

1. **Okta Configuration:**
   - Create SAML application in Okta
   - Configure AEM as Service Provider (SP)
   - Define attribute mapping (email, groups)
   - Download IdP metadata/certificate

2. **AEM Configuration:**
   - Import IdP certificate into AEM Truststore
   - Create AEM SP key pair in Keystore
   - Configure `Adobe Granite SAML 2.0 Authentication Handler`:
     - idpUrl: Okta SSO URL
     - idpCertAlias: Imported cert alias
     - userIDAttribute: email
     - groupMembershipAttribute: groups
     - createUser: true (auto-provision)
   - Configure Apache Sling Authentication Requirements

3. **Dispatcher Configuration:**
   - Allow SAML callback URL
   - Pass authentication headers

4. **Testing:**
   - Verify SP-initiated SSO flow
   - Verify IdP-initiated SSO flow
   - Test group mapping and CUG access
</details>

---

*[Back to AD0-E117 Index](../README.md)*
