# Ideabiz MCP — User Guide

Connect your AI coding assistant to Ideabiz APIs via MCP. Send SMS, check balances, charge subscribers, query locations, detect subscriber MSISDNs via header enrichment, and manage PIN-based flows.

**MCP URL:** `https://mcp.ideamart.io/ideabiz/mcp`

---

## Prerequisites

1. **MCP Token** — Get one at https://mcp.ideamart.io/auth/login
2. **Ideabiz Client Key & Secret** — OAuth2 credentials from the Ideabiz developer portal
3. **Sender ID / Short Code** — For SMS tools (e.g. `87798`)

## Setup

```json
{
  "mcpServers": {
    "ideabiz": {
      "url": "https://mcp.ideamart.io/ideabiz/mcp",
      "headers": {
        "Authorization": "Bearer mcp_your_token_here"
      }
    }
  }
}
```

---

## Available Tools (18)

| Tool | Purpose |
|------|---------|
| `ideabiz_token_renew` | Get OAuth2 access token |
| `ideabiz_balance_check` | Check subscriber balance (prepaid/postpaid) |
| `ideabiz_sms_send` | Send SMS via short code |
| `ideabiz_lbs_query` | Query subscriber location (GPS) |
| `ideabiz_carrier_billing_charge` | Direct carrier billing charge |
| `ideabiz_pin_subscription` | Initiate PIN-based subscription |
| `ideabiz_pin_payment` | Initiate PIN-based payment |
| `ideabiz_pin_verify` | Initiate PIN-based identity verification |
| `ideabiz_pin_submit` | Submit PIN to confirm payment/verify/subscription |
| `ideabiz_subscription_subscribe` | Subscribe user to service |
| `ideabiz_subscription_unsubscribe` | Unsubscribe user |
| `ideabiz_subscription_state_change` | Change subscription state |
| `ideabiz_subscription_status_check` | Check subscription status |
| `ideabiz_subscription_history_query` | Query subscription history |
| `ideabiz_msisdn_authorize_url` | Build the MSISDN-detect web-redirect URL (HE step 1) |
| `ideabiz_msisdn_status` | Retrieve detected MSISDN by `unique_ref` (HE step 2) |
| `ideabiz_msisdn_lookup` | Recover MSISDN-detect transactions by your campaign ref |
| `ideabiz_docs` | Get Ideabiz API documentation by topic |

---

## Workflow: Token → Action → Result

All Ideabiz tools require an `access_token`. Get it first:

```
Step 1: ideabiz_token_renew → { client_key, client_secret }
        ↓ returns access_token (valid ~1 hour)
Step 2: Use access_token in any other tool
```

---

## Common Workflows

### Check Account Balance

```
You: "Check balance for 94777330710"

AI calls:
1. ideabiz_token_renew → { client_key, client_secret }
2. ideabiz_balance_check → { access_token, msisdn: "94777330710" }

Response:
  account_type: "POSTPAID"
  account_status: "ACTIVE"
  balance: 1158.02
  currency: "LKR"
  credit_limit: 1158.02
```

### Send an SMS

```
You: "Send 'Your OTP is 1234' to 94774585551 from short code 87798"

AI calls:
1. ideabiz_token_renew → { client_key, client_secret }
2. ideabiz_sms_send → {
     access_token,
     to_msisdn: "tel:+94774585551",
     message: "Your OTP is 1234",
     sender_id: "87798"
   }

Response:
  server_reference_code: "OB-390-b0f280f1..."
  delivery_status: "MessageWaiting"
```

### Query Location (LBS)

```
You: "Where is subscriber 94774585551?"

AI calls:
1. ideabiz_token_renew → { client_key, client_secret }
2. ideabiz_lbs_query → { access_token, msisdn: "94774585551" }

Response:
  location_status: "Retrieved"
  latitude: 6.918437
  longitude: 79.863326
  accuracy: "10"
  timestamp: "2026-08-03T15:37:25"
```

### PIN-Based Payment (2-step flow)

```
You: "Charge 100 LKR to 94774585551 using PIN confirmation"

AI calls:
1. ideabiz_token_renew → { client_key, client_secret }
2. ideabiz_pin_payment → {
     access_token,
     msisdn: "tel:+94774585551",
     amount: 100,
     description: "Premium content access",
     txn_ref: "TXN-001"
   }

Response:
  status: "PENDING_AUTH"
  server_ref: "3910fbc27dfb4092..."
  next_step: "User will receive PIN..."

(User receives PIN via SMS: e.g. "342771")

3. ideabiz_pin_submit → {
     access_token,
     server_ref: "3910fbc27dfb4092...",
     pin: "342771",
     type: "payment"
   }

Response:
  status: "SUCCESS"
  confirmed: true
```

### PIN-Based Subscription

```
You: "Subscribe 94774585551 with PIN verification"

AI calls:
1. ideabiz_token_renew → { client_key, client_secret }
2. ideabiz_pin_subscription → { access_token, msisdn: "94774585551", method: "WEB" }
   → status: "PENDING_AUTH", server_ref: "e043..."
3. (User receives PIN)
4. ideabiz_pin_submit → { access_token, server_ref: "e043...", pin: "XXXX", type: "verify" }
```

### PIN-Based Identity Verification

```
You: "Verify identity of 94774585551"

AI calls:
1. ideabiz_token_renew → { client_key, client_secret }
2. ideabiz_pin_verify → { access_token, msisdn: "94774585551", method: "ANC" }
   → status: "PENDING_AUTH", server_ref: "a71f..."
3. (User receives PIN)
4. ideabiz_pin_submit → { access_token, server_ref: "a71f...", pin: "XXXX", type: "verify" }
```

### Carrier Billing (Direct Charge)

```
You: "Charge 50 LKR to 94774585551 for news service"

AI calls:
1. ideabiz_token_renew → { client_key, client_secret }
2. ideabiz_carrier_billing_charge → {
     access_token,
     msisdn: "+94774585551",
     amount: 50,
     currency: "LKR",
     description: "News subscription",
     reference_code: "REF-001",
     merchant_name: "MyApp",
     service_id: "news-svc"
   }
```

### MSISDN Detection (Header Enrichment)

Detect a subscriber's MSISDN automatically when they browse on mobile data — no PIN, no
OTP. Two steps: (1) redirect the user's **browser** through the detect flow, (2) look up
the detected number from your **backend**. Detection only works on mobile data
(3G/4G/5G); WiFi users return `NOT_DETECTED`.

```
You: "Start MSISDN detection for client a1b2c3d4-... with campaign ref SIGNUP-42"

Step 1 — build the redirect URL (no access token needed):

AI calls:
  ideabiz_msisdn_authorize_url → {
    client_id: "a1b2c3d4-...",
    ref: "SIGNUP-42",
    custom_params: { region: "LK" }   // optional, up to 5; emitted as p_region=LK
  }

Response:
  authorize_url: "https://consent.ideamart.io/web/v1/authorize?client_id=...&ref=SIGNUP-42&p_region=LK"
  → Redirect the user's BROWSER to this URL.

After detection the browser returns to your callback:
  https://yoursite.com/callback?ref=SIGNUP-42&unique_ref=HE-xxxx&status=DETECTED

Step 2 — retrieve the MSISDN from your backend (needs access_token):

AI calls:
1. ideabiz_token_renew → { client_key, client_secret }
2. ideabiz_msisdn_status → {
     access_token,
     ref: "HE-xxxx",      // the unique_ref from the callback
     format: "ideabiz"    // or "tmf" for TMF681 shape
   }

Response (detected):
  detected: true
  status_code: "S1000"
  msisdn_token: "IB-A-000A-6a537144-..."
  msisdn_mask: "9477*****67"
  operator: "Dialog"
  detection_method: "HE"
```

**Recover a dropped flow** — if the user closed the browser before the callback fired,
find the transaction by your own campaign `ref`:

```
ideabiz_msisdn_lookup → { access_token, ref: "SIGNUP-42" }
  → { count, transactions: [{ unique_ref, status, flow_step, created_at }, ...] }
Then call ideabiz_msisdn_status with a returned unique_ref.
```

| Point | Detail |
|-------|--------|
| Step 1 auth | `client_id` only — NO access token (it's a browser redirect) |
| Step 2 auth | OAuth2 `access_token` (Bearer), same as every other backend tool |
| `format` | `ideabiz` (default) or `tmf` (TMF681 IndividualIdentification) |
| `unique_ref` | One-time per detection, format `HE-{uuid}` |
| Detection method | `HE` (header enrichment), `AAA` (IP resolver), `BROWSER` (cookie recall) |
| Only on mobile data | WiFi / HTTPS-forced browsers return `NOT_DETECTED` |

---

## API Endpoints (Internal Reference)

| API | Method | Endpoint |
|-----|--------|----------|
| Token Renew | POST | `/apicall/token` |
| Balance Check | GET | `/apicall/balancecheck/v4.2/{msisdn}/transactions/amount/balance` |
| SMS Send | POST | `/apicall/smsmessaging/v3/outbound/{sender}/requests` |
| LBS Query | GET | `/apicall/location/v1/queries/location?address={msisdn}&requestedAccuracy={m}` |
| Carrier Billing | POST | `/apicall/carrier-billing/v1/payments` |
| PIN Subscribe | POST | `/apicall/pin/subscription/v1/subscribe` |
| PIN Payment Charge | POST | `/apicall/pin/payment/v1/charge` |
| PIN Payment Submit | POST | `/apicall/pin/payment/v1/submitPin` |
| PIN Verify Initiate | POST | `/apicall/pin/verify/v1/verify` |
| PIN Verify Submit | POST | `/apicall/pin/verify/v1/submitPin` |
| MSISDN Detect — Authorize | GET | `https://consent.ideamart.io/web/v1/authorize?client_id={id}&ref={ref}` (browser) |
| MSISDN Detect — Status | GET | `/apicall/msisdndetect/v1/status?ref={unique_ref}` (`-tmf` variant for TMF681) |
| MSISDN Detect — Lookup | GET | `/apicall/msisdndetect/v1/lookup?ref={campaign_ref}` (`-tmf` variant for TMF681) |

> The authorize step uses the **`consent.ideamart.io`** host (browser redirect), not the
> `ideabiz.lk` API base. The status/lookup calls use the standard `ideabiz.lk` base with a
> Bearer token.

---

## MSISDN Format

Ideabiz accepts these formats:
- `tel:+94771234567` (recommended for PIN payment)
- `94771234567` (recommended for balance, LBS, PIN subscription)
- `+94771234567`
- `0771234567`

## Environments

| Value | Description |
|-------|-------------|
| `prod` | Production (default) — `ideabiz.lk` |
| `uat` | UAT testing |
| `dev` | Development |

## Error Handling

| Error | Meaning | Fix |
|-------|---------|-----|
| Token expired | access_token invalid | Call `ideabiz_token_renew` again |
| API blocked (503) | API temporarily suspended | Contact IdeaBiz support |
| Payment denied (403) | Business rules blocked charge | Check subscriber eligibility |
| Wrong Pin (500) | Incorrect PIN submitted | Ask user for correct PIN |
| 404 Not Found | API not subscribed for your app | Subscribe to API in IdeaBiz portal |
| Unauthorized (401) | MCP token invalid | Get new token at `/auth/login` |
| MSISDN not detected (S1001) | User on WiFi / no header | Expected on WiFi — only mobile data (3G/4G/5G) is detectable |
| MSISDN E4040 | `unique_ref` not found | Use the exact `unique_ref` from the callback; token must match the same app |

### MSISDN Detect — `ideabiz_msisdn_status` statusCode

| Code | Meaning |
|------|---------|
| S1000 | MSISDN detected |
| S1001 | MSISDN not detected (WiFi, no header, HTTPS) |
| E4001 | Missing `ref` |
| E4010 / E4011 | Missing / invalid token |
| E4030 | App not authorized for this ref |
| E4040 | Transaction not found for ref |
| E5002 | Number format conversion failed |

## Tips

- The AI manages token renewal automatically when it expires
- Provide `client_key`/`client_secret` once per session — AI remembers them
- PIN flows are always 2-step: initiate → user gets PIN → submit PIN
- MSISDN detection is also 2-step: `ideabiz_msisdn_authorize_url` (browser redirect, no token) → `ideabiz_msisdn_status` (backend, token)
- `ideabiz_msisdn_authorize_url` makes no API call — it only builds the redirect URL
- Use `include_raw_response: true` on any tool to debug API responses
- Use `ideabiz_docs` with topic `msisdn` for the full header-enrichment reference
- Balance check works for both prepaid and postpaid numbers

---

## Webhook Testing with Echo2

Echo2 (https://echo.idmrt.dev/) captures inbound notifications/callbacks without running your own server. Useful for debugging PIN callbacks, delivery reports, and carrier billing notifications.

### Capture SMS Delivery Reports:

1. **Create Echo2 session** — Go to https://echo.idmrt.dev/ → Create Session → get unique callback URL
2. **Use as notifyURL** — When sending SMS, include the Echo2 URL in `receiptRequest.notifyURL`
3. **View** — When delivery status changes, Echo2 shows the DLR payload in real-time

### Capture PIN Payment Callbacks:

1. Set Echo2 URL as `callbackURL` in the PIN payment charge request
2. After user confirms PIN, Echo2 captures the payment confirmation
3. Inspect the payload to verify transaction outcome

### Capture Carrier Billing Notifications:

1. Use Echo2 URL as `notificationUrl` in carrier billing webhook field
2. After charge processes, Echo2 captures the notification with transaction status

### Echo2 Notes:
- Region-restricted for session creation — use session code from colleague if blocked
- Use for testing/debugging only — switch to your real server for production
- Works for all Ideabiz notification types (DLR, PIN callbacks, billing notifications)
- Each session has a unique URL — don't share publicly

