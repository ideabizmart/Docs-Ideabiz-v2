# Consent v2 — Complete Integration Guide


## Contents

- [Overview](#overview)
- [How it works](#how-it-works)
- [Entry URLs](#entry-url)
- [Request parameters](#request-parameters)
- [OAuth2 integration](#oauth2)
- [Return to your site](#return-to-your-site)
- [Notifications (Native and TMF)](#notifications)
- [Lookup API](#lookup-api)
- [Code examples](#code-examples)
- [Error codes](#error-codes)
- [Troubleshooting](#troubleshooting)
- [Onboarding checklist](#onboarding-checklist)
  - [Customization overview](#customization)
  - [Custom fields](#custom-fields)
  - [Multiple forms and show-rules](#forms-show-rules)
  - [Variants and pre-filled links](#variants-prefilled-links)
  - [Branding and theming](#branding-theming)
  - [Languages](#languages)
  - [Subscription and payment options](#subscription-payment)
  - [Add-ons and packs](#add-ons-packs)
  - [Sign-in providers](#sign-in-providers)
  - [Shared core app and multiple journeys](#shared-core-journeys)

---

<a id="overview"></a>

## Overview

Consent v2 is a configurable journey service for your app. You send the customer to one URL. We run the steps your app is set up for, which can include number detection, OTP verification, forms, consent, a subscription or a one-time charge, and payment. Then we return the customer to your site with a reference.

This section is for developers, merchants and vendors who integrate with the service. It covers how to call it, what comes back, and what you can customize.

<a id="overview-what-a-journey-can-include"></a>
### What a journey can include

| Step | What happens |
|------|--------------|
| Number detection | We detect the customer's mobile number on mobile data. If that is not possible, the customer types it. |
| OTP | The customer confirms the number with a one-time PIN. |
| Forms and custom fields | You collect the details you need: text, choices, locations, files, scanned codes and more. |
| Sign-in (optional) | The customer can identify themselves with Google, Microsoft or a third-party provider. |
| Consent | The customer reads and accepts the terms you configured. |
| Subscription or charge | A new subscription, or a one-time payment. |
| Payment | Operator billing or another payment method, depending on your setup. |
| Outcome | The customer sees a receipt and returns to your site. |

Steps that your app does not need are skipped. You choose the combination during onboarding.

<a id="overview-the-integration-in-three-steps"></a>
### The integration in three steps

1. **Send** the customer to your entry URL (or start an OAuth2 authorization).
2. **Receive** the redirect back to your site, and the server-to-server notification we POST to your endpoint.
3. **Confirm** the result from your backend with the lookup API before you deliver value.

<a id="overview-what-you-need-before-you-start"></a>
### What you need before you start

| Item | Where it comes from |
|------|---------------------|
| A registered application and `client_id` | Issued during onboarding. |
| A **slug** | Agreed during onboarding. It becomes part of your public URL, for example `summer-promo`. |
| A callback (return) URL | You register it during onboarding. We only redirect to registered URLs. |
| A notification endpoint | Optional but recommended. You choose the format and the authentication. |
| An application on the IdeaBiz API Gateway | Used to get the Bearer token for the lookup API. |

The full list is in the [onboarding checklist](#onboarding-checklist).

---

<a id="how-it-works"></a>

## How it works

You send the customer to one URL. We run whatever journey your app is configured for, and return the customer to you with a reference.

```
your site
   │  link
   ▼
https://consent.ideamart.io/v2/offer/{slug}
   │
   │  we handle: MSISDN detection, OTP, consent,
   │  custom fields, subscription or charge, payment
   │
   ▼
your callback?ref=…&ext_ref=…&status=…
   │
   ├── we POST a notification to your endpoint   ← push
   └── you call the lookup API with either ref   ← pull (authoritative)
```

Three things to hold onto:

1. **The redirect parameters are a hint.** They tell you the journey finished and how, but they arrive through the browser.
2. **The notification is a convenience.** Delivery is at-least-once, so make your endpoint idempotent.
3. **The lookup API is the truth.** Confirm server-side before you deliver value.

<a id="how-it-works-the-journey-is-a-list-of-steps"></a>
### The journey is a list of steps

When a customer starts, we build the list of steps for that journey from your app's configuration. The list is fixed for that customer's session, so a configuration change you make later never disrupts a journey already in progress.

| Step | Included when |
|------|---------------|
| Security check | A fraud-prevention vendor is assigned to your app. |
| Number detection | Your app needs the customer's number and you did not supply a verified one. |
| Manual number entry | Detection failed, or your app always asks for the number plus an OTP. |
| OTP | OTP is enabled for your app. |
| Phone handoff (QR) | Your app requires mobile data and the customer opened the link on a desktop. |
| Forms and custom fields | Your app collects fields. |
| Sign-in | Your app offers at least one sign-in provider. |
| Consent | Your app has consent configured. |
| Subscription or charge | Your app is configured for one. |
| Payment | The action needs a payment method beyond operator billing. |
| Outcome | Always. |

Steps cannot be skipped or replayed by editing the page. If a customer reaches a step out of order, the journey ends safely (see [Error codes](#error-codes), `E1005` and `E1006`).

<a id="how-it-works-features-are-switched-on-per-app"></a>
### Features are switched on per app

Each capability is a setting on your app, and everything that could expose a journey to a customer starts switched off. Your journey is only reachable once it has been configured, checked and activated. Until then, your entry URL returns `E1002`.

<a id="how-it-works-setting-precedence"></a>
### Setting precedence

Where a setting can come from more than one place, the most specific one wins:

```
URL parameter (only where your app allows it)
   ↓ falls back to
your app's own setting
   ↓ falls back to
the platform default
```

This is why you can pass `theme_id` or `banner_id` in a link, yet cannot select a banner that has not been approved for your app.

<a id="how-it-works-journeys-slugs-and-your-core-app"></a>
### Journeys, slugs and your core app

A **journey** has its own slug, its own look (theme and banner), and its own consent configuration. One vendor account can run several journeys that share the same underlying registration. See [Shared core app and multiple journeys](#shared-core-journeys).

<a id="how-it-works-languages"></a>
### Languages

The page language is chosen from, in order of priority: the customer's choice on the page's language switcher, then your `lang` parameter or the customer's remembered preference, then your app default, then English. See [Languages](#languages).

---

<a id="entry-url"></a>

## Entry URLs

There are five ways in. Pick whichever suits your stack. The journey is identical once started.

| # | Entry | When to use |
|---|-------|-------------|
| 1 | `GET /v2/offer/{slug}` | Simplest. One link, SEO-friendly. |
| 2 | `GET /v2/offer/{slug}/{variant}` | One link **per package**, with the pack or amount already fixed. |
| 3 | `GET /v2/authorize?...` | You already have an OAuth2 / OIDC client. |
| 4 | `GET /web/v1/authorize?client_id=…` | You already integrate with MSISDN Detect. |
| 5 | `GET /v2/offer/{slug}/{link_code}` (short form `/s/{link_code}`) | A **pre-filled link** you create for one customer: their details already filled in, optionally a subscription, add-ons or a pack code preselected. |

All paths use the `/v2/` version prefix on `https://consent.ideamart.io`, so a future breaking change can ship as `/v3/` without moving your existing links.

<a id="entry-url-1-offer-url"></a>
### 1. Offer URL

```
GET https://consent.ideamart.io/v2/offer/{slug}
```

`{slug}` is assigned during onboarding, for example `summer-promo`. The page carries your configured title, description and sharing image, so it can be shared or indexed. No server-side authentication is needed. Your app is identified by the slug.

<a id="entry-url-2-variant-sub-urls"></a>
### 2. Variant sub-URLs

A variant is a second path segment that arrives with choices already made:

```
https://consent.ideamart.io/v2/offer/summer-promo                    base journey
https://consent.ideamart.io/v2/offer/summer-promo/sports-pack        SPORTS preselected
https://consent.ideamart.io/v2/offer/summer-promo/premium-bundle     BASE + SPORTS + MUSIC
https://consent.ideamart.io/v2/offer/summer-promo/topup-100          one-time charge, LKR 100
```

A variant can preset the subscription packs, the charge amount, theme, banner, language and custom-field defaults. It can also **lock** any of them so the customer cannot change what your link chose.

Use a variant rather than `?pack=` or `?amount=` whenever the value is fixed for a campaign:

| | Query string | Variant path |
|---|--------------|--------------|
| Customer edits the URL | Can change the pack or amount | Cannot. The preset is held on our side. |
| Sharing and SEO | Parameters are treated inconsistently | Clean, indexable, own metadata |
| Reporting | Needs parsing to attribute | Attributed directly to the variant |

Query parameters remain the right choice for genuinely dynamic values such as `ref`, `lang`, and `amount` when your app permits a vendor-supplied amount.

Ask for variants to be created during onboarding. They are configured, not passed. See [Variants and pre-filled links](#variants-prefilled-links).

<a id="entry-url-3-oauth2"></a>
### 3. OAuth2

See [OAuth2 integration](#oauth2).

<a id="entry-url-4-existing-msisdn-detect-link"></a>
### 4. Existing MSISDN Detect link

If you already integrate MSISDN Detect and Consent v2 is switched on for your app, your existing `/web/v1/authorize?client_id=…` link keeps working. It redirects into the Consent v2 journey automatically. You gain the new steps without changing anything on your side.

If your app runs **more than one** journey (several slugs sharing one `client_id`), this automatic handoff goes to whichever journey was set up **first**. Any additional journey is reached only by its own `/v2/offer/{slug}` link (entry method 1). Ask during onboarding which journey should own the automatic handoff if that matters to you. See [Shared core app and multiple journeys](#shared-core-journeys).

<a id="entry-url-5-pre-filled-links"></a>
### 5. Pre-filled links

Create one link per customer from your own system. A link carries the customer's details, whether they may edit them, an expiry, how many times it may be submitted, and optionally what to offer. The URL itself holds only an opaque code. See [Variants and pre-filled links](#variants-prefilled-links) for the request, the response and the 8-digit access code.

---

<a id="request-parameters"></a>

## Request parameters

Add these to your [offer URL](#entry-url). Anything not listed here is passed through (see the last row).

| Parameter | Required | Description |
|-----------|----------|-------------|
| `ref` | Recommended | Your own campaign or session reference. Returned unchanged, and queryable later. |
| `lang` | No | Language tag, for example `si`. **Overrides the app default.** Must be a language enabled for your app. Unknown values are ignored. |
| `theme_id` | No | Selects one of your assigned themes. Ignored unless the theme allows URL override. |
| `banner_id` | No | Selects one of your **approved** banners. Ignored if unknown or unapproved. |
| `amount` | Conditional | One-time charge amount. Only honoured when your app allows a vendor-supplied amount, and always limited to the minimum and maximum configured for your app. |
| `pack` | No | Comma-separated subscription pack codes to pre-select, for example `pack=SPORTS,MUSIC`. Only packs marked as selectable by URL are applied. |
| `sso` | No | Send the customer straight to a sign-in provider: `google`, `microsoft`, or a third-party provider's code such as `x12`. Ignored unless that provider is approved for your app. The customer can still continue without signing in. See [Sign-in providers](#sign-in-providers). |
| `security_ref` | Conditional | Reference from your anti-fraud vendor, when your app is configured to require one. |
| `security_vendor` | Conditional | Vendor name, for example `evina`. |
| *(any other)* | No | Extra parameters are echoed back to you (see below). |

Example:

```
https://consent.ideamart.io/v2/offer/summer-promo
  ?ref=camp-778
  &lang=si
  &pack=SPORTS
  &amount=50.00
```

<a id="request-parameters-extra-parameters"></a>
### Extra parameters

Any other query parameters on your entry URL, such as `utm_source`, are carried through the journey. You get them back on the return redirect and in the `passthrough` object of the [native notification](#notifications).

<a id="request-parameters-language-behaviour"></a>
### Language behaviour

Your `lang` tag overrides the app default. It does **not** override a choice the customer makes on the page. If they use the language switcher, their choice wins for the rest of the journey.

When a customer explicitly picks a language, we remember it against their number for future journeys. Whether your `lang` tag or that remembered preference takes precedence is a per-app setting, because the right answer differs between campaign traffic and service portals. Ask during onboarding which you want. Your `lang` tag never overwrites a customer's remembered preference.

More detail is in [Languages](#languages).

<a id="request-parameters-parameters-are-checked-on-our-side"></a>
### Parameters are checked on our side

Values you pass are only ever applied within what your app permits. An out-of-range `amount`, a pack that is not selectable by URL, an unapproved banner or an unassigned language does not break the journey. It is ignored, or limited, and the next source applies.

---

<a id="oauth2"></a>

## OAuth2 integration

`/v2/authorize` is a standard authorization-code endpoint. You can use an off-the-shelf OAuth2 or OIDC client library instead of hand-rolling the redirect handling.

<a id="oauth2-discovery"></a>
### Discovery

```
GET https://consent.ideamart.io/v2/.well-known/openid-configuration
```

<a id="oauth2-step-1-authorize"></a>
### Step 1: authorize

```
GET https://consent.ideamart.io/v2/authorize
      ?response_type=code
      &client_id={your client_id}
      &redirect_uri=https://vendor.com/callback
      &scope=msisdn%20consent%20subscription
      &state={your CSRF token}
      &nonce={your replay guard}
      &code_challenge={BASE64URL(SHA256(verifier))}
      &code_challenge_method=S256
      &ref={your reference}
      &variant={variant_slug}
```

| Parameter | Required | Notes |
|-----------|----------|-------|
| `response_type` | Yes | `code` only. |
| `client_id` | Yes | The same `client_id` as MSISDN Detect. |
| `redirect_uri` | Yes | Must **exactly** match one of your registered redirect URLs. |
| `scope` | Yes | Space separated, from `msisdn consent subscription charge fields`. |
| `state` | Recommended | Returned verbatim. We never interpret it. |
| `nonce` | Recommended | Echoed into `id_token`. |
| `code_challenge` | Usually | **PKCE is required by default.** Send `S256`. |
| `code_challenge_method` | Usually | `S256`. `plain` is rejected unless explicitly enabled for your app. |
| `ref` | No | Your own reference, as on the offer URL. |
| `variant` | No | Apply a variant's presets to an OAuth journey. |

The customer completes the journey, then you receive:

```
https://vendor.com/callback?code={authorization_code}&state={your state}
```

On failure you get standard OAuth2 error parameters instead:

```
https://vendor.com/callback?error=access_denied&error_description=...&state=...
```

<a id="oauth2-step-2-exchange-the-code"></a>
### Step 2: exchange the code

```http
POST https://consent.ideamart.io/v2/token
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
&code={authorization_code}
&redirect_uri=https://vendor.com/callback
&client_id={your client_id}
&code_verifier={your PKCE verifier}
```

```json
{
  "access_token": "eyJ...",
  "token_type": "Bearer",
  "expires_in": 900,
  "id_token": "eyJ...",
  "scope": "msisdn consent subscription",
  "ext_ref": "EXT-3f9a1c4e-7b21-4d0e-9c33-5a1f2b8e4d70"
}
```

Codes are **single use** with a 120-second lifetime. A second presentation is refused and logged.

<a id="oauth2-step-3-read-the-result"></a>
### Step 3: read the result

```http
GET https://consent.ideamart.io/v2/userinfo
Authorization: Bearer {access_token}
```

This returns the same body as the [native notification](#notifications-native-notification-format), so one parser serves the notification, `userinfo` and the [lookup API](#lookup-api).

<a id="oauth2-two-things-to-be-clear-about"></a>
### Two things to be clear about

!!! warning "This is not a login system"
    The access token is scoped to **one journey**, not to a user. It expires in 15 minutes and grants read access to that journey's result only. Do not treat `id_token` as a durable subscriber identity or build a session on it. Use your own session keyed by the MSISDN value we return.

!!! note "PKCE is on by default"
    Your client is typically a browser redirect with no secret to protect. Without PKCE, a stolen authorization code is enough to read the journey. If your library does not support PKCE, ask during onboarding rather than assuming `plain` will be accepted.

<a id="oauth2-oauth2-errors"></a>
### OAuth2 errors

The OAuth2 entry uses standard RFC 6749 and RFC 6750 error identifiers, so a client library recognises them. See [Error codes](#error-codes-oauth2-errors).

---

<a id="return-to-your-site"></a>

## Return to your site

When the journey ends, the customer is redirected to your registered callback URL:

```
{your_callback_url}
  ?ref={your_ref}
  &ext_ref={our_reference}
  &status={COMPLETED|ABANDONED|FAILED|BLOCKED}
  &action_status={CONSENT_RECORDED|SUBSCRIBED|ALREADY_ACTIVE|CHARGED|NONE}
  &code={status_code}
```

| Parameter | Description |
|-----------|-------------|
| `ref` | Your original reference, unchanged. |
| `ext_ref` | Our journey reference, `EXT-{uuid}`. Use this for lookup. |
| `status` | Overall outcome. |
| `action_status` | What actually happened. |
| `code` | Status code. See [Error codes](#error-codes). |

Any extra parameters you sent on the entry URL are echoed back as well.

<a id="return-to-your-site-status-values"></a>
### `status` values

| `status` | Meaning |
|----------|---------|
| `COMPLETED` | Journey finished. Check `action_status` for what was done. |
| `ABANDONED` | The customer left, or the session expired. |
| `FAILED` | An action or payment was attempted and failed. |
| `BLOCKED` | Stopped by security or captcha. |

<a id="return-to-your-site-action_status-values"></a>
### `action_status` values

| Value | Meaning |
|-------|---------|
| `CONSENT_RECORDED` | Consent captured, nothing else required. |
| `SUBSCRIBED` | A new subscription was created. |
| `ALREADY_ACTIVE` | The subscriber was already subscribed. Nothing was billed again. |
| `CHARGED` | A one-time charge succeeded. |
| `NONE` | No action configured, or the journey ended before the action. |

The redirect always happens, whatever the outcome. Check `status` before acting.

<a id="return-to-your-site-what-to-do-for-each-code"></a>
### What to do for each code

| `code` | `status` | `action_status` | Your action |
|--------|----------|-----------------|-------------|
| `S1000` | `COMPLETED` | any | Confirm via lookup, then deliver value. |
| `S1001` | `COMPLETED` | `ALREADY_ACTIVE` | Already a subscriber. Grant access, do not bill again. |
| `S1002` | `ABANDONED` | `NONE` | The customer did not finish. Nothing to grant. |
| `S1003` | `COMPLETED` | any | Completed, but the anti-fraud check could not be confirmed. Apply your own risk rules. |
| `E1004` | `ABANDONED` | `NONE` | Session expired. Offer the journey again. |
| `E1006` | `ABANDONED` | `NONE` | Out-of-order or replayed access, handled silently. |
| `E2001` | `BLOCKED` | `NONE` | Security vendor refused. Do not grant. |
| `E2002` | `BLOCKED` | `NONE` | The security check could not complete and your policy is to block. |
| `E2003` | `BLOCKED` | `NONE` | Captcha refused. |
| `E3004` | `ABANDONED` | `NONE` | OTP attempts exhausted. |
| `E5013` | `FAILED` | `NONE` | Charge failed. |
| `E5021` | `FAILED` | `NONE` | Payment declined. |
| `E5022` | `ABANDONED` | `NONE` | The customer cancelled at the payment provider. |
| `E5023` | `FAILED` | `NONE` | Payment or QR session expired. |

!!! warning
    The redirect is a hint. Confirm with the [lookup API](#lookup-api) before delivering anything of value.

For OAuth2 entries the redirect carries `code` and `state` instead, and failures arrive as standard OAuth2 `error` parameters. See [OAuth2 integration](#oauth2).

---

<a id="notifications"></a>

## Notifications (Native and TMF)

When the journey reaches a terminal state, we POST the collected data to the endpoint(s) configured for your app. Two formats are available, **native** and **TMF**. You can register both if you want to migrate between them.

| Property | Behaviour |
|----------|-----------|
| Method | `POST` (or `PUT` if you prefer). |
| Auth | None, Bearer, Basic, or HMAC signature. Your choice at onboarding. |
| Timeout | 10 s default. |
| Retries | 3 by default, with exponential backoff. |
| Delivery | **At-least-once.** Be idempotent on `ext_ref`. |
| Expected response | Any `2xx`. Anything else is retried. |
| Target URL | Must be HTTPS. |

If delivery fails after all retries, nothing is lost. The journey stays retrievable through the [lookup API](#lookup-api), which is the authoritative interface.

<a id="notifications-hmac-verification"></a>
### HMAC verification

If you choose HMAC, we send a hex-encoded SHA-256 HMAC of the **raw request body** in the header you nominate (default `X-Signature`):

```
X-Signature: 4f8a2c...  = HMAC_SHA256(shared_secret, raw_body)
```

Compute it over the raw bytes before any JSON parsing, and compare using a constant-time comparison. See [Code examples](#code-examples).

<a id="notifications-native-notification-format"></a>
### Native notification format

Flat and explicit. Recommended unless you have a reason to want TMF.

```json
{
  "ext_ref": "EXT-3f9a1c4e-7b21-4d0e-9c33-5a1f2b8e4d70",
  "merchant_ref": "camp-778",
  "core_unique_ref": "HE-cc487c73-e628-40c5-88b5-8ff26e93f840",
  "slug": "summer-promo",
  "status": "COMPLETED",
  "action_status": "SUBSCRIBED",
  "status_code": "S1000",
  "occurred_at": "2026-09-03T14:02:59+05:30",

  "subscriber": {
    "msisdn": "IB-A-000A-6a537144-e3be-4728-a136-b94d11083bf3",
    "msisdn_mask": "9477*****61",
    "msisdn_format": "ATEL",
    "verified": true,
    "verification_method": "OTP",
    "source": "DETECTED",
    "operator": "Dialog"
  },

  "session": {
    "device_type": "MOBILE_DATA",
    "language": "si",
    "language_source": "USER_SELECTED",
    "duration_ms": 48200,
    "security_result": "VALID",
    "security_flag": false,
    "captcha_result": "OK"
  },

  "transaction": {
    "txn_ref": "EXTTXN-9f1c8b02-4a55-4c19-b2f8-7d3e6a1c9b44",
    "type": "SUBSCRIPTION",
    "status": "SUCCESS",
    "amount": 30.00,
    "currency": "LKR",
    "payment_method": "DCB",
    "payment_provider": "DCB",
    "provider_txn_ref": "DCB-556677",
    "subscription_ref": "SUB-88213",
    "packs": [
      { "pack_code": "BASE_MONTHLY", "name": "Premium News Alerts", "price": 30.00, "billing_period": "MONTHLY", "optional": false },
      { "pack_code": "SPORTS", "name": "Weekly sports digest", "price": 0.00, "billing_period": "MONTHLY", "optional": true }
    ]
  },

  "consent": {
    "type": "SUBSCRIPTION",
    "accepted": true,
    "accepted_at": "2026-09-03T14:02:44+05:30",
    "message_shown": "Subscribe to Premium News Alerts for LKR 30.00 / month. You can cancel any time.",
    "parameters": [
      { "key": "service_name", "label": "Service", "value": "Premium News Alerts", "mandatory": true, "accepted": true },
      { "key": "price", "label": "Price", "value": "LKR 30.00 / month", "mandatory": true, "accepted": true },
      { "key": "sports", "label": "Weekly sports digest", "value": "", "mandatory": false, "accepted": false }
    ]
  },

  "customer": {
    "id": 41,
    "name": "Nimal Perera",
    "email": "nimal@example.com",
    "email_verified": true,
    "provider": "google"
  },
  "addons": [
    { "code": "MUSIC", "name": "Music", "kind": "RECURRING", "billing_period": "MONTHLY", "price": 50.0, "currency": "LKR", "ideabiz_service_id": "h1" }
  ],
  "pack_code": "JUNE50",
  "custom_fields": [
    { "key": "nic", "label": "NIC number", "type": "TEXT", "value": "199012345678" },
    { "key": "district", "label": "District", "type": "DROPDOWN", "value": "COLOMBO" },
    { "key": "interests", "label": "Interests", "type": "CHECKBOX", "values": ["SPORTS", "MUSIC"] }
  ],

  "link_events": [
    { "key": "tnc", "url": "https://vendor.com/terms", "clicked_at": "2026-09-03T14:02:31+05:30" }
  ],

  "passthrough": { "utm_source": "facebook", "utm_campaign": "summer" }
}
```

Notes:

- `subscriber.msisdn` is a **tokenised or formatted** value according to the number format chosen for your app, not a raw number. `msisdn_mask` is safe to display.
- Single-value custom fields use `value`. Multi-value fields use `values` as an array.
- `custom_fields` and `link_events` are omitted if you turn them off in your configuration.
- `customer` is present only when the customer **signed in** with Google, Microsoft or a third-party provider. `name` is always filled (the customer is asked for it if the provider did not return one). `email_verified` is `true` only when the provider said so. `provider` is `google`, `microsoft`, or the third-party provider's code. See [Sign-in providers](#sign-in-providers).
- `addons` lists the add-ons the customer ticked, and `pack_code` the pack code they used. **Add-ons are recorded only**: nothing has been subscribed or charged for them. Each carries its own `ideabiz_service_id`. See [Add-ons and packs](#add-ons-packs).
- A **location** field's `value` is a JSON string `{"address","lat","lng"}`. A **barcode** field's `value` is the text inside the code. **Photo, PDF and camera** fields appear as `1 file` or `2 files`. The files themselves are **never** sent in a notification. They stay encrypted on the platform and are available to your app's owners in your account portal.
- `customer`, `addons` and `pack_code` are in the **native** format only. The TMF format does not carry them yet.
- `passthrough` carries any extra query parameters from your entry URL.

<a id="notifications-tmf-notification-format"></a>
### TMF notification format

TMF681-flavoured, consistent with the TMF lookup of MSISDN Detect. Journey data that has no natural TMF home is carried in `characteristic` arrays rather than invented top-level fields.

```json
{
  "id": "EXT-3f9a1c4e-7b21-4d0e-9c33-5a1f2b8e4d70",
  "href": "/v2/api/tmf/status?ref=EXT-3f9a1c4e-7b21-4d0e-9c33-5a1f2b8e4d70",
  "@type": "IndividualIdentification",
  "identificationId": "8a395ff9-c181-4c7a-a786-557ab74dd846",
  "lifecycleStatus": "Completed",
  "validFor": { "startDateTime": "2026-09-03T14:02:11+05:30" },
  "verificationResult": true,
  "externalIdentifier": [
    { "@type": "ExternalIdentifier", "owner": "merchant", "externalIdentifierType": "merchantRef", "id": "camp-778" },
    { "@type": "ExternalIdentifier", "owner": "ideamart", "externalIdentifierType": "coreRef", "id": "HE-cc487c73-e628-40c5-88b5-8ff26e93f840" }
  ],

  "identifiedPerson": {
    "@type": "Individual",
    "contactMedium": [
      {
        "mediumType": "mobile",
        "preferred": true,
        "characteristic": {
          "phoneNumber": "IB-A-000A-6a537144-e3be-4728-a136-b94d11083bf3",
          "phoneNumberMask": "9477*****61",
          "phoneNumberFormat": "ATEL",
          "operator": "Dialog"
        }
      }
    ]
  },

  "characteristic": [
    { "name": "status",           "valueType": "string",  "value": "COMPLETED" },
    { "name": "actionStatus",     "valueType": "string",  "value": "SUBSCRIBED" },
    { "name": "statusCode",       "valueType": "string",  "value": "S1000" },
    { "name": "msisdnVerified",   "valueType": "boolean", "value": true },
    { "name": "verificationMethod","valueType": "string", "value": "OTP" },
    { "name": "deviceType",       "valueType": "string",  "value": "MOBILE_DATA" },
    { "name": "language",         "valueType": "string",  "value": "si" },
    { "name": "securityResult",   "valueType": "string",  "value": "VALID" },
    { "name": "transactionRef",   "valueType": "string",  "value": "EXTTXN-9f1c8b02-4a55-4c19-b2f8-7d3e6a1c9b44" },
    { "name": "transactionType",  "valueType": "string",  "value": "SUBSCRIPTION" },
    { "name": "amount",           "valueType": "number",  "value": 30.00 },
    { "name": "currency",         "valueType": "string",  "value": "LKR" },
    { "name": "paymentMethod",    "valueType": "string",  "value": "DCB" },
    { "name": "subscriptionRef",  "valueType": "string",  "value": "SUB-88213" }
  ],

  "consent": {
    "@type": "Consent",
    "status": "Accepted",
    "acceptedDate": "2026-09-03T14:02:44+05:30",
    "description": "Subscribe to Premium News Alerts for LKR 30.00 / month. You can cancel any time.",
    "characteristic": [
      { "name": "service_name", "value": "Premium News Alerts", "mandatory": true,  "accepted": true },
      { "name": "price",        "value": "LKR 30.00 / month",   "mandatory": true,  "accepted": true },
      { "name": "sports",       "value": "",                     "mandatory": false, "accepted": false }
    ]
  },

  "relatedParty": [
    { "@type": "RelatedParty", "role": "subscriptionPack", "id": "BASE_MONTHLY", "name": "Premium News Alerts" },
    { "@type": "RelatedParty", "role": "subscriptionPack", "id": "SPORTS",       "name": "Weekly sports digest" }
  ],

  "attachment": [
    {
      "@type": "Attachment",
      "name": "customFields",
      "mimeType": "application/json",
      "content": "{\"nic\":\"199012345678\",\"district\":\"COLOMBO\",\"interests\":[\"SPORTS\",\"MUSIC\"]}"
    },
    {
      "@type": "Attachment",
      "name": "linkEvents",
      "mimeType": "application/json",
      "content": "[{\"key\":\"tnc\",\"clickedAt\":\"2026-09-03T14:02:31+05:30\"}]"
    }
  ]
}
```

Custom fields and link events sit in `attachment` with JSON-encoded `content`, because TMF has no native shape for arbitrary merchant-defined key/value capture. If you find that awkward, use the native format. It is the better fit for this data.

---

<a id="lookup-api"></a>

## Lookup API

The authoritative check. Query by **your** reference or **ours**, in either the native or the TMF format.

<a id="lookup-api-authentication"></a>
### Authentication

Use an OAuth2 Bearer token from the IdeaBiz API Gateway.

```bash
curl -X POST "https://ideabiz.lk/apicall/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials&client_id=KEY&client_secret=SECRET"
```

Create a gateway subscription for the lookup APIs during onboarding.

<a id="lookup-api-endpoints"></a>
### Endpoints

| Method | Path | Query | Format |
|--------|------|-------|--------|
| `GET` | `/v2/api/status` | `ref={ext_ref}` **or** `txnRef={our_txn_ref}` | Native |
| `GET` | `/v2/api/lookup` | `merchant_ref={your_ref}` **or** `providerRef={gateway_ref}` | Native |
| `GET` | `/v2/api/tmf/status` | `ref={ext_ref}` **or** `txnRef={our_txn_ref}` | TMF |
| `GET` | `/v2/api/tmf/lookup` | `merchant_ref={your_ref}` **or** `providerRef={gateway_ref}` | TMF |

A `status` call takes exactly one **unique** selector: `ref` (our `EXT-{uuid}`) or `txnRef` (our `EXTTXN-{uuid}`). A `lookup` call takes one **non-unique** selector: `merchant_ref` (your own ref) or `providerRef` (the payment provider's transaction reference, `provider_txn_ref`). It returns an array. Supplying two selectors at once is rejected with `E9002`.

Published through the gateway as:

```
https://ideabiz.lk/apicall/consent-ext/v1/status?ref={ext_ref}
https://ideabiz.lk/apicall/consent-ext/v1/status?txnRef={our_txn_ref}
https://ideabiz.lk/apicall/consent-ext/v1/lookup?merchant_ref={your_ref}
https://ideabiz.lk/apicall/consent-ext/v1/lookup?providerRef={gateway_ref}
https://ideabiz.lk/apicall/consent-ext-tmf/v1/status?ref={ext_ref}
https://ideabiz.lk/apicall/consent-ext-tmf/v1/lookup?merchant_ref={your_ref}
```

<a id="lookup-api-by-our-reference"></a>
### By our reference

```
GET /v2/api/status?ref=EXT-3f9a1c4e-7b21-4d0e-9c33-5a1f2b8e4d70
Authorization: Bearer {access_token}
```

Returns the same body as the [native notification](#notifications-native-notification-format), so one parser serves both push and pull.

<a id="lookup-api-by-our-transaction-reference"></a>
### By our transaction reference

```
GET /v2/api/status?txnRef=EXTTXN-9f1c8b02-...
```

Same body as `?ref=`. `txnRef` is unique, so this resolves to a single journey.

<a id="lookup-api-by-your-reference-or-the-provider-reference"></a>
### By your reference or the provider reference

```
GET /v2/api/lookup?merchant_ref=camp-778
GET /v2/api/lookup?providerRef=PG-55de21aa
```

Returns an array, because neither your reference nor the provider reference is guaranteed unique on our side:

```json
{
  "status": "SUCCESS",
  "statusCode": "S1000",
  "reference": "camp-778",
  "count": 1,
  "items": [
    {
      "ext_ref": "EXT-3f9a1c4e-...",
      "merchant_ref": "camp-778",
      "status": "COMPLETED",
      "action_status": "SUBSCRIBED",
      "created_at": "2026-09-03T14:02:11+05:30",
      "txn_ref": "EXTTXN-9f1c8b02-...",
      "provider_txn_ref": "PG-55de21aa"
    }
  ]
}
```

Use `lookup` when the customer never came back (a closed tab, a lost redirect) and you only have your own reference. Use it too when reconciling against a payment provider's report and you only have the provider's `providerRef`.

Results are scoped to your application. You cannot read another merchant's journeys.

<a id="lookup-api-errors"></a>
### Errors

Failures return `{ "status": "FAILED", "statusCode": "<code>", "message": "..." }`. The TMF endpoints use `ERR-` codes past authentication. See [Error codes](#error-codes-lookup-api-errors).

---

<a id="code-examples"></a>

## Code examples

Each example covers the same three parts: send the customer, receive the notification, and confirm server-side. Replace the placeholder helper functions (`alreadyProcessed`, `grantAccess` and so on) with your own logic.

<a id="code-examples-php"></a>
### PHP

Send the customer:

```php
<?php
// 1. Send the customer
$slug = 'summer-promo';
$ref  = session_id();
header('Location: https://consent.ideamart.io/v2/offer/' . $slug . '?' . http_build_query([
    'ref'  => $ref,
    'lang' => 'si',
]));
exit;
```

Receive the notification:

```php
<?php
// 2. Receive the notification
$raw  = file_get_contents('php://input');
$sig  = $_SERVER['HTTP_X_SIGNATURE'] ?? '';
$mine = hash_hmac('sha256', $raw, getenv('EXT_HMAC_SECRET'));

if (!hash_equals($mine, $sig)) {
    http_response_code(401);
    exit;
}

$n = json_decode($raw, true);

// At-least-once delivery: be idempotent
if (alreadyProcessed($n['ext_ref'])) {
    http_response_code(200);
    exit;
}

if ($n['status'] === 'COMPLETED' && $n['action_status'] === 'SUBSCRIBED') {
    grantAccess($n['subscriber']['msisdn'], $n['transaction']['subscription_ref']);
}
markProcessed($n['ext_ref']);
http_response_code(200);
```

Confirm server-side (authoritative):

```php
<?php
// 3. Confirm server-side
$ch = curl_init('https://ideabiz.lk/apicall/consent-ext/v1/status?ref=' . urlencode($extRef));
curl_setopt_array($ch, [
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_HTTPHEADER     => ['Authorization: Bearer ' . $token],
]);
$res = json_decode(curl_exec($ch), true);
curl_close($ch);

if (($res['status'] ?? '') === 'COMPLETED') {
    // safe to deliver value
}
```

<a id="code-examples-nodejs"></a>
### Node.js

```javascript
import express from 'express';
import crypto from 'crypto';

const app = express();

// Raw body is required for HMAC verification
app.post('/api/he-notify',
  express.raw({ type: 'application/json' }),
  async (req, res) => {
    const expected = crypto
      .createHmac('sha256', process.env.EXT_HMAC_SECRET)
      .update(req.body)
      .digest('hex');
    const received = req.get('X-Signature') || '';

    const a = Buffer.from(expected);
    const b = Buffer.from(received);
    if (a.length !== b.length || !crypto.timingSafeEqual(a, b)) {
      return res.sendStatus(401);
    }

    const n = JSON.parse(req.body.toString());

    if (await alreadyProcessed(n.ext_ref)) return res.sendStatus(200);

    if (n.status === 'COMPLETED') {
      switch (n.action_status) {
        case 'SUBSCRIBED':
        case 'ALREADY_ACTIVE':
          await grantAccess(n.subscriber.msisdn);
          break;
        case 'CHARGED':
          await recordPayment(n.transaction.txn_ref, n.transaction.amount);
          break;
        case 'CONSENT_RECORDED':
          await recordConsent(n.consent);
          break;
      }
    }

    await markProcessed(n.ext_ref);
    res.sendStatus(200);
  });
```

<a id="code-examples-python"></a>
### Python

```python
import hmac, hashlib, requests

def verify(raw: bytes, signature: str, secret: str) -> bool:
    expected = hmac.new(secret.encode(), raw, hashlib.sha256).hexdigest()
    return hmac.compare_digest(expected, signature or "")

def confirm(ext_ref: str, token: str) -> dict:
    r = requests.get(
        "https://ideabiz.lk/apicall/consent-ext/v1/status",
        params={"ref": ext_ref},
        headers={"Authorization": f"Bearer {token}"},
        timeout=15,
    )
    return r.json()
```

<a id="code-examples-tips"></a>
### Tips

- Verify the signature over the **raw** body, before parsing.
- Make the handler idempotent on `ext_ref`.
- Return a `2xx` quickly and do heavy work afterwards, to avoid timeouts and retries.
- Never deliver value on the redirect alone. Confirm with the [lookup API](#lookup-api).

---

<a id="error-codes"></a>

## Error codes

The status and error codes you or your customers can see in the redirect, the notification, the lookup API and the journey pages. Each family maps to one area, so a code alone tells you where to look.

| Family | Area | Seen by |
|--------|------|---------|
| `S1xxx` | Success and terminal outcomes | You (redirect, notification, lookup) |
| `E1xxx` | Entry and session lifecycle | Customer (page), you (redirect) |
| `E2xxx` | Security vendor and captcha | Customer, you |
| `E3xxx` | OTP | Customer |
| `E4xxx` | Consent | Customer |
| `E5xxx` | Subscription, charge and payment | Customer, you |
| `E6xxx` | Custom fields and trackable links | Customer |
| OAuth2 identifiers | OAuth2 entry (`invalid_grant`, `access_denied`, ...) | Your OAuth client |
| `E9xxx` | Lookup API and pre-filled link API | Your backend |

<a id="error-codes-success-and-terminal-status"></a>
### Success and terminal status

<a id="error-codes-status-values"></a>
#### `status` values

| `status` | Meaning |
|----------|---------|
| `COMPLETED` | Journey finished. Check `action_status` for what was done. |
| `ABANDONED` | The customer left, or the session expired before a terminal step. |
| `FAILED` | An action or payment was attempted and failed. |
| `BLOCKED` | Stopped by security or captcha. |

<a id="error-codes-action_status-values"></a>
#### `action_status` values

| Value | Meaning |
|-------|---------|
| `CONSENT_RECORDED` | Consent captured, nothing else required. |
| `SUBSCRIBED` | New subscription created. |
| `ALREADY_ACTIVE` | The subscriber was already subscribed. No new request was made. |
| `CHARGED` | One-time charge succeeded. |
| `NONE` | No action configured, or the journey ended before the action. |

<a id="error-codes-success-codes"></a>
#### Success codes

| Code | HTTP | `status` | Meaning |
|------|------|----------|---------|
| `S1000` | 200 | `COMPLETED` | Journey completed successfully. |
| `S1001` | 200 | `COMPLETED` | Completed with nothing to do, already subscribed. |
| `S1002` | 200 | `ABANDONED` | The customer did not finish. Recorded, no action taken. |
| `S1003` | 200 | `COMPLETED` | Completed but **flagged**. A security validation error was allowed through by your app's policy. |

`S1003` is a genuine success for the customer and a signal for you. The journey completed, but the anti-fraud check could not be confirmed. Treat it as lower confidence than `S1000` if your risk model cares.

<a id="error-codes-e1xxx-entry-and-session"></a>
### E1xxx: entry and session

| Code | HTTP | Condition | Customer sees |
|------|------|-----------|---------------|
| `E1001` | 404 | Slug not found. | Branded not-found page. |
| `E1002` | 403 | Journey inactive or not enabled. | "This offer is not available". |
| `E1003` | 400 | Session not found. | "This link is not valid". |
| `E1004` | 410 | Session expired. | Expired page with a restart link. |
| `E1005` | 400 | Step accessed out of order, or replayed. | "This link has already been used". |
| `E1006` | 302 | Same as `E1005`, handled silently. | Redirected to you with `status=ABANDONED`, session flagged. |
| `E1007` | 409 | A desktop tab tried to advance after the phone took over. | "Continue on your phone". |
| `E1008` | 400 | A required parameter is missing or malformed on entry. | Branded error page. |
| `E1011` | 409 / 410 | A pre-filled link with a submit limit has been used that many times. 410 on entry, 409 if a journey already open on it is submitted. | "This link has already been used". |

Reloading the current step, or reloading the outcome page after completion, is allowed and is not an error. Going back to a completed step gives `E1005` or `E1006`.

<a id="error-codes-e2xxx-security-and-captcha"></a>
### E2xxx: security and captcha

| Code | HTTP | Condition | Result |
|------|------|-----------|--------|
| `E2001` | 403 | The security vendor returned a **clear failure**. | **Always blocked.** |
| `E2002` | 403 | The vendor errored, timed out or was ambiguous, and your policy is to block. | Blocked, flagged. |
| `E2003` | 403 | Captcha verification failed. | Blocked, flagged. |
| `E2004` | 403 | Captcha could not be verified and your policy is to block. | Blocked, flagged. |
| `E2005` | 400 | A `security_ref` was required but not supplied. | Treated as a failure, blocked. |
| `E2010` | 410 | QR pairing token expired. | Expired page, new QR offered. |
| `E2011` | 409 | QR pairing token already used by another device. | Blocked. |

<a id="error-codes-how-your-error-policy-applies"></a>
#### How your error policy applies

Your app has a policy for security checks that cannot be confirmed (`CONTINUE` or `BLOCK`). It governs **only** the ambiguous case. An explicit vendor rejection always blocks.

| Vendor outcome | Policy `CONTINUE` | Policy `BLOCK` |
|----------------|-------------------|----------------|
| Clear success | Proceed. | Proceed. |
| **Clear failure** | **Blocked, `E2001`.** | **Blocked, `E2001`.** |
| Error / timeout / ambiguous | Proceed, flagged, `S1003`. | Blocked, `E2002`. |

<a id="error-codes-e3xxx-otp"></a>
### E3xxx: OTP

| Code | HTTP | Condition | Customer sees |
|------|------|-----------|---------------|
| `E3001` | 400 | Invalid number format or unsupported operator prefix. | Inline field message. |
| `E3002` | 200 | The OTP could not be sent. | "Please try again later" with a Resend button. |
| `E3003` | 410 | Code expired. | "That code has expired" and a request for a new one. |
| `E3004` | 429 | Verify attempts exhausted. | "Too many attempts". The journey ends. |
| `E3005` | 429 | Resend requested too soon. | Countdown shown, button disabled. |
| `E3006` | 429 | Resend limit reached. | Resend hidden. |
| `E3007` | 400 | Wrong code, attempts remaining. | Inline message and attempt counter. |
| `E3008` | 403 | The customer tried to change the number, which your app does not allow. | Change option not offered. |

`E3004` is terminal for the session. The customer must start a new journey. This is deliberate, since a lockout that could be reset by reloading is not a lockout.

<a id="error-codes-e4xxx-consent"></a>
### E4xxx: consent

| Code | HTTP | Condition | Customer sees |
|------|------|-----------|---------------|
| `E4001` | 400 | Consent not accepted but required to continue. | "You need to accept before continuing". |
| `E4002` | 400 | A mandatory item was submitted as unticked. | Mandatory items render locked, so this indicates tampering. |
| `E4003` | 400 | Unknown consent item submitted. | Rejected, not stored. |

Mandatory items are enforced on our servers, and unknown items are rejected rather than passed into your notification. The record of what a customer agreed to is immutable. Later configuration edits never rewrite it.

<a id="error-codes-e5xxx-subscription-charge-and-payment"></a>
### E5xxx: subscription, charge and payment

<a id="error-codes-subscription"></a>
#### Subscription

| Code | HTTP | Condition |
|------|------|-----------|
| `E5001` | 502 | The subscription platform rejected the request or was unreachable. |
| `E5002` | 400 | The requested pack code is unknown, inactive, or not selectable by URL. |
| `E5003` | 409 | No default pack configured for this journey. |
| `E5004` | 502 | The subscription status check failed and could not be assumed safe. |

An already-active subscriber is not an error. It returns `S1001` with `action_status=ALREADY_ACTIVE`.

<a id="error-codes-charge"></a>
#### Charge

| Code | HTTP | Condition |
|------|------|-----------|
| `E5010` | 400 | Amount outside the minimum and maximum configured for your app. |
| `E5011` | 403 | You supplied an amount, but vendor-supplied amounts are not allowed. |
| `E5012` | 400 | No amount available: none supplied and no default configured. |
| `E5013` | 502 | The charging service rejected the request or was unreachable. |

Limits are enforced server-side even when you supply the amount, so a tampered URL cannot raise the charge.

<a id="error-codes-payment"></a>
#### Payment

| Code | HTTP | Condition |
|------|------|-----------|
| `E5020` | 400 | No payment method available for this device and configuration. |
| `E5021` | 402 | The provider declined the payment. |
| `E5022` | 499 | The customer cancelled at the provider. |
| `E5023` | 410 | The payment or QR session expired at the provider. |
| `E5024` | 502 | The provider was unreachable, or the response could not be verified. |
| `E5025` | 400 | The provider's callback signature was invalid. The response is not trusted. |
| `E5026` | 409 | A recurring subscription needed a stored card token and none was returned. |
| `E5027` | 403 | The selected saved card is not eligible for reuse. |
| `E5028` | 400 | The selected payment provider is not enabled for this journey. |

A saved card is only offered when the current session is OTP-verified **and** the card was itself stored under OTP verification. The browser return from a provider is never trusted on its own. We confirm the outcome with the provider server-side.

<a id="error-codes-e6xxx-custom-fields-and-links"></a>
### E6xxx: custom fields and links

| Code | HTTP | Condition | Customer sees |
|------|------|-----------|---------------|
| `E6001` | 400 | Required field missing. | Inline "this is required". |
| `E6002` | 400 | Value did not match the field's pattern. | The field's own message. |
| `E6003` | 400 | Value outside the minimum or maximum length. | Inline length message. |
| `E6004` | 400 | Submitted option is not one of the configured options. | Rejected, not stored. |
| `E6005` | 400 | A required link was never opened. | Continue stays disabled with an explanation. |
| `E6006` | 400 | Multiple values submitted for a single-value field. | Rejected. |
| `E6007` | 400 | Unknown field submitted. | Rejected, not stored. |
| `E6008` | 400 | Number outside the allowed range. | Inline range message. |
| `E6010` | 400 | File larger than the field's size limit (hard cap 10 MB). | "The file is too large". |
| `E6011` | 400 | Not a readable image, or a PDF with unsafe content. | Type-specific message. |
| `E6012` | 400 | Image too small (shortest side under 320 px). | "Too small or blurry". |
| `E6013` | 400 | A capture guide needs two photos and only one was sent. | "Please add every photo: Front, Back". |
| `E6014` | 400 | File format not allowed for the field. | Allowed formats listed. |
| `E6015` | 400 | The file could not be stored. | "Please try again". |
| `E6016` | 400 | A pre-filled form was submitted without the "I have checked these details" tick. | "Please confirm the details". |
| `E6017` | 400 | An optional AI check said the file is not the requested document, on a field set to reject mismatches. | "This doesn't look like the document we asked for". |
| `E6018` | 400 | A location that must come from the device got a searched or typed place, or the device position was not precise enough. | A message asking the customer to share their device position, or use their phone. |
| `E6019` | 400 | A scanned code limited to retail types (EAN / UPC) is not a valid code of those types. | "This isn't a valid barcode". |

Validation is enforced **server-side**. The browser-side check is a convenience only. Because unknown fields and options are rejected, the notification you receive can only contain fields and option values you defined.

<a id="error-codes-oauth2-errors"></a>
### OAuth2 errors

The OAuth2 entry uses standard RFC 6749 and RFC 6750 error identifiers, so an off-the-shelf client library recognises them.

<a id="error-codes-returned-on-the-redirect-from-v2authorize"></a>
#### Returned on the redirect from `/v2/authorize`

```
https://vendor.com/callback?error=invalid_scope&error_description=...&state=...
```

| `error` | Condition |
|---------|-----------|
| `invalid_request` | Missing or malformed `response_type`, `client_id` or `redirect_uri`. |
| `unauthorized_client` | `client_id` unknown or inactive, or OAuth is not enabled for the app. |
| `unsupported_response_type` | Anything other than `code`. |
| `invalid_scope` | Requested scope is not allowed for your app. |
| `access_denied` | The customer abandoned, or the journey was blocked by security or captcha. |
| `server_error` | Internal failure. |
| `temporarily_unavailable` | A dependency is unavailable. |

!!! note
    If `redirect_uri` fails validation, the error is **not** redirected. An unvalidated redirect target would be an open redirector, so the customer sees a branded error page instead.

<a id="error-codes-returned-from-post-v2token"></a>
#### Returned from `POST /v2/token`

```json
{ "error": "invalid_grant", "error_description": "authorization code already used" }
```

| `error` | HTTP | Condition |
|---------|------|-----------|
| `invalid_request` | 400 | Missing `grant_type`, `code`, `redirect_uri`, or `code_verifier` when PKCE is required. |
| `invalid_grant` | 400 | Code unknown, expired, already used, or issued to a different `redirect_uri`. |
| `invalid_client` | 401 | `client_id` unknown or mismatched. |
| `unsupported_grant_type` | 400 | Anything other than `authorization_code`. |
| `invalid_scope` | 400 | Scope escalation attempted at exchange. |

`invalid_grant` deliberately covers several distinct failures with one identifier, so it does not tell an attacker which one they hit.

<a id="error-codes-returned-from-get-v2userinfo"></a>
#### Returned from `GET /v2/userinfo`

Per RFC 6750, in the `WWW-Authenticate` header:

| `error` | HTTP | Condition |
|---------|------|-----------|
| `invalid_token` | 401 | Token unknown, expired or revoked. |
| `insufficient_scope` | 403 | Token lacks the scope for the requested data. |

<a id="error-codes-lookup-api-errors"></a>
### Lookup API errors

| Code | HTTP | Condition |
|------|------|-----------|
| `E9001` | 400 | No reference supplied (or, for the pre-filled link API, `links` is empty or not an array). |
| `E9002` | 400 | More than one selector supplied, or (pre-filled link API) a malformed link, value, number, date, `maxUses` or `offer`, or more than 1000 links. The message names which. |
| `E9003` | 400 | *(Pre-filled link API)* The body is not a JSON object. |
| `E9010` | 401 | Missing Bearer token. |
| `E9011` | 401 | Invalid or expired token. |
| `E9012` | 403 | *(Pre-filled link API)* The gateway token cannot be tied to one app, so it cannot create links. |
| `E9030` | 403 | Application not registered, or the journey belongs to another app. |
| `E9040` | 404 | No journey found for the given reference. (Pre-filled link API: the `slug` is not an app of the calling application.) |
| `E9041` | 409 | *(Pre-filled link API)* Pre-filled links are not enabled for the app. |
| `E9050` | 500 | Internal error. |

Body shape: `{ "status": "SUCCESS|FAILED", "statusCode": "<code>", "message": "..." }`.

Results are scoped to the calling application. One merchant cannot read another's journeys.

<a id="error-codes-tmf-endpoints"></a>
#### TMF endpoints

The TMF-format endpoints use TMF-style codes past authentication.

| Code | HTTP | Condition |
|------|------|-----------|
| `ERR-0001` | 400 | Missing or invalid required parameter. |
| `ERR-0004` | 404 | Resource not found. |
| `ERR-0005` | 500 | Internal error. |

```json
{
  "code": "ERR-0004",
  "reason": "Resource not found",
  "message": "No journey found for the given reference",
  "status": "404"
}
```

Authentication failures happen before the TMF handler runs, so those responses use the `E90xx` shape rather than TMF codes.

<a id="error-codes-sign-in-has-no-customer-facing-code"></a>
### Sign-in has no customer-facing code

A failed or cancelled sign-in returns the customer to the sign-in page with a general message and does not say why. A stale or already-used sign-in link shows a plain "This sign-in link has expired" page (`E1003`).

---

<a id="troubleshooting"></a>

## Troubleshooting

| Symptom | Likely cause | What to do |
|---------|--------------|------------|
| Landing page shows a QR code on desktop | Your app requires mobile data. | Expected. The customer continues on their phone so the number cannot be swapped. |
| `status=NOT_DETECTED` then a number entry screen | WiFi, or no header enrichment. | Expected. Detection needs mobile data. The journey falls back to manual entry plus OTP. |
| `lang` parameter ignored | Language not enabled for your app. | Ask for it to be added. Unknown values are ignored by design. |
| `banner_id` ignored | Banner not approved for your app. | Approval is required before a banner can be selected by URL. |
| `amount` ignored or rejected | Vendor amounts disabled, or outside the minimum and maximum. | Amounts are always limited server-side. |
| Saved card not offered | OTP disabled, or the card was stored without OTP verification. | Both the session **and** the card must be OTP-verified. |
| Notification never arrives | Endpoint unreachable, non-2xx response, or retries exhausted. | Ask support to check the delivery log, and use the lookup API meanwhile. |
| Notification arrives twice | At-least-once delivery. | Make your handler idempotent on `ext_ref`. |
| HMAC check fails | Signature computed over parsed JSON. | Compute it over the **raw** body bytes before parsing. |
| `status=BLOCKED` | Security vendor or captcha refused. | A genuine refusal. Support can see the flags on the session. |
| Journey completed but nothing granted | You acted on the redirect only. | Confirm with the lookup API before delivering value. |
| A pre-filled link says "already been used" | The link's submit limit was reached (`E1011`). | Create a new link, or ask for the limit to be raised. |
| A pack code is refused | Wrong, expired, used up, not accepted by the app, or more than 10 tries in an hour. | Check the code, its expiry and its usable count in your account portal. |
| A sign-in button is missing | The provider is not yet approved, is switched off, or its client has no secret. | Ask your onboarding contact to approve it. |
| Sign-in "didn't complete" | The customer cancelled, opened the sign-in in another browser, or the provider refused. | Expected. The customer can try again or continue without signing in. |
| A scanned code is refused | Wrong code type, fails the value format, or (EAN / UPC) a bad check digit (`E6019`). | Check the accepted types and the pattern on the field. |
| Lookup returns nothing for your ref | Wrong reference, or the journey belongs to a different app. | `merchant_ref` must match exactly, and results are scoped to your app. |
| Entry URL says "This offer is not available" | The journey is not active yet (`E1002`). | Confirm with your onboarding contact that it has been activated. |
| Customer sees "This link is not valid" or an expired page | The session is unknown or has expired (`E1003`, `E1004`). | Start a new journey from your entry URL. |

If you still need help, send your onboarding contact the `ext_ref`, your `ref`, the time of the attempt and the `code` you received. See [Error codes](#error-codes) for what each code means.

---

<a id="onboarding-checklist"></a>

## Onboarding checklist

Work through this list with your onboarding contact. Items marked "if" apply only when you use that feature.

- [ ] Core app registered and `client_id` issued.
- [ ] Consent v2 enabled and your **slug** agreed. This becomes your public URL.
- [ ] Running several campaigns or brands under one `client_id`? Each journey gets its own slug. Say which one (if any) should receive the automatic `/web/v1/authorize` handoff. See [Shared core app and multiple journeys](#shared-core-journeys).
- [ ] Callback / return URL registered.
- [ ] Journey agreed: OTP mode, consent type, subscription or charge, payment methods.
- [ ] Notification endpoint registered, and format chosen (**native** or **TMF**, or both).
- [ ] Notification authentication chosen. HMAC secret exchanged if applicable.
- [ ] Custom fields defined, with value-format patterns tried in the pattern editor in your account portal.
- [ ] If you collect photos, PDFs or codes: field limits and capture guides agreed, and optional AI checks and an AI budget set for the app.
- [ ] If you use several forms: the show rules written down and tested with each answer.
- [ ] If you use pre-filled links: lock, expiry and submit limit decided, and code entry (needs captcha) agreed.
- [ ] If you offer add-ons or pack codes: IdeaBiz subscription ids for each add-on, and which subscriptions they go with.
- [ ] If you want sign-in: provider(s) chosen, redirect URI `https://consent.ideamart.io/v2/sso/callback` registered with each OAuth client, and approval requested.
- [ ] Trackable links (such as terms) supplied, and whether a click is mandatory.
- [ ] Languages enabled, and language priority decided (your `lang` tag or the customer's remembered choice).
- [ ] Theme and banner artwork supplied, with alt text.
- [ ] Page title, description and sharing image supplied.
- [ ] Analytics IDs supplied, if you want secondary tags.
- [ ] Gateway subscription created for the lookup APIs.
- [ ] Tested end to end on mobile data **and** on desktop (desktop may show the QR handoff).

<a id="onboarding-checklist-before-you-go-live"></a>
### Before you go live

1. Run a complete journey for each slug and each variant you plan to publish.
2. Test failure paths too: abandon halfway, enter a wrong OTP, and cancel a payment.
3. Confirm your endpoint handles a repeated notification safely.
4. Confirm that your backend checks the [lookup API](#lookup-api) before granting anything.

---

<a id="customization"></a>

## Customization overview

Consent v2 is configured, not coded. You describe the journey you want, and we run it. Your onboarding contact (or your account portal, where your account has one) applies the settings. Everything on this page is **opt-in**. An app that turns none of it on runs the basic journey.

<a id="customization-what-you-can-customize"></a>
### What you can customize

| Area | What you can change | Page |
|------|---------------------|------|
| What you collect | Text, choices, locations, files, scanned codes and links, with validation. | [Custom fields](#custom-fields) |
| How you link | Variants with fixed choices, and per-customer pre-filled links. | [Variants and pre-filled links](#variants-prefilled-links) |
| How it looks | Colors, logo and banners. | [Branding and theming](#branding-theming) |
| Form layout | Several forms in a row, with rules for when each shows. | [Multiple forms and show-rules](#forms-show-rules) |
| Language | Which languages, and which source wins. | [Languages](#languages) |
| What you sell | Subscription, one-time charge, payment methods. | [Subscription and payment options](#subscription-payment) |
| Extras | Add-ons and pack codes. | [Add-ons and packs](#add-ons-packs) |
| Identity | Google, Microsoft or third-party sign-in. | [Sign-in providers](#sign-in-providers) |
| Several brands | Multiple journeys on one registration. | [Shared core app and multiple journeys](#shared-core-journeys) |

<a id="customization-how-settings-are-applied"></a>
### How settings are applied

Where a setting can come from more than one place, the most specific wins:

1. A URL parameter, only where your app allows it for that setting.
2. Your journey's own setting.
3. The platform default.

A link can therefore adjust a theme or language, but cannot select anything your app has not been approved to use.

<a id="customization-safe-by-default"></a>
### Safe by default

- Everything that could expose a journey to a customer starts switched off. A journey is checked before it goes live.
- Validation always runs on our servers. A customer cannot skip a rule by editing the page.
- Changing your configuration never affects journeys already in progress, and never rewrites what a customer already agreed to.

<a id="customization-the-onboarding-checklist"></a>
### The onboarding checklist

The [onboarding checklist](#onboarding-checklist) lists what to decide and supply for each of these areas.

---

<a id="custom-fields"></a>

## Custom fields

Beyond the customer's number and consent, a journey can collect extra details. Fields are configured for your app, shown on the page you choose, validated on our servers, and delivered to you in the `custom_fields` part of the [notification](#notifications).

<a id="custom-fields-where-a-field-can-appear"></a>
### Where a field can appear

| Page | Use for |
|------|---------|
| Form page | The usual place for details. Supports several forms. |
| Consent page | Details tied to accepting the terms. |
| Number page | Details tied to the mobile number. |
| Payment page | Details tied to payment. |

File, camera and barcode fields can only be placed on the form page or the consent page.

<a id="custom-fields-field-types"></a>
### Field types

| Type | What the customer sees | Value you receive |
|------|------------------------|-------------------|
| Text, long text | Text box. | The text. |
| E-mail, URL, phone, date | Typed value with the format checked. | The text. |
| Number | Number with optional minimum and maximum. | The number. |
| Dropdown, radio | One option, from a fixed list or from an API. | The option value. |
| Checkbox, multi-select | Several options. | A list of option values. |
| Location | Map with address search and "detect my location". | `{address, lat, lng}` as a JSON string. |
| Open a link | A card that opens a page, such as Terms. | `AGREED` once opened. |
| Photo | Photo upload (JPG, PNG, WebP). | `1 file` / `2 files`. |
| PDF | PDF upload. | `1 file`. |
| Camera | Live camera capture, with an optional guide. | `1 file` / `2 files`. |
| Barcode | Scan a QR code or barcode. | The text inside the code. |

Labels, help text and placeholders can be translated per language. If a translation is empty, the label you typed is used. A help text or placeholder with no translation and no typed text is not shown.

**Option values are checked against your configured set.** A value that is not one of your options is refused, so a notification only contains values you defined.

<a id="custom-fields-value-format-patterns"></a>
### Value format (patterns)

Text, long text, e-mail, URL, phone and barcode fields can carry a pattern and a message shown when it does not match.

- The **whole value** must match the pattern.
- A pattern can be at most 255 characters.
- Patterns that can freeze a page on some inputs are refused when you save them.
- Ready-made patterns are offered, for example Sri Lankan NIC, mobile number, passport, vehicle number, postal code, digits, letters, e-mail and https link.
- A **try values** box lets you type sample answers and see which are accepted, using the same rule customers get. Use it before go-live.

The pattern runs again on our servers when the customer submits.

<a id="custom-fields-open-a-link-terms-and-conditions"></a>
### Open a link (terms and conditions)

A link field shows a card. Opening it is recorded by the server, and then its tick appears. The tick cannot be set by hand.

| Setting | Effect |
|---------|--------|
| Link address | Must start with `https://`. |
| Link text | Text on the card (default "Read Terms and Conditions"). |
| Reference parameter | The name of a query parameter that carries this journey's `ext_ref` to your page. Default `ref`. |
| Require reading | Continue stays disabled until every required link was opened. We also enforce this on the server. |

You receive each click time in `link_events`.

<a id="custom-fields-location"></a>
### Location

A location field stores the address and coordinates. It can be set to **device only**, so the customer can share only the device's own position (no search, no tapping the map) with an accuracy of 1000 m or better. On a computer, the page offers to finish this on the phone. A pre-filled link cannot carry a device-only location.

This stops typing in an address. It cannot prove a position against someone who fakes their GPS.

<a id="custom-fields-photos-pdfs-and-camera"></a>
### Photos, PDFs and camera

- Files must be within the field's size limit (never above 10 MB).
- The file type is decided from its content, not from its name.
- Images must be readable and not too small (shortest side at least 320 px). They are re-saved on our side, which removes hidden metadata such as GPS and device details.
- PDFs with encryption, scripts, launch actions, attachments or rich media are refused.
- Files are stored encrypted and are **never** sent in a notification. Your app's owners can view them in your account portal.
- A **capture guide** tells the customer how to take the photo: a frame (oval, card, passport, A4 or none), which camera, tips, and one or two sides (for example a NIC front and back). Standard guides are available. Guides you create for your own app are reviewed before use.
- **Use your phone**: on a computer, a camera field, a required photo or a device-only location offers a QR code so the customer can finish that part on their phone, then continue on the computer.

<a id="custom-fields-optional-ai-check"></a>
#### Optional AI check

A photo or PDF field can ask for an automatic check that the file is the document you described. It can also read listed details (such as a NIC number) into empty fields that the customer then confirms. Each app has a monthly usage budget. If the check errors, has no budget or is unsure, the customer is never blocked. Only an explicit mismatch on a field set to **reject mismatches** refuses a file.

Your account portal has a test box on each file field where you can upload a sample and see each check without saving anything.

<a id="custom-fields-qr-codes-and-barcodes"></a>
### QR codes and barcodes

A barcode field stores the text inside a QR code or barcode.

| Setting | Meaning |
|---------|---------|
| Code types accepted | Any of QR, Data Matrix, PDF417, Aztec, EAN-13, EAN-8, UPC-A, UPC-E, Code 128, Code 39, Code 93, ITF, Codabar. None ticked means any. |
| Customer can also type the value | On by default. |
| Value format | The same pattern tool as above. |

Customers can scan live with their camera (in browsers that support it), scan from a photo (works on every phone), or type the value if you allow it. A scanned photo is read in memory and never stored. When every accepted type is EAN or UPC, the check digit is verified too.

!!! note
    A scan is not proof that a code was physically scanned. The browser only fills the text box, and we validate the value on submit.

<a id="custom-fields-what-the-customer-sees-afterwards"></a>
### What the customer sees afterwards

The result page lists what the customer entered, in your form's order and the page's language: option names instead of codes, a location's address, "2 files uploaded", "Opened" for a terms link. Empty answers are skipped, and nothing is shown if the journey failed.

---

<a id="forms-show-rules"></a>

## Multiple forms and show-rules

The form page can hold more than one form, shown one after another like a wizard. Later forms can appear only when earlier answers say so.

<a id="forms-show-rules-how-forms-are-arranged"></a>
### How forms are arranged

- **Form 1** always shows. Its title and description are your normal form title and description.
- **Forms 2 to 10** each have a title (also their step name), a description and a **show rule**.
- A form with **no rule** always shows. So "form 2 is not related to form 1" is simply a form with no rule.
- You place each field on a form with the field's **Form** setting. This applies to form-page fields.

<a id="forms-show-rules-show-rules"></a>
### Show rules

A rule combines up to 10 conditions on answers from **earlier forms**. It matches when **all** of them are true, or when **any** of them is, as you choose.

| Operator | Meaning |
|----------|---------|
| `EQ` / `NEQ` | Is / is not. Case-insensitive, and matches any value of a multi-value answer. |
| `IN` / `NOT_IN` | Is one of / none of a list. |
| `CONTAINS` | The text contains a value. |
| `FILLED` / `EMPTY` | Has / has no answer. |
| `GT` / `LT` | More than / less than. Numbers only. A missing number never matches. |

Example: show "Business details" only when the answer to "Account type" is `BUSINESS`, or "Parent details" when "Age" is less than 18.

<a id="forms-show-rules-behaviour"></a>
### Behaviour

- **Rules run on our servers** against the answers already stored (and any pre-filled values). A customer cannot reach a hidden form by editing the page, because the form to show is always recomputed.
- Each form is validated and saved when the customer presses Continue. **Back** returns to the previous shown form without checking or uploading anything, and keeps the answers.
- The page shows "Form 2 of 3" and a progress bar. With the step bar on, each shown form is one entry named by its title.
- Required links are checked on the **last** shown form. The 8-digit code box shows only on form 1.
- If a customer goes back and changes an answer so that a form becomes hidden, the answers and uploaded files on that hidden form are removed. They never reach you.
- If you have no extra forms, everything is one form.

<a id="forms-show-rules-before-go-live"></a>
### Before go-live

Write the rules down and try every combination of answers that decides whether a form shows.

---

<a id="variants-prefilled-links"></a>

## Variants and pre-filled links

Two ways to give a customer a link that already carries choices.

| | Variant | Pre-filled link |
|---|---------|-----------------|
| Made for | A campaign or package, shared by many customers. | One specific customer. |
| URL | `/v2/offer/{slug}/{variant}` | `/v2/offer/{slug}/{link_code}` or `/s/{link_code}` |
| Holds | Subscription packs, charge amount, theme, banner, language, custom-field defaults. | That customer's details, an expiry, a submit limit, and optionally an offer. |
| Created | During onboarding. | By you, whenever you need one. |

<a id="variants-prefilled-links-variants"></a>
### Variants

A variant is a second path segment under your slug:

```
https://consent.ideamart.io/v2/offer/summer-promo/sports-pack        SPORTS preselected
https://consent.ideamart.io/v2/offer/summer-promo/premium-bundle     BASE + SPORTS + MUSIC
https://consent.ideamart.io/v2/offer/summer-promo/topup-100          one-time charge, LKR 100
```

A variant can preset the subscription packs, the charge amount, the theme, the banner, the language and custom-field defaults. Any of these can be **locked**, so the customer cannot change what your link chose. For OAuth2 journeys, pass `variant={variant_slug}` to apply the presets.

Prefer a variant over `?pack=` or `?amount=` when the value is fixed for a campaign, because the preset is held on our side and cannot be edited in the URL. See [Entry URL](#entry-url-2-variant-sub-urls).

<a id="variants-prefilled-links-pre-filled-links"></a>
### Pre-filled links

A pre-filled link opens the normal journey with the form already filled in.

```
https://consent.ideamart.io/v2/offer/{slug}/{link_code}      full link
https://consent.ideamart.io/s/{link_code}                    short link
https://consent.ideamart.io/v2/offer/{slug}?pf={link_code}   also accepted
```

- The URL carries **only an opaque code**. The values, the lock setting and anything uploaded are held on our side, never in the query string, because a customer can edit a query string.
- A link code is 7 readable characters (no 0, O, 1, I or L).
- Each link also has an **8-digit access code**, unique within your app.
- Extra query parameters on the link (`ref`, `lang`, campaign values) are passed through as usual.

<a id="variants-prefilled-links-settings-that-apply"></a>
#### Settings that apply

| Setting | Meaning |
|---------|---------|
| Allow pre-filled links (app) | When off, links cannot be created and existing ones open an empty form. |
| Lock pre-filled fields by default (app) | The customer cannot edit the values. Anything posted for a locked field is ignored and the stored value is used. |
| Lock (per link) | Overrides the app default. |
| Expiry (per link) | After this date the link opens a normal journey. |
| Can be submitted (per link) | Once, a set number of times, or unlimited. A use is a **completed** journey from the link. |
| Bound mobile (per link) | The 8-digit code then works only for that number. |
| Offer (per link) | A subscription, add-ons or a pack code to preselect. See [Add-ons and packs](#add-ons-packs). |
| Disabled | The link stops working at once. |

A link that has reached its limit shows "This link has already been used" (`E1011`).

Some fields are **never** pre-filled, because the customer must do them: opened links, photos, PDFs, camera captures and device-only locations.

When values were filled in for the customer, the form says "This form was filled in for you", and the customer must tick **"I have checked these details"** before continuing (`E6016`).

<a id="variants-prefilled-links-entering-a-code-by-hand"></a>
#### Entering a code by hand

A customer who has a code but not the link has three places to type it:

| Code | Where |
|------|-------|
| Link code (7 or 8 characters) | The landing page, **Have a link code?**. Or open `/v2/offer/{slug}?code=1` to go straight to the box. |
| 8-digit access code | The form step, **Have an 8-digit code?** |
| Pack code | The consent step, **Have a pack code?**, only when your app accepts pack codes. |

Because an 8-digit code can be guessed by a script, code entry is only offered when the customer's number is verified, captcha is enabled and passes, fewer than 5 wrong codes were tried in the journey, and (for a bound link) the same verified number is used. Code entry cannot be switched on without captcha.

<a id="variants-prefilled-links-creating-links-from-your-own-system"></a>
#### Creating links from your own system

```
POST https://consent.ideamart.io/v2/api/prefill-links
Authorization: Bearer {gateway token}
Content-Type: application/json
```

```json
{
  "slug": "summer-promo",
  "batchName": "June offer",
  "lock": true,
  "expiresAt": "2026-12-31",
  "maxUses": 1,
  "offer": { "packCode": "GOLD", "addonCodes": ["MUSIC"] },
  "links": [
    { "externalId": "APP-1001", "ref": "ORD-77", "msisdn": "0771234567",
      "values": { "full_name": "Nimal Perera", "district": "COLOMBO" },
      "offer": { "bundleCode": "JUNE50" } }
  ]
}
```

| Field | Notes |
|-------|-------|
| `slug` | Your journey. It must belong to the calling application. |
| `links` | 1 to 1000 per call. Each may carry its own `values`, `externalId`, `ref`, `msisdn` and `offer`. |
| `lock`, `expiresAt`, `maxUses`, `batchName` | Apply to every link in the call. `lock` defaults to your app setting. `expiresAt` is `YYYY-MM-DD` or `YYYY-MM-DD HH:MM:SS`. `maxUses` is 1 to 100000, or omit for unlimited. |
| `offer` | `packCode` (a subscription), `addonCodes`, `bundleCode` (a pack code). A per-link `offer` overrides the call's. |

A successful call returns `201`:

```json
{ "status": "SUCCESS", "batchId": 12, "locked": true, "maxUses": 1, "codeEntry": true,
  "links": [ { "externalId": "APP-1001", "ref": "ORD-77", "msisdn": "9477***33",
               "linkCode": "K7M3QXR", "accessCode": "48210937",
               "url": "https://consent.ideamart.io/s/K7M3QXR",
               "fullUrl": "https://consent.ideamart.io/v2/offer/summer-promo/K7M3QXR" } ] }
```

The `ref` you give on a link comes back to you as `merchant_ref`. Errors use the `E9xxx` family. See [Error codes](#error-codes-lookup-api-errors).

<a id="variants-prefilled-links-creating-links-in-your-account-portal"></a>
#### Creating links in your account portal

Your account portal lets you create links one at a time, enter several by form, or upload a CSV or Excel list and map columns to your fields. You can search, filter by state (new, opened, completed, used up, expired, disabled), download the list with QR images, share a link, and show a clean full-screen QR to a customer. You can also set the QR colors, logo and layout.

---

<a id="branding-theming"></a>

## Branding and theming

Each journey has its own look. You customize colors and logo through your account portal, or supply the artwork during onboarding.

<a id="branding-theming-what-you-can-brand"></a>
### What you can brand

| Element | Notes |
|---------|-------|
| Theme | The set of colors and styles for your pages. You can have more than one theme assigned to a journey. |
| Logo and banner | Separate artwork for mobile and desktop. A journey can have several banners. |
| Page title, description and sharing image | Used when your offer URL is shared or indexed. |
| Wording | Text on the pages can be reworded per language. |
| Analytics | Optional analytics IDs for secondary tags. |

<a id="branding-theming-choosing-a-theme-or-banner-from-a-link"></a>
### Choosing a theme or banner from a link

| Parameter | Behaviour |
|-----------|-----------|
| `theme_id` | Selects one of your assigned themes. Ignored unless the theme allows URL override. |
| `banner_id` | Selects one of your **approved** banners. Ignored if unknown or unapproved. |

Without a parameter, your default theme and banner show. A [variant](#variants-prefilled-links) can also fix a theme or banner for a campaign, and lock it.

<a id="branding-theming-rules-to-know"></a>
### Rules to know

- **Banners need approval** before they can be selected by URL.
- **Colors must be readable.** Text and background pairs must meet a 4.5:1 contrast ratio before a theme is accepted. This is an automated accessibility check, not a conformance guarantee. Full accessibility validation needs manual testing with assistive technology and expert review.
- **Alt text** is needed for your artwork.
- **Wording changes** apply per app and per language together. You can reword a string in Sinhala and leave English as it is. Customer-facing text can only use the placeholders that string allows.
- Pages work on small screens. They are designed mobile first, down to 320 px wide.

<a id="branding-theming-what-to-send-during-onboarding"></a>
### What to send during onboarding

- Logo and banner artwork for mobile and desktop, with alt text.
- Your brand colors, or the theme you want to start from.
- Page title, description and sharing image.
- Any wording changes, per language.
- Analytics IDs, if you want them.

---

<a id="languages"></a>

## Languages

Three parties have a say in the page language, so the order is explicit.

| Priority | Source | Set by |
|----------|--------|--------|
| 1 | The customer picks from the language switcher on the page. | The customer, at any time. |
| 2 | Your `lang` tag on the entry URL, **or** the customer's saved preference for their number. | Your link, or a previous journey. |
| 3 | Your app's default language. | Onboarding. |
| 4 | English. | Platform default. |

<a id="languages-vendor-tag-or-saved-preference"></a>
### Vendor tag or saved preference

Priority 2 holds two competing sources. A per-app setting, **language priority**, decides which wins when both are present:

- **Your `lang` tag** (the default) suits campaign traffic. A Tamil-language creative should land on a Tamil page, even for a subscriber who once browsed in Sinhala.
- **The customer's saved preference** suits service portals, where the subscriber's own past choice matters more than the link they clicked.

Ask during onboarding which you want.

<a id="languages-rules"></a>
### Rules

- Only languages **enabled for your app** are accepted. An unknown or unassigned `lang` value is ignored and the next source applies, so a stale link degrades instead of erroring.
- Only an **explicit customer selection** is saved against the customer's number. Your `lang` tag is a campaign instruction, not a personal preference, so it never overwrites a saved choice.
- The number is not known at entry, so a saved preference can only be applied once detection or OTP resolves it. The language is then re-resolved, but **only** if the customer has not already chosen. We do not switch a page the customer just set.
- You receive the language used and which source won in the notification (`session.language`, `session.language_source`).

<a id="languages-translating-your-content"></a>
### Translating your content

Labels, help text and placeholders on your custom fields, and wording on the pages, can be translated per language. If a translation is empty, the text you typed is used. See [Custom fields](#custom-fields) and [Branding and theming](#branding-theming).

<a id="languages-what-to-send-during-onboarding"></a>
### What to send during onboarding

- The languages to enable, and your default.
- Language priority: your `lang` tag or the customer's remembered choice.
- Translations for your fields, consent text and any reworded page text.

---

<a id="subscription-payment"></a>

## Subscription and payment options

A journey can end in a consent record only, a new subscription, or a one-time charge. You agree the combination during onboarding. This page describes the choices.

<a id="subscription-payment-the-action"></a>
### The action

| Action | What happens | `action_status` you receive |
|--------|--------------|-----------------------------|
| Consent only | The customer's consent is recorded. | `CONSENT_RECORDED` |
| Subscription | The customer subscribes to a pack. | `SUBSCRIBED`, or `ALREADY_ACTIVE` |
| One-time charge | The customer pays an amount. | `CHARGED` |

An already-active subscriber is not an error. They get `ALREADY_ACTIVE` (code `S1001`), and nothing is billed again.

<a id="subscription-payment-subscription-packs"></a>
### Subscription packs

- A subscription journey has one **default pack** and, optionally, additional packs.
- A pack has a code, a name, a price and a billing period (for example daily, weekly or monthly).
- Optional packs can be ticked by the customer on the subscription page.
- To preselect packs from a link, use `?pack=SPORTS,MUSIC`, or a [variant](#variants-prefilled-links). Only packs marked as selectable by URL are applied. An unknown or non-selectable pack gives `E5002`.
- Packs you can offer on top of a subscription are described in [Add-ons and packs](#add-ons-packs).

<a id="subscription-payment-one-time-charge-amounts"></a>
### One-time charge amounts

- The amount comes from your configured default, from a [variant](#variants-prefilled-links), or (if you enable it) from your `amount` parameter.
- Your app has a minimum and a maximum. An amount outside them is refused or limited. This is enforced on our servers, so a tampered URL cannot raise the charge.
- `amount` is ignored if vendor-supplied amounts are not enabled for your app.

<a id="subscription-payment-payment-methods"></a>
### Payment methods

| Method | How it works |
|--------|--------------|
| Operator billing (DCB) | Charged to the customer's mobile account. Needs mobile data. |
| Card or bank payment | The customer is redirected to the payment provider and returned to the journey. |
| LankaQR | The customer scans a dynamic QR code with their banking app. |

Which methods are offered depends on the device and on what is enabled for your app.

| Situation | Methods offered |
|-----------|-----------------|
| Mobile data, only operator billing enabled | Operator billing, with no chooser shown. |
| Mobile data, operator billing and card enabled | A chooser: operator billing, or card / bank. |
| Not on mobile data | Card / bank and QR only. Operator billing needs the operator connection. |
| Recurring subscription paid by card | Card tokenisation is required. |
| Recurring subscription by operator billing | The operator handles renewal. |

We confirm every payment outcome with the provider on the server. The customer's browser return is never trusted on its own, and provider callbacks are handled safely if repeated or if the customer closes the tab.

<a id="subscription-payment-saved-cards"></a>
### Saved cards

A saved card is offered for one-tap reuse only when **both** hold:

- the current session is OTP-verified, and
- the card was itself stored during an OTP-verified journey.

Being verified now is not enough. This prevents a card added by someone who merely typed a number from appearing for the real subscriber later. Saved cards also require OTP to be enabled for your app.

<a id="subscription-payment-otp-options"></a>
### OTP options

Your app can detect the number and verify it with OTP, always ask for the number plus OTP, or skip OTP. You can also decide whether a customer may change the number. OTP has limits on attempts, resends and code lifetime. When the attempts are exhausted the journey ends (`E3004`), and the customer starts again.

<a id="subscription-payment-security-checks"></a>
### Security checks

You can attach a fraud-prevention vendor and a captcha to your journey. For checks that cannot be confirmed, you choose whether to continue (the journey completes with `S1003`) or to block. A clear failure from the vendor always blocks. See [Error codes](#error-codes-how-your-error-policy-applies).

<a id="subscription-payment-what-to-send-during-onboarding"></a>
### What to send during onboarding

- Which action: consent only, subscription or charge.
- Packs, prices and billing periods, and which pack is the default.
- Charge amount, minimum and maximum, and whether you will supply `amount`.
- Payment methods you want enabled.
- OTP mode, and your security vendor and captcha choices.

---

<a id="add-ons-packs"></a>

## Add-ons and packs

Two ways to extend a subscription offer: **add-ons** the customer can tick, and **pack codes** that preselect a subscription, add-ons, or both.

<a id="add-ons-packs-add-ons"></a>
### Add-ons

An add-on is an extra the customer can take with a subscription.

- Add-ons are available to apps registered on **IdeaBiz**. Ideamart apps do not have add-ons.
- Each add-on is **its own IdeaBiz subscription**. It has its own IdeaBiz service id, a price, and is either **recurring** (daily, weekly or monthly) or **one-time**.
- An add-on is offered with **every subscription** of your app, or **only with selected ones**.
- On the subscription page, the customer ticks add-ons under the subscription they chose. An add-on box hides when the customer picks a subscription it does not go with. Anything that does not apply, or does not exist, is dropped on our side.

!!! warning "Add-ons are recorded only"
    The customer's choice is saved with the journey and reported to you. **Nothing is subscribed or charged for an add-on yet.** Use the `addons` list in the [native notification](#notifications-native-notification-format) to fulfil them yourself. Linking add-ons to the subscription and payment step is planned for later.

<a id="add-ons-packs-packs-and-pack-codes"></a>
### Packs and pack codes

A **pack** is a code that selects a subscription, add-ons, or both.

!!! note
    Not to be confused with a *subscription pack*, which is the thing the customer subscribes to and what the `?pack=` parameter selects. Here, a **subscription** is that, and a **pack** is the code that points at one.

| Setting | Notes |
|---------|-------|
| Code | 2 to 32 letters or digits (also `_` and `-`), upper-case, unique within your app. |
| Subscription | One of your subscriptions, or "keep the customer's choice". |
| Add-ons | Any of your add-ons. |
| Expiry | Optional date. The code stops working at the end of that day. |
| Usable count | Optional. The number of **completed** journeys that used the code. |

<a id="add-ons-packs-how-a-pack-reaches-a-journey"></a>
#### How a pack reaches a journey

1. **The customer types it.** With **Customers can enter a pack code** switched on for your app, the subscription page shows "Have a pack code?". A good code selects its subscription and add-ons. A wrong, expired or used-up code shows an error. A journey gets 10 tries per hour.
2. **A pre-filled link carries it.** Use `offer.bundleCode` when you [create the link](#variants-prefilled-links-creating-links-from-your-own-system). It is applied once, only while the journey has no choice of its own, with the same checks.
3. **A link preselects directly.** A link can also preselect a subscription or add-ons without a pack code.

A pack's subscription is honoured even when the customer is not allowed to change subscription. If the customer then picks a different subscription, the code is dropped. The customer can always change what was preselected.

<a id="add-ons-packs-what-you-receive"></a>
### What you receive

In the native notification:

```json
"addons": [ { "code": "MUSIC", "name": "Music", "kind": "RECURRING", "billing_period": "MONTHLY",
              "price": 50.0, "currency": "LKR", "ideabiz_service_id": "h1" } ],
"pack_code": "JUNE50"
```

The reference on a pre-filled link comes back as `merchant_ref`.

<a id="add-ons-packs-set-up"></a>
### Set up

- Create your add-ons and packs in your account portal (or supply them during onboarding), with the IdeaBiz subscription id for each add-on and which subscriptions they go with.
- Decide whether customers may enter pack codes themselves.

---

<a id="sign-in-providers"></a>

## Sign-in providers

An optional step lets a customer identify themselves with an account, after their mobile number. You then receive the customer's **name and e-mail** with the result, and your app keeps a customer record.

<a id="sign-in-providers-what-the-customer-sees"></a>
### What the customer sees

- A **Sign in** page after the number steps, with one button per provider your app offers: "Continue with Google", "Continue with Microsoft", or a third-party provider with its own name and icon.
- It is always **optional**. "Continue without signing in" skips it. The step only exists for apps with at least one approved provider.
- **A name is always required.** If the provider returns none, the customer is asked to type it.
- You can skip the choice by adding `?sso=google`, `?sso=microsoft` or a third-party provider's code (for example `?sso=x12`) to the entry URL. The customer goes straight to that provider, once. If they cancel, the normal choice is shown.

<a id="sign-in-providers-providers"></a>
### Providers

| Provider | Kind | Notes |
|----------|------|-------|
| Google | OpenID Connect | Shows the account chooser. |
| Microsoft | OpenID Connect | Work, school, personal, a specific tenant, or a domain. A fixed tenant is enforced. |
| Third-party | OpenID Connect or plain OAuth 2.0 | Your own endpoints, scopes and field names. |

Each client can be **shared** (set up once by the platform administrator for any app to use) or **your own** (your own client id and secret for your app).

**Every association needs approval.** Your choice is pending until approved. Customers only see approved, enabled providers. Changing your own client's id, secret, tenant, endpoints or field names sends it back to pending, while switching a provider on or off does not. A rejection comes with a reason. Approval is checked again when the customer returns from the provider, so withdrawing it takes effect immediately.

Client secrets are stored encrypted and are never shown again.

<a id="sign-in-providers-third-party-providers"></a>
#### Third-party providers

You provide:

- A **button name** ("Continue with ...") and an **icon**.
- The **type**: OpenID Connect (the issuer is required) or plain OAuth 2.0 (the profile is read from a userinfo URL).
- The authorize, token and (optional for OIDC) userinfo URLs, scopes, and whether PKCE is used.
- The **claim names** for the account id, e-mail and name. A dotted path reaches inside nested replies, such as `data.user.email`.

Its code is `X` plus an id, for example `X12`. One app can offer several.

All URLs must be plain `https` addresses on the standard port, without a user name, and must resolve to **public** addresses. Internal and private addresses are refused.

<a id="sign-in-providers-redirect-uri"></a>
### Redirect URI

Register this **authorised redirect URI** with every OAuth client:

```
https://consent.ideamart.io/v2/sso/callback
```

<a id="sign-in-providers-how-a-sign-in-is-made-safe"></a>
### How a sign-in is made safe

- The customer signs in on the provider's own page. We never see a password.
- Each attempt uses a one-time state, a nonce and PKCE, and is tied to the customer's browser. A sign-in link sent to someone else cannot attach their account to your customer's journey.
- The provider's token is checked for audience, nonce, expiry and issuer.
- An e-mail is treated as verified only when the provider says so.
- Starting is limited to 20 attempts per journey per hour.

If a sign-in fails, the customer returns to the sign-in page with "Sign-in didn't complete. You can try again or continue without signing in." The reason is not shown. A stale link shows "This sign-in link has expired".

<a id="sign-in-providers-customers"></a>
### Customers

A **customer** is a person known to **one app**. Apps never see each other's customers. A customer record holds the verified mobile number (hashed and masked), e-mail and whether it is verified, name, and the account ids of the providers they signed in with.

On sign-in, we find the customer by account id first, then by the journey's verified number, then by a verified e-mail, and otherwise create a new one. The same person can arrive by number one day and by Google the next and remain one customer.

<a id="sign-in-providers-what-you-receive"></a>
### What you receive

The native notification gains a `customer` object, present only when the customer signed in:

```json
"customer": { "id": 41, "name": "Nimal Perera", "email": "nimal@example.com",
              "email_verified": true, "provider": "google" }
```

`provider` is `google`, `microsoft` or the third-party provider's code. The TMF format does not carry `customer` yet.

<a id="sign-in-providers-set-up"></a>
### Set up

1. Choose the providers, and for each decide between a shared client and your own.
2. If you use your own, register the redirect URI above in the provider's console, and supply the client id and secret.
3. Request approval.
4. Optionally add `?sso=...` to your entry URLs.

---

<a id="shared-core-journeys"></a>

## Shared core app and multiple journeys

One vendor account can run **several branded consent journeys** that share one underlying registration. You register once, keep a single `client_id`, and publish as many journeys as you need.

<a id="shared-core-journeys-what-this-gives-you"></a>
### What this gives you

Each journey has its own:

- **Slug** and public URL: `https://consent.ideamart.io/v2/offer/{slug}`
- **Look**: theme, banner, logo and page wording
- **Consent configuration**: consent text, fields, forms, subscription or charge, and payment options

All journeys share:

- Your registered app and `client_id`
- Your registered redirect URLs
- Your lookup API access. Results from every journey are scoped to your application.

Typical uses are separate campaigns, separate brands, or separate services under one company.

<a id="shared-core-journeys-reaching-each-journey"></a>
### Reaching each journey

| Entry | Goes to |
|-------|---------|
| `https://consent.ideamart.io/v2/offer/{slug}` | That journey. Use this for every journey. |
| `https://consent.ideamart.io/v2/offer/{slug}/{variant}` | That journey, with the variant's choices. |
| `/v2/authorize?client_id=…&variant=…` | Your OAuth2 client's journey. See [OAuth2 integration](#oauth2). |
| `/web/v1/authorize?client_id=…` | **The first journey set up** for the `client_id`. |

<a id="shared-core-journeys-the-automatic-msisdn-detect-handoff"></a>
### The automatic MSISDN Detect handoff

If you already integrate MSISDN Detect and Consent v2 is switched on for your app, your existing `/web/v1/authorize?client_id=…` link redirects into a Consent v2 journey automatically.

With **more than one** journey on a `client_id`, that automatic handoff cannot know which one you mean. It goes to whichever journey was set up **first**. Every other journey is reached only by its own `/v2/offer/{slug}` link.

If it matters to you which journey should receive the automatic handoff, say so during onboarding.

<a id="shared-core-journeys-telling-journeys-apart-in-results"></a>
### Telling journeys apart in results

- The `slug` field in the [native notification](#notifications-native-notification-format) names the journey.
- Use your own `ref` value to group journeys by campaign. It is returned unchanged on the redirect, in the notification, and in the [lookup API](#lookup-api).
- Lookup results are scoped to your application, so you can look up a journey with your reference whichever of your slugs it ran on.

<a id="shared-core-journeys-setting-it-up"></a>
### Setting it up

1. Register your app once and get your `client_id`.
2. For each journey, agree a slug during onboarding, and decide its look, fields, consent and action.
3. Say which journey, if any, should own the automatic `/web/v1/authorize` handoff.
4. Test each slug end to end, on mobile data and on desktop.

See the [onboarding checklist](#onboarding-checklist).

