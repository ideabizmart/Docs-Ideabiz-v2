# IdeaBiz MSISDN Detection API — Integration Documentation

## Overview

The MSISDN Detection API enables vendors to identify mobile subscribers via Header Enrichment (HE) when users browse through the mobile data network. The integration uses a two-step approach:

1. **Web redirect** — detect the user's MSISDN via browser redirect chain
2. **Backend API call** — retrieve the detected MSISDN from your server

**Domains:**

| Domain | Purpose |
|--------|---------|
| `consent.ideamart.io` | Web redirect endpoints (user browser) |
| `ideabiz.lk` | Backend API endpoints (server-to-server) |

---

## Quick Start

```
1. Get your client_id from IdeaBiz team
2. Subscribe to "MSISDN Detect" API on IdeaBiz Store
3. Redirect user browser → consent.ideamart.io/web/v1/authorize?client_id=X&ref=Y
4. Receive callback with status and unique_ref
5. Call ideabiz.lk API with unique_ref → get MSISDN
```

---

## Step 1: Web Redirect (consent.ideamart.io)

Redirect the user's browser to start MSISDN detection. No server-side auth needed — your app is identified by `client_id`.

### Option A: Browser Redirect (default)

For web apps that have a callback URL:

### Authorize Endpoint

```
GET https://consent.ideamart.io/web/v1/authorize?client_id={client_id}&ref={campaign_ref}
    [&service_code={service_code}&service_name={service_name}&p_<key>=<value>]
```

`service_code`, `service_name`, and `p_<key>` are optional (see the parameter table
below). After detection, the user is redirected to your configured callback URL with
params — including any `service_code` / `service_name` / `p_<key>` you sent.

### Option B: JSON Mode (no redirect URL)

For backend-only or testing integrations that don't have a web callback:

Set `redirect_urls = "JSON"` in your app config. After detection, the service returns a JSON response at its own `/web/v1/result/{session}` endpoint instead of redirecting:

```
GET https://consent.ideamart.io/web/v1/result/{session_id}
```

Response:
```json
{
  "ref": "your-campaign-ref",
  "unique_ref": "HE-925a9eb8-42832",
  "status": "DETECTED",
  "session_id": "d66328ee-2d45-4801-90bf-f2d30979c31d",
  "timestamp": "2026-06-11T12:00:00Z"
}
```

> In JSON mode, cookie/localStorage detection is disabled (HE only). The user's browser will land on the JSON result page after the flow completes.

---

### Authorize Endpoint

```
GET https://consent.ideamart.io/web/v1/authorize?client_id={client_id}&ref={campaign_ref}
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| `client_id` | Yes | Your UUID (provided during onboarding) |
| `ref` | Yes | Your campaign/session reference. Returned unchanged in callback. |
| `service_code` | No | Optional platform service code identifying the service on your side (e.g. a webwidget UUID or 32-char hex code). Max 64 chars. Stored on the transaction and echoed back on the callback, the JSON result (JSON-mode apps), and the server notification. |
| `service_name` | No | Optional platform service name. Max 64 chars. Echoed back like `service_code`. |
| `p_<key>` | No | Optional custom params (up to **5**). Any query param prefixed `p_` is captured as a custom key/value — e.g. `p_region=LK&p_tier=gold`. Each value is capped at 100 chars (longer values are truncated). Echoed back on the callback (`p_<key>`), the JSON result / server notification (`params`). |
| `protocol` | No | Force detect protocol: `http` or `https`. Only works if enabled for your app. Default: auto. |
| `security_vendor` | No | Anti-fraud vendor name, e.g. `evina`. Only if your app has security validation enabled. |
| `security_ref` | Conditional | Reference issued by the security vendor. Required when your app is configured with `security_validation=REQUIRED`. Validated at the finish step. |

> **Service identity + custom params** (`service_code`, `service_name`, `p_<key>`) are
> all optional. When sent, they are persisted and echoed back on the browser callback,
> the JSON result page (JSON-mode apps), and the server notification — so you can
> correlate the result to your own service and request. (The TMF `/status` lookup
> response does not carry them; use the callback or notification for echo-back.) On an
> AAA lookup they
> are also forwarded to the internal resolver (your values ride as
> `remote_service_code` / `remote_service_name`), so the resolver record ties back to
> your service too.

### What Happens

The user's browser is automatically redirected through the detection chain (invisible to the user, takes < 1 second):

```
consent.ideamart.io/web/v1/authorize → (WAF detection) → (security validation, if enabled) → callback to your site
```

### Callback to Your Site

After detection completes, the user is redirected to your configured callback URL:

```
https://yoursite.com/callback?ref={campaign_ref}&unique_ref={unique_ref}&status={status}
```

If you sent `service_code` / `service_name` / `p_<key>` on authorize, they are appended
to the callback too, e.g.:

```
https://yoursite.com/callback?ref={campaign_ref}&unique_ref={unique_ref}&status={status}&service_code={service_code}&service_name={service_name}&p_region=LK
```

| Parameter | Description |
|-----------|-------------|
| `ref` | Your original campaign reference (unchanged) |
| `unique_ref` | Transaction reference for API lookup, `HE-{uuid}` (e.g. `HE-e1f6df42-a686-4dea-89f4-d32be009f3fb`) |
| `status` | `DETECTED` or `NOT_DETECTED` |
| `service_code` / `service_name` | Echoed back on the callback when you sent them on authorize. Omitted when not sent. |
| `p_<key>` | Each custom param you sent is echoed back on the callback as `p_<key>=<value>`. |
| `code` | Present when security validation is enabled: `S1000`/`S1001` (normal), `S1002` (vendor said invalid — blocked), `S1003` (vendor error, continued), `E1005` (vendor error, blocked). |

> The redirect always happens regardless of detection outcome. Check the `status` parameter.

### Security Validation (Optional)

If your app is onboarded with an anti-fraud vendor (e.g. Evina), pass `security_vendor` and `security_ref` on the authorize URL. The service validates the reference with the vendor at the finish step:

- Vendor says **invalid** → callback `status=NOT_DETECTED&code=S1002` (always blocked).
- Vendor **errors** and your app is `CONTINUE` → real result with `code=S1003`.
- Vendor **errors** and your app is `BLOCK` → `status=NOT_DETECTED&code=E1005`.

The backend lookup API returns the same distinction via `statusCode` (`S1002`/`S1003`/`E1005`) plus `securityFlag: true`.

---

## Step 2: Backend API Lookup (ideabiz.lk)

From your backend server, call the API with the `unique_ref` to retrieve the MSISDN.

### Authentication

Obtain an OAuth2 Bearer token:

```bash
curl -X POST "https://ideabiz.lk/apicall/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials&client_id=YOUR_CONSUMER_KEY&client_secret=YOUR_CONSUMER_SECRET"
```

Response:
```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOi...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

---

### API Option A: IdeaBiz Format

**API Name:** MSISDN Detect  
**Version:** v1

```
GET https://ideabiz.lk/apicall/msisdndetect/v1/status?ref={unique_ref}
Authorization: Bearer {access_token}
```

#### Success — DETECTED

```json
{
  "status": "SUCCESS",
  "statusCode": "S1000",
  "msisdn": {
    "value": "IB-A-000A-6a537144-e3be-4728-a136-b94d11083bf3",
    "mask": "9477*****67",
    "connectionType": "UNKNOWN",
    "platform": "IB",
    "format": "ATEL",
    "rvalue": "IB-A-000A-697-648-774-1704",
    "operator": "Dialog",
    "detectionMethod": "AAA",
    "sessionId": "d66328ee-2d45-4801-90bf-f2d30979c31d",
    "identityRef": "e3b0c442-98fc-1c14-9afb-4c8996fb9242"
  }
}
```

**`detectionMethod`** reports how the MSISDN was detected:

| Value | Meaning |
|-------|---------|
| `HE` | Header enrichment over the mobile network |
| `AAA` | RADIUS IP→MSISDN resolver (mobile data) |
| `AAA-C6` | RADIUS resolve via the consent6 IPv6 capture |
| `BROWSER` | Cookie / localStorage identity recall |

**`connectionType`** is reserved for the subscriber's billing class
(`PREPAID` / `POSTPAID`) and currently returns `UNKNOWN` until that signal is
available from the network.

**`sessionId`** is the internal session id of this detection's transaction (the same
`session_id` seen on the result/callback) — it identifies the transaction itself.

**`identityRef`** is a separate per-transaction UUID that maps this transaction to its
**browser identity**. Hand it to **ideamart-dot**; ideamart-dot sends it back to
correlate the same browser/subscriber across requests. Distinct from `sessionId`: the
session id names the txn, while `identityRef` is the identity-mapping ref. Present on a
`DETECTED` response (a browser identity is attached on every detection).

#### Success — NOT DETECTED

```json
{
  "status": "SUCCESS",
  "statusCode": "S1001",
  "message": "MSISDN not detected"
}
```

#### Security Validation Status Codes (if enabled)

Only relevant when your app is onboarded with an anti-fraud vendor. `securityFlag: true` marks any transaction where a security event occurred.

| Code | HTTP | Meaning |
|------|------|---------|
| `S1002` | 200 | Vendor said **invalid** — treated as not-detected, always blocked. `{ "status": "SUCCESS", "statusCode": "S1002", "message": "security validation failed", "securityFlag": true }` |
| `S1003` | 200 | Vendor **errored**, app set to `CONTINUE` — legit result returned with `securityFlag: true`. `msisdn` present if detected. |
| `E1005` | 200 | Vendor **errored**, app set to `BLOCK` — not-detected. `{ "status": "FAILED", "statusCode": "E1005", "message": "security validation unavailable", "securityFlag": true }` |

#### Errors

| HTTP | Code | Description |
|------|------|-------------|
| 400 | E4001 | Missing `ref` query parameter |
| 401 | E4010 | Missing Bearer/JWT token |
| 401 | E4011 | Invalid or expired token |
| 403 | E4030 | Application not registered / not authorized for this ref |
| 404 | E4040 | Transaction not found for given ref |
| 500 | E5002 | Number format conversion failed |

> All error bodies follow `{ "status": "FAILED", "statusCode": "<code>", "message": "..." }`.

---

### Lookup by Vendor Reference

Use this to find our `unique_ref` using your own campaign reference — useful for transactions that terminated mid-flow (user dropped off before callback).

```
GET https://ideabiz.lk/apicall/msisdndetect/v1/lookup?ref={your_campaign_ref}
Authorization: Bearer {access_token}
```

#### Response (200 OK)

```json
{
  "status": "SUCCESS",
  "statusCode": "S1000",
  "vendor_ref": "your-campaign-ref-123",
  "count": 1,
  "transactions": [
    {
      "session_id": "19239b60-11e6-4118-a6fa-aa05e212ddf6",
      "unique_ref": "HE-072b413b-90079",
      "status": "DETECTED",
      "flow_step": "REDIRECTED",
      "created_at": "2026-06-09 18:18:10"
    }
  ]
}
```

#### Response — Not Found (404)

```json
{
  "status": "FAILED",
  "statusCode": "E4040",
  "message": "no transactions found for given ref"
}
```

> Results are scoped to your application — you can only see transactions created with your `client_id`/`api_mgr_id`.

---

### API Option B: TMF681 Format

**API Name:** MSISDN Detect TMF  
**Version:** v1

```
GET https://ideabiz.lk/apicall/msisdndetect-tmf/v1/status?ref={unique_ref}
Authorization: Bearer {access_token}
```

#### Success — DETECTED

```json
{
  "id": "HE-1a3d1f87-36511",
  "href": "/tmf/v1/status?ref=HE-1a3d1f87-36511",
  "@type": "IndividualIdentification",
  "identificationId": "93620143-837e-42a3-9e0d-104bad6a3114",
  "lifecycleStatus": "Completed",
  "validFor": {
    "startDateTime": "2026-06-09T12:30:00Z"
  },
  "verificationResult": true,
  "attachment": {
    "id": "HE-1a3d1f87-36511",
    "name": "MSISDN",
    "mimeType": "application/json",
    "description": "Detected mobile subscriber number"
  },
  "identifiedPerson": {
    "@type": "Individual",
    "contactMedium": [{
      "mediumType": "mobile",
      "preferred": true,
      "characteristic": {
        "phoneNumber": "IB-A-000A-6a537144-e3be-4728-a136-b94d11083bf3",
        "phoneNumberMask": "9477*****67",
        "phoneNumberFormat": "ATEL",
        "operator": "Dialog"
      }
    }]
  }
}
```

#### Success — NOT DETECTED

```json
{
  "id": "HE-1a3d1f87-36511",
  "href": "/tmf/v1/status?ref=HE-1a3d1f87-36511",
  "@type": "IndividualIdentification",
  "identificationId": "93620143-837e-42a3-9e0d-104bad6a3114",
  "lifecycleStatus": "Completed",
  "validFor": {
    "startDateTime": "2026-06-09T12:30:00Z"
  },
  "verificationResult": false
}
```

#### Errors

| HTTP | Code | Description |
|------|------|-------------|
| 400 | ERR-0001 | Missing required parameter (`ref`) |
| 401 | E4010 | Missing Bearer/JWT token (auth layer, IdeaBiz-format body) |
| 401 | E4011 | Invalid or expired token (auth layer, IdeaBiz-format body) |
| 403 | E4030 | Application not registered (auth layer, IdeaBiz-format body) |
| 404 | ERR-0004 | Resource not found |
| 500 | ERR-0005 | Internal error |

> Auth failures are enforced before the TMF handler runs, so their bodies use the IdeaBiz `statusCode` format (`E40xx`), not the TMF `code` format. Everything past auth uses TMF `ERR-000x` codes.

---

## Error Code Reference

Consolidated list of every code a developer can encounter across the web flow and both backend API formats.

### Web Redirect `status` (callback to your site)

| `status` | Meaning |
|----------|---------|
| `DETECTED` | MSISDN detected |
| `NOT_DETECTED` | MSISDN could not be detected (WiFi, no header, HTTPS, etc.) |
| `INVALID_STATE` | Session accessed out-of-order or replayed (silent fraud handling) |

### Backend API — IdeaBiz Format (`statusCode`)

| Code | HTTP | Status | Meaning |
|------|------|--------|---------|
| `S1000` | 200 | SUCCESS | MSISDN detected |
| `S1001` | 200 | SUCCESS | MSISDN not detected |
| `E4001` | 400 | FAILED | Missing `ref` query parameter |
| `E4010` | 401 | FAILED | Missing Bearer/JWT token |
| `E4011` | 401 | FAILED | Invalid or expired token |
| `E4030` | 403 | FAILED | Application not registered / not authorized for this ref |
| `E4040` | 404 | FAILED | Transaction not found for given ref |
| `E5002` | 500 | FAILED | Number format conversion / encryption failed |

Body shape: `{ "status": "SUCCESS|FAILED", "statusCode": "<code>", "message": "..." }`.

### Backend API — TMF681 Format (`code`)

| Code | HTTP | Meaning |
|------|------|---------|
| `ERR-0001` | 400 | Missing required parameter (`ref`) |
| `ERR-0004` | 404 | Resource not found |
| `ERR-0005` | 500 | Internal error |

> TMF auth failures return the IdeaBiz-format `E40xx` codes (auth runs before the TMF handler). TMF `code` values apply only past authentication.
>
> Error body shape: `{ "code": "ERR-000x", "reason": "...", "message": "...", "status": "<http>" }`.

### Security Validation Codes (planned — not yet active)

These codes are reserved for optional anti-fraud vendor (e.g. Evina) validation. **Not currently emitted** — documented so integrators can prepare. They appear only for apps onboarded with a security vendor once the feature ships.

| Code | Format | Meaning |
|------|--------|---------|
| `S1002` | IdeaBiz / redirect `code` | Vendor said invalid — not-detected, always blocked, `securityFlag: true` |
| `S1003` | IdeaBiz / redirect `code` | Vendor errored, app set to CONTINUE — legit result, `securityFlag: true` |
| `E1005` | IdeaBiz / redirect `code` | Vendor errored, app set to BLOCK — not-detected, `securityFlag: true` |

> The internal reference `docs/Error_Codes.md` holds the complete cross-endpoint matrix, including the AAA resolver and admin API codes.

---

## MSISDN Formats

The format of the returned MSISDN value depends on your service configuration:

| Format | Example | Use Case |
|--------|---------|----------|
| ATEL | `IB-A-000A-6a537144-e3be-4728-a136-b94d11083bf3` | Recommended. App-level token. |
| STEL | `IB-S-0064-d2f8a1b3-...` | Service-level token isolation. |
| ETEL | `9477-vl%1D%A3%F1%AD%E1%A3%C5%A8%FB` | Encrypted token. |
| PLAIN | `94771234567` | Raw number (restricted, internal apps only). |

The `mask` field always shows a masked number (e.g. `9477*****67`) regardless of format.
The `operator` field identifies the mobile operator (e.g. `Dialog`, `Mobitel`, `Hutch`, `Airtel`).

---

## Optional: Precheck & Subscription Status

If configured for your app, the API response may include additional precheck and/or subscription status data.

### With Precheck Status

```json
{
  "status": "SUCCESS",
  "statusCode": "S1000",
  "msisdn": { "value": "...", "mask": "9477*****67", "operator": "Dialog" },
  "precheckStatus": {
    "status": "OK",
    "message": "Access granted",
    "statusCode": "S1000"
  }
}
```

### With Subscription Status

```json
{
  "status": "SUCCESS",
  "statusCode": "S1000",
  "msisdn": { "value": "...", "mask": "9477*****67", "operator": "Dialog" },
  "subscriptionStatus": {
    "status": "OK",
    "message": "Access granted",
    "subscription": {
      "application": "YES",
      "serviceSubscription": "YES"
    },
    "statusCode": "S1000"
  }
}
```

### With Both

```json
{
  "status": "SUCCESS",
  "statusCode": "S1000",
  "msisdn": { "value": "...", "mask": "9477*****67", "operator": "Dialog" },
  "precheckStatus": {
    "status": "OK",
    "message": "Access granted",
    "statusCode": "S1000"
  },
  "subscriptionStatus": {
    "status": "OK",
    "message": "Access granted",
    "subscription": {
      "application": "YES",
      "serviceSubscription": "YES"
    },
    "statusCode": "S1000"
  }
}
```

> These fields are optional and only included if your app is configured with `additional_response` set to `PRECHECK_STATUS`, `SUBSCRIPTION_STATUS`, or `PRECHECK_AND_SUBSCRIPTION`. Contact the IdeaBiz team to enable.

---

## Code Examples

### PHP

```php
<?php
// Step 1: Redirect user
$clientId = 'a1b2c3d4-e5f6-7890-abcd-ef1234567890';
$campaignRef = session_id();
header("Location: https://consent.ideamart.io/web/v1/authorize?client_id={$clientId}&ref=" . urlencode($campaignRef));
exit;
```

```php
<?php
// Step 2: Handle callback and lookup MSISDN
$status = $_GET['status'];
$uniqueRef = $_GET['unique_ref'];

if ($status === 'DETECTED') {
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, "https://ideabiz.lk/apicall/msisdndetect/v1/status?ref=" . urlencode($uniqueRef));
    curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: Bearer " . $accessToken]);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    $response = json_decode(curl_exec($ch), true);
    curl_close($ch);

    if ($response['statusCode'] === 'S1000') {
        $msisdnToken = $response['msisdn']['value'];
        $maskedNumber = $response['msisdn']['mask'];
        // Use $msisdnToken to identify the user
    }
}
```

### Node.js

```javascript
const CLIENT_ID = 'a1b2c3d4-e5f6-7890-abcd-ef1234567890';

// Step 1: Redirect user
app.get('/detect', (req, res) => {
  const ref = req.sessionID;
  res.redirect(`https://consent.ideamart.io/web/v1/authorize?client_id=${CLIENT_ID}&ref=${ref}`);
});

// Step 2: Handle callback
app.get('/callback', async (req, res) => {
  const { status, unique_ref } = req.query;

  if (status === 'DETECTED') {
    const resp = await fetch(
      `https://ideabiz.lk/apicall/msisdndetect/v1/status?ref=${unique_ref}`,
      { headers: { 'Authorization': `Bearer ${accessToken}` } }
    );
    const data = await resp.json();
    
    if (data.statusCode === 'S1000') {
      console.log('MSISDN Token:', data.msisdn.value);
      console.log('Masked:', data.msisdn.mask);
    }
  } else {
    console.log('MSISDN not detected (user on WiFi)');
  }
});
```

### Python

```python
import requests

# Step 2: Backend lookup
def lookup_msisdn(unique_ref, access_token):
    resp = requests.get(
        f"https://ideabiz.lk/apicall/msisdndetect/v1/status?ref={unique_ref}",
        headers={"Authorization": f"Bearer {access_token}"}
    )
    data = resp.json()
    
    if data.get("statusCode") == "S1000":
        return {
            "token": data["msisdn"]["value"],
            "mask": data["msisdn"]["mask"],
            "format": data["msisdn"]["format"],
        }
    return None
```

### Java

```java
// Step 2: Backend lookup
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://ideabiz.lk/apicall/msisdndetect/v1/status?ref=" + uniqueRef))
    .header("Authorization", "Bearer " + accessToken)
    .GET()
    .build();

HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
JsonObject json = JsonParser.parseString(response.body()).getAsJsonObject();

if ("S1000".equals(json.get("statusCode").getAsString())) {
    JsonObject msisdn = json.getAsJsonObject("msisdn");
    String token = msisdn.get("value").getAsString();
    String mask = msisdn.get("mask").getAsString();
}
```

---

## Important Notes

- MSISDN detection **only works on mobile data** (3G/4G/5G). WiFi users will return `NOT_DETECTED`.
- The `unique_ref` is for **one-time lookup**. Each detection generates a new reference.
- The redirect flow is invisible to the user (< 1 second, HTTP 302 chain).
- Your callback URL must be pre-registered with the IdeaBiz team.
- The `ref` parameter you send is for your own tracking — it's returned unchanged.
- `Referer` header from the user's browser must match your configured referral domains.

---

## Server-to-Server Notification (Optional)

If configured, a **POST notification** is automatically sent to your server when the detection flow completes (at the redirect step). This is useful as a backup — even if the user's browser doesn't reach your callback, your server still gets notified.

### Configuration

Provide your notification URL to the IdeaBiz team. It will be configured per-service.

### Notification Request

```http
POST https://yoursite.com/api/he-notify
Content-Type: application/json

{
  "unique_ref": "HE-1a3d1f87-36511",
  "campaign_ref": "your-campaign-ref-123",
  "status": "DETECTED",
  "session_id": "93620143-837e-42a3-9e0d-104bad6a3114",
  "timestamp": "2026-06-09T12:30:45Z",
  "service_code": "551e955ef4a7d762327569556a202cda",
  "service_name": "ShemarooMe",
  "params": { "region": "LK", "tier": "gold" }
}
```

| Field | Description |
|-------|-------------|
| `unique_ref` | Our transaction reference (use for `/status` API call) |
| `campaign_ref` | Your original `ref` parameter from authorize |
| `status` | `DETECTED` or `NOT_DETECTED` |
| `session_id` | Internal session ID |
| `timestamp` | UTC timestamp of redirect |
| `service_code` / `service_name` | Echoed when you sent them on authorize. Omitted otherwise. |
| `params` | Your custom `p_<key>` params as a `{ key: value }` object. Omitted when none were sent. |

### Behavior

- Sent **async** — does not block the user's redirect
- **2-second timeout** — if your server doesn't respond within 2s, the notification is abandoned
- Response body is **not read** — only HTTP status code is logged
- Fires for both `DETECTED` and `NOT_DETECTED` outcomes
- Logged in our session events for troubleshooting

### Your Endpoint Requirements

- Accept `POST` with JSON body
- Return `2xx` status code to indicate receipt
- Must respond within 2 seconds
- No retry mechanism (fire-and-forget)

---

## Lookup by Your Campaign Reference

If a user dropped off mid-flow (browser closed, network issue) and your callback never fired, you can find the transaction using your original `ref`:

### IdeaBiz Format

```
GET https://ideabiz.lk/apicall/msisdndetect/v1/lookup?ref={your_campaign_ref}
Authorization: Bearer {access_token}
```

### TMF681 Format

```
GET https://ideabiz.lk/apicall/msisdndetect-tmf/v1/lookup?ref={your_campaign_ref}
Authorization: Bearer {access_token}
```

### Response (IdeaBiz Format)

```json
{
  "status": "SUCCESS",
  "statusCode": "S1000",
  "vendor_ref": "your-campaign-ref-123",
  "count": 1,
  "transactions": [
    {
      "session_id": "19239b60-11e6-4118-a6fa-aa05e212ddf6",
      "unique_ref": "HE-072b413b-90079",
      "status": "DETECTED",
      "flow_step": "REDIRECTED",
      "created_at": "2026-06-09 18:18:10"
    }
  ]
}
```

> Results are scoped to your application. You can only see your own transactions.

---

## Onboarding Checklist

- [ ] Receive `client_id` (UUID) from IdeaBiz team
- [ ] Subscribe to **MSISDN Detect** API on IdeaBiz Store
- [ ] (Optional) Subscribe to **MSISDN Detect TMF** API for TMF681 format
- [ ] Provide your callback URL(s) to IdeaBiz team
- [ ] Provide your referral domain(s)
- [ ] (Optional) Provide your server notification URL for async callbacks
- [ ] Choose number format: ATEL (recommended), ETEL, STEL, or PLAIN
- [ ] Choose return policy: ALL (default), PRECHECK_OK, or SUBSCRIBED_ONLY
- [ ] Choose additional response: NONE (default), PRECHECK_STATUS, SUBSCRIPTION_STATUS, or both
- [ ] Implement browser redirect on your landing page
- [ ] Implement callback handler to capture `unique_ref` and `status`
- [ ] (Optional) Implement notification endpoint (POST, respond within 2s)
- [ ] Implement backend API call to retrieve MSISDN
- [ ] Handle both DETECTED and NOT_DETECTED cases gracefully

---

## Troubleshooting

### Common Issues

| Symptom | Cause | Solution |
|---------|-------|----------|
| Always `NOT_DETECTED` | User on WiFi, not mobile data | MSISDN detection requires mobile data (3G/4G/5G). WiFi users cannot be detected via HE. |
| Always `NOT_DETECTED` on mobile | Browser upgrading HTTP to HTTPS (HSTS) | Clear browser HSTS cache, or use `protocol=http` param if enabled. |
| `403 invalid client_id` | Wrong or unregistered client_id | Verify your client_id UUID with the IdeaBiz team. |
| Callback never fires | User closed browser mid-flow | Use the `/lookup` API with your `ref` to find the transaction. Configure server notification as backup. |
| `400 Invalid Request` error page | User reloaded a step or used back button | Each session URL is single-use. Start a new detection flow. |
| `E4040 transaction not found` | Wrong `unique_ref` or using another app's ref | Ensure you're using the exact `unique_ref` from the callback, and your Bearer token matches the same API Manager subscription. |
| Detection works but no MSISDN in API | `NOT_DETECTED` status in callback | Check the `status` parameter first. Only call the API if `status=DETECTED`. |
| Slow detection (> 2 seconds) | Decrypt service latency | Normal is < 1s. If consistently slow, contact IdeaBiz team. |

### Detection Modes

Your app may be configured with browser-based detection as a fallback. If so:

- **First visit on mobile data** — MSISDN detected via HE, saved to browser cookie
- **Subsequent visits on WiFi** — MSISDN detected via saved cookie (if configured)
- **New/incognito browser** — Cannot detect on WiFi (no saved cookie)

### Debug Tips

1. Use the PT Tool at `https://he-consent-ptapp.pages.dev` to test the full flow interactively
2. Check your browser's Network tab — all redirects should be 302 with `Cache-Control: no-store`
3. If the authorize redirect shows `Location: http://msisdndetect...`, that's correct (HTTP is needed for enrichment)
4. The `unique_ref` format is `HE-{uuid}` (e.g. `HE-e1f6df42-a686-4dea-89f4-d32be009f3fb`)

---

## Support

For onboarding, configuration, or troubleshooting, contact the IdeaBiz team.
