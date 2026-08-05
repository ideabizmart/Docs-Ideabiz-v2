# Ideabiz MCP — User Guide

Connect your AI coding assistant to Ideabiz APIs via MCP. Send SMS, check balances, charge subscribers, query locations, and manage PIN-based flows.

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

## Available Tools (14)

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

## Tips

- The AI manages token renewal automatically when it expires
- Provide `client_key`/`client_secret` once per session — AI remembers them
- PIN flows are always 2-step: initiate → user gets PIN → submit PIN
- Use `include_raw_response: true` on any tool to debug API responses
- Balance check works for both prepaid and postpaid numbers
