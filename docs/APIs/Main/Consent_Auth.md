# Consent Auth API — Integration Documentation

## Overview

The Consent Auth API enables vendors to identify mobile subscribers via Header Enrichment (HE) when users browse through the mobile data network. The integration uses a two-step approach:

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
2. Subscribe to "Consent Auth" API on IdeaBiz Store
3. Redirect user browser → consent.ideamart.io/web/v1/authorize?client_id=X&ref=Y
4. Receive callback with status and unique_ref
5. Call ideabiz.lk API with unique_ref → get MSISDN
```

---

## Step 1: Web Redirect (consent.ideamart.io)

Redirect the user's browser to start MSISDN detection. No server-side auth needed — your app is identified by `client_id`.

### Authorize Endpoint

```
GET https://consent.ideamart.io/web/v1/authorize?client_id={client_id}&ref={campaign_ref}
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| `client_id` | Yes | Your UUID (provided during onboarding) |
| `ref` | Yes | Your campaign/session reference. Returned unchanged in callback. |

### What Happens

The user's browser is automatically redirected through the detection chain (invisible to the user, takes < 1 second):

```
consent.ideamart.io/web/v1/authorize → (WAF detection) → callback to your site
```

### Callback to Your Site

After detection completes, the user is redirected to your configured callback URL:

```
https://yoursite.com/callback?ref={campaign_ref}&unique_ref={unique_ref}&status={status}
```

| Parameter | Description |
|-----------|-------------|
| `ref` | Your original campaign reference (unchanged) |
| `unique_ref` | Transaction reference for API lookup (e.g. `HE-1a3d1f87-36511`) |
| `status` | `DETECTED` or `NOT_DETECTED` |

> The redirect always happens regardless of detection outcome. Check the `status` parameter.

---

## Step 2: Backend API Lookup (ideabiz.lk)

From your backend server, call the API with the `unique_ref` to retrieve the MSISDN.

### Authentication

Obtain an OAuth2 Bearer token:

```bash
curl -X POST "https://ideabiz.lk/token" \
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

**API Name:** Consent Auth  
**Version:** v1

```
GET https://ideabiz.lk/apicall/consent/v1/status?ref={unique_ref}
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
    "rvalue": "IB-A-000A-697-648-774-1704"
  }
}
```

#### Success — NOT DETECTED

```json
{
  "status": "SUCCESS",
  "statusCode": "S1001",
  "message": "MSISDN not detected"
}
```

#### Errors

| HTTP | Code | Description |
|------|------|-------------|
| 400 | E4001 | Missing `ref` query parameter |
| 401 | E4010 | Missing or invalid Bearer token |
| 404 | E4040 | Transaction not found for given ref |
| 500 | E5002 | Number format conversion failed |

---

### Lookup by Vendor Reference

Use this to find our `unique_ref` using your own campaign reference — useful for transactions that terminated mid-flow (user dropped off before callback).

```
GET https://ideabiz.lk/apicall/consent/v1/lookup?ref={your_campaign_ref}
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

**API Name:** Consent Auth TMF  
**Version:** v1

```
GET https://ideabiz.lk/apicall/consent-tmf/v1/status?ref={unique_ref}
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
        "phoneNumberFormat": "ATEL"
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
| 400 | ERR-0001 | Missing required parameter |
| 401 | — | Unauthorized |
| 404 | ERR-0004 | Resource not found |
| 500 | ERR-0005 | Internal error |

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
    curl_setopt($ch, CURLOPT_URL, "https://ideabiz.lk/apicall/consent/v1/status?ref=" . urlencode($uniqueRef));
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
      `https://ideabiz.lk/apicall/consent/v1/status?ref=${unique_ref}`,
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
        f"https://ideabiz.lk/apicall/consent/v1/status?ref={unique_ref}",
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
    .uri(URI.create("https://ideabiz.lk/apicall/consent/v1/status?ref=" + uniqueRef))
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
  "timestamp": "2026-06-09T12:30:45Z"
}
```

| Field | Description |
|-------|-------------|
| `unique_ref` | Our transaction reference (use for `/status` API call) |
| `campaign_ref` | Your original `ref` parameter from authorize |
| `status` | `DETECTED` or `NOT_DETECTED` |
| `session_id` | Internal session ID |
| `timestamp` | UTC timestamp of redirect |

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
GET https://ideabiz.lk/apicall/consent/v1/lookup?ref={your_campaign_ref}
Authorization: Bearer {access_token}
```

### TMF681 Format

```
GET https://ideabiz.lk/apicall/consent-tmf/v1/lookup?ref={your_campaign_ref}
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
- [ ] Subscribe to **Consent Auth** API on IdeaBiz Store
- [ ] (Optional) Subscribe to **Consent Auth TMF** API for TMF681 format
- [ ] Provide your callback URL(s) to IdeaBiz team
- [ ] Provide your referral domain(s)
- [ ] (Optional) Provide your server notification URL for async callbacks
- [ ] Choose number format: ATEL (recommended), ETEL, STEL, or PLAIN
- [ ] Implement browser redirect on your landing page
- [ ] Implement callback handler to capture `unique_ref` and `status`
- [ ] (Optional) Implement notification endpoint (POST, respond within 2s)
- [ ] Implement backend API call to retrieve MSISDN
- [ ] Handle both DETECTED and NOT_DETECTED cases gracefully

---

## Support

For onboarding, configuration, or troubleshooting, contact the IdeaBiz team.
