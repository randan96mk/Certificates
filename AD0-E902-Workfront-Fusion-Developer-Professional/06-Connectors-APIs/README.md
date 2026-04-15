# Domain 4: Working with APIs (10% — ~5 questions)

[Back to AD0-E902 Index](../README.md)

---

## HTTP Module — Universal API Connector

### HTTP Module Variants

| Module | Authentication | Use When |
|---|---|---|
| **Make a Request** | Manual (headers/params) | Full control over request |
| **Make a Basic Auth Request** | Username + Password | Simple APIs with Basic Auth |
| **Make an OAuth 2.0 Request** | OAuth 2.0 flow | APIs requiring OAuth tokens |
| **Make a Client Certificate Auth** | Client certificate | mTLS APIs |
| **Retrieve Headers** | N/A | Inspect response headers |
| **Resolve Target URL** | N/A | Follow redirects, get final URL |
| **Make an API Key Auth Request** | API key | APIs with key-based auth |

### Make a Request — Configuration

```
HTTP Make a Request:
├── URL: https://api.example.com/v1/records
├── Method: GET / POST / PUT / PATCH / DELETE
├── Headers:
│   ├── Content-Type: application/json
│   ├── Authorization: Bearer {{connection.token}}
│   └── Custom-Header: value
├── Query String:
│   ├── limit: 100
│   ├── offset: 0
│   └── filter: status=active
├── Body:
│   ├── Type: Raw / JSON / Form Data / Multipart
│   └── Content: {{JSON}} or form fields
├── Parse Response: Yes (auto-parse JSON)
├── Timeout: 300 seconds max
└── Follow Redirects: Yes/No
```

### Response Handling

```
HTTP Response Fields:
├── data (parsed body — if Parse Response = Yes)
├── headers (response headers collection)
├── statusCode (200, 201, 400, 404, 500, etc.)
└── body (raw string — if Parse Response = No)

Status Code Handling:
├── 2xx → Success (continue scenario)
├── 3xx → Redirect (follow or handle)
├── 4xx → Client error (data/auth issue)
│   ├── 400 Bad Request → check body format
│   ├── 401 Unauthorized → refresh token
│   ├── 403 Forbidden → check permissions
│   ├── 404 Not Found → record doesn't exist
│   └── 429 Rate Limited → add Sleep, reduce frequency
├── 5xx → Server error (transient, retry)
│   ├── 500 Internal Server Error → retry via Break
│   └── 503 Service Unavailable → retry via Break
└── Use Router after HTTP to branch by statusCode
```

## JSON Handling

### JSON Modules

```
Parse JSON:
├── Input: JSON string (raw text)
├── Output: Structured data (mappable fields)
├── Data Structure: Required (define expected schema)
├── Example:
│   Input: '{"name":"John","age":30}'
│   Output: { name: "John", age: 30 }
└── USE WHEN: HTTP response with Parse=No, or webhook raw body

Create JSON:
├── Input: Mapped fields from previous modules  
├── Output: JSON string
├── Data Structure: Defines the output format
├── USE WHEN: Building request body for HTTP module

Transform JSON:
├── Input: JSON string
├── Output: Transformed JSON string
├── USE WHEN: Restructuring complex JSON responses
```

### JSON Tips for Exam

```
Common JSON Patterns:
├── Access nested: {{1.data.user.name}}
├── Access array: {{1.data.items[1].id}}  (1-based index)
├── Array length: {{length(1.data.items)}}
├── First item: {{first(1.data.items)}}
├── Map array: {{map(1.data.items; "name")}}  → array of names
└── Stringify: {{toString(1.data)}}  → JSON string
```

## Webhooks

### Custom Webhook

```
Custom Webhook Setup:
1. Add "Custom Webhook" trigger module
2. Fusion generates unique URL:
   https://hook.us1.make.com/xxxxxxxxxxxxxxxx
3. Configure in external system to POST to this URL
4. Define Data Structure (expected payload schema)
5. First request: "Redetermine data structure" to auto-detect

Webhook Features:
├── Instant execution (no polling delay)
├── No operations consumed for the trigger
├── Unique URL per webhook instance
├── IP whitelist support (security)
├── Custom response: send HTTP response back to caller
├── Queue: requests queued if scenario is running
└── Rate limit: 100 requests/second typical
```

### Webhook Response Module

```
Webhook Response:
├── Placed at end of scenario (or in error handler)
├── Returns HTTP response to the webhook caller
├── Configure:
│   ├── Status Code: 200, 201, 400, 500, etc.
│   ├── Headers: Content-Type, etc.
│   └── Body: JSON, text, or empty
├── Use case: Confirm receipt to external system
│   └── External system gets { "status": "received", "id": "123" }
└── Without this: caller gets default 200 OK (empty)
```

### Webhook vs Custom Mailhook vs Custom Webhook

| Type | Trigger | Use Case |
|---|---|---|
| **Custom Webhook** | HTTP POST/GET to URL | Generic API integration |
| **Custom Mailhook** | Email to Fusion address | Email-triggered workflows |
| **App-specific Webhook** | App events (e.g., Workfront) | App-native instant triggers |

## Authentication Methods

### Connection Types

```
Connection Architecture:
├── API Key
│   ├── Key sent in header or query param
│   ├── Simple, no token refresh
│   └── Example: X-API-Key: abc123
├── Basic Auth
│   ├── Username:Password base64-encoded
│   ├── Sent in Authorization header
│   └── Authorization: Basic dXNlcjpwYXNz
├── OAuth 2.0 (Authorization Code)
│   ├── User authorizes Fusion in browser
│   ├── Fusion receives auth code → exchanges for tokens
│   ├── Auto-refresh when token expires
│   └── Most common for SaaS apps (Google, Slack, etc.)
├── OAuth 2.0 (Client Credentials)
│   ├── Server-to-server (no user interaction)
│   ├── Client ID + Secret → access token
│   └── Used for service accounts
└── Custom Authentication
    ├── Manual header construction
    ├── Token management via variables/data stores
    └── For non-standard APIs
```

### Connection Management

```
Best Practices:
├── Create ONE connection per service per environment
├── Name connections clearly: "Workfront-Production", "Jira-Dev"
├── Re-use connections across scenarios (don't duplicate)
├── Monitor connection health (expiring tokens)
├── Rotate credentials via connection settings
└── Team-level connection sharing
```

## Common App Connectors

### Workfront Connector Modules

| Module | Purpose |
|---|---|
| Watch Records | Poll for new/updated records |
| Watch Events | Instant trigger via event subscriptions |
| Read a Record | Get single record by ID |
| Search Records | Find records by criteria |
| Create a Record | Create new object |
| Update a Record | Modify existing object |
| Delete a Record | Remove object |
| Upload Document | Upload file to WF document |
| Download Document | Download WF document |
| Custom API Call | Raw API call to any WF endpoint |
| Misc Action | Special actions (convert issue, etc.) |

### Key Connector Differences

| Feature | App Connector | HTTP Module |
|---|---|---|
| **Setup** | Pre-built, easy | Manual configuration |
| **Auth** | Managed connection | Manual auth handling |
| **Fields** | Auto-discovered | Must know API schema |
| **Errors** | App-specific messages | Raw HTTP errors |
| **Speed** | Optimized | Variable |
| **Flexibility** | Limited to provided modules | Full API access |
| **Use when** | Connector exists and is sufficient | No connector OR need custom endpoints |

---

### Practice Scenarios

**Scenario 1:** You need to call an API that requires OAuth 2.0 with client credentials (no user login). The API returns paginated results with a `next_page_url` field. Design the scenario.

<details><summary>Answer</summary>

```
1. HTTP: Make an OAuth 2.0 Request
   - Grant Type: Client Credentials
   - Client ID + Secret configured in connection
   - URL: https://api.example.com/records?page=1
   - Method: GET

2. Set Variable: "hasNextPage"
   - Value: {{if(1.data.next_page_url != null; true; false)}}

3. Set Variable: "nextURL"
   - Value: {{1.data.next_page_url}}

4. Iterator: Split response array
   - Array: {{1.data.records}}

5. Action: Create record per item

6. Aggregator: Collect results

   --- For pagination, wrap steps 1-6 in a loop: ---

Alternative: Use Repeater with break condition
  Repeater (50 max) →
    HTTP: Get (URL from variable or initial) →
    Iterator + Action →
    Router:
      ├── Has next_page_url: Set Variable to next URL
      └── No next_page_url: end (Break error deliberately)
```
</details>

**Scenario 2:** A webhook receives data but the scenario takes 30 seconds to process. The external system times out after 5 seconds. How do you handle this?

<details><summary>Answer</summary>

Add a **Webhook Response** module immediately after the trigger:

```
Custom Webhook (trigger)
  │
  ▼
Webhook Response (immediately returns 200 OK)
  Body: { "status": "accepted", "message": "Processing..." }
  │
  ▼
[... rest of scenario (30 second processing) ...]
```

The external system receives an immediate 200 response and doesn't time out. The scenario continues processing asynchronously after sending the response.
</details>

---

*[Back to AD0-E902 Index](../README.md)*
