# IdeaBiz MSISDN Detect — Integration Guide

## Contents

- [Overview](#msisdn-detect-overview)
- [How detection works](#how-detection-works)
- [Entry point and request parameters](#entry-point-and-request-parameters)
- [Response and MSISDN formats (ATEL/STEL/ETEL)](#response-and-msisdn-formats-atelsteletel)
- [Notifications and callbacks](#notifications-and-callbacks)
- [Error codes](#error-codes)
- [Number format choice](#number-format-choice)
- [Detection methods](#detection-methods)
- [Referral URL allowlisting](#referral-url-allowlisting)
- [Other configurable behavior](#other-configurable-behavior)

---

## MSISDN Detect — Overview

MSISDN Detect identifies a mobile subscriber's phone number (MSISDN) when they open your web page over their mobile data connection. The customer does not type anything, receive an SMS or approve a prompt. You send them to one URL, we detect the number, and we return them to your site with a reference you can use to fetch the result.

This section is for developers and vendors integrating with the service. It covers how to call it, what comes back, and what you can customize for your application.

### The integration in two steps

1. **Redirect** the customer's browser to the authorize URL. We detect the number and redirect back to your site with a reference.
2. **Look up** the result from your backend, using that reference, through the IdeaBiz API Gateway.

```
your page
   │  redirect
   ▼
https://consent.ideamart.io/web/v1/authorize?client_id=…&ref=…
   │
   │  we handle: session, detection, fallbacks
   │
   ▼
your callback?ref=…&unique_ref=HE-…&status=DETECTED|NOT_DETECTED
   │
   ├── we POST a notification to your endpoint   ← push (optional)
   └── your backend calls the status API         ← pull (authoritative)
```

Three things to hold onto:

1. **The redirect is a hint.** It tells you the outcome, but it travels through the browser. Do not deliver value based on it alone.
2. **The status API is the truth.** Confirm server-side with the `unique_ref` before you act on a number.
3. **You never receive the raw number by default.** The value you get back is a token specific to your application (see [Response and MSISDN formats](#response-and-msisdn-formats-atelsteletel)).

### What you need before you start

| Item | Where it comes from |
|------|---------------------|
| `client_id` | Issued to you during onboarding. Identifies your app on the web endpoints. |
| An application on IdeaBiz API Manager | Used to obtain the Bearer access token for the status API. |
| One or more redirect (callback) URLs | You provide these during onboarding. We only redirect to registered URLs. |
| Referral domain(s) | Optional. See [Referral URL allowlisting](#referral-url-allowlisting). |
| Number format | Chosen during onboarding. See [Number format choice](#number-format-choice). |

### Good to know

- Detection by network header works on **mobile data only**. A customer on WiFi is usually reported as `NOT_DETECTED` unless a return-visit method is enabled for your app. See [Detection methods](#detection-methods).
- The whole redirect chain normally completes in well under a second. The customer sees your page load, nothing in between.
- The `ref` you send is for your own tracking. We return it unchanged.
- Always handle both `DETECTED` and `NOT_DETECTED`. The redirect happens either way.

### Public domains

| Domain | Used for |
|--------|----------|
| `consent.ideamart.io` | Web entry point (HTTPS). This is the only URL you send customers to. |
| `msisdndetect.ideamart.io` | Part of the detection hop. The customer's browser is redirected through it automatically; you do not link to it. |
| `ideabiz.lk/apicall` | IdeaBiz API Gateway. Your backend calls the status API here. |

---

## How detection works

### Header enrichment in one paragraph

When a customer browses over mobile data, the mobile network adds their subscriber number to the HTTP request as an encrypted header. We read that header, decrypt it on our side and tie the result to the session you started. Your application never sees the header, and never sees the raw number unless your agreement says otherwise.

### The flow

```
Your page
   │ 1. redirect the customer's browser
   ▼
/web/v1/authorize?client_id=…&ref=…
   │ 2. we validate your app and start a session
   ▼
detection hop (network adds the header; we read it)
   │ 3. detection result recorded
   ▼
finish
   │ 4. redirect back to your registered callback URL
   ▼
your callback?ref=…&unique_ref=HE-…&status=…
   │ 5. your backend calls the status API with unique_ref
   ▼
JSON result: tokenized number, masked number, format
```

| Step | What happens | What you do |
|------|--------------|-------------|
| 1 | Customer is sent to the authorize URL. | Redirect their browser. Send your `client_id` and a `ref`. |
| 2 | We check the `client_id` and, if configured, the referring site. A session is created. | Nothing. |
| 3 | The customer is passed through the detection hop. Depending on your configuration, we may also try fallback methods (see below). | Nothing. |
| 4 | The customer is redirected to your callback URL with `ref`, `unique_ref` and `status`. | Read `status` and keep `unique_ref`. |
| 5 | Your server requests the result. | Call the status API with the `unique_ref`. |

For the default configuration, the customer sees no page of ours at any point. Every step is an HTTP redirect.

### Outcomes

| `status` on the redirect | Meaning |
|--------------------------|---------|
| `DETECTED` | A number was found. Fetch it with the status API. |
| `NOT_DETECTED` | No number could be found, for example the customer is on WiFi or the network did not add the header. |
| `INVALID_STATE` | The link was opened out of order or replayed. See [Error codes](#error-codes). |

### Fallbacks for customers not on mobile data

Network header detection only works on mobile data. Your application can be configured with additional methods so that returning customers on WiFi, or customers whose first attempt did not carry a header, can still be recognized. These add a short page to the flow (a brief "Detecting…" screen you can brand). The options are described in [Detection methods](#detection-methods).

### Link previews and crawlers

If a non-browser agent opens your authorize URL, for example a chat app generating a link preview or a web crawler, the redirect chain is meaningless for it. By default these requests receive a simple preview page instead of entering the flow. See [Other configurable behavior](#other-configurable-behavior).

### Single-use references

The `unique_ref` is meant for a one-time lookup of one detection. Treat it as short-lived: look it up as soon as the callback arrives, and do not expose it in client-side code or logs shared outside your team.

### If your app is also set up for Consent v2

Applications that also use the Consent v2 journeys are handed on to that journey after detection, and the result is delivered at the end of that journey instead. If this applies to you, your account manager will have told you. The Consent v2 section of this site describes what you receive in that case.

---

## Entry point and request parameters

### Authorize URL

Redirect the customer's browser (a normal top-level navigation, not an XHR or server-side fetch) to:

```
GET https://consent.ideamart.io/web/v1/authorize?client_id={clientId}&ref={campaignRef}
```

No access token is needed on this endpoint. Your app is identified by `client_id`.

!!! note "Use the customer's own browser"
    Detection depends on the customer's mobile connection. Calling the authorize URL from your server will not work. It must be opened by the customer's device.

### Request parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `client_id` | Yes | The identifier issued to you at onboarding. |
| `ref` | Yes | Your own campaign or session reference. Returned unchanged on the redirect back to you. |
| `protocol` | No | `http` or `https`. Overrides the protocol of the detection hop. Only honored if your app is set up to allow it; otherwise ignored. See [Other configurable behavior](#other-configurable-behavior). |
| `service_code` | No | A code identifying the service on your platform. Maximum 64 characters. Returned on the redirect, the result and the notification. |
| `service_name` | No | A name for the service on your platform. Maximum 64 characters. Returned like `service_code`. |
| `p_<key>` | No | Custom parameters. Up to **5** query parameters whose names start with `p_`, for example `p_region=LK&p_tier=gold`. Each value is limited to 100 characters; longer values are truncated. Returned on the redirect (as `p_<key>`), the result and the notification. |
| `security_vendor` | No | Name of an anti-fraud provider, for example `evina`. Only for apps onboarded with security validation. See below. |
| `security_ref` | Conditional | The reference issued by that provider. Required if your app is set up to require security validation. |

Any other parameter is ignored, unless your app has **parameter passthrough** enabled, in which case all additional parameters are appended to the final redirect to your callback URL.

!!! tip "Encode your values"
    URL-encode `ref` and every other value you send.

### Example

```
https://consent.ideamart.io/web/v1/authorize
  ?client_id=a1b2c3d4-e5f6-7890-abcd-ef1234567890
  &ref=order-48213
  &service_code=premium-news
  &p_region=LK
```

### What comes back

Whatever the outcome, the customer is redirected to the callback URL registered for your app:

```
https://yoursite.com/callback?ref={campaignRef}&unique_ref={uniqueRef}&status={status}
```

| Parameter | Description |
|-----------|-------------|
| `ref` | The `ref` you sent. |
| `unique_ref` | Our transaction reference, in the form `HE-` followed by a UUID. Use it to call the status API. |
| `status` | `DETECTED`, `NOT_DETECTED`, or `INVALID_STATE`. |
| `code` | Only present when security validation is enabled for your app. See below. |
| `service_code`, `service_name` | Echoed if you sent them. |
| `p_<key>` | Echoed for each custom parameter you sent. |

!!! warning "The redirect always happens"
    Even when nothing is detected, the customer is sent back to you. Always check `status`. Do not assume that arriving at your callback means success.

If your app was configured for API-only testing, the result is returned as a JSON page instead of a redirect. This is arranged during onboarding; contact your account manager if you want a test setup.

### Fetching the result (status API)

From your backend, call the IdeaBiz API Gateway with the `unique_ref`. Two formats are published; use whichever suits your stack.

```
# IdeaBiz format
GET https://ideabiz.lk/apicall/msisdndetect/v1/api/ideabiz/v1/status?ref={unique_ref}

# TMF681 format
GET https://ideabiz.lk/apicall/msisdndetect/v1/api/tmf/v1/status?ref={unique_ref}
```

| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | Yes | `Bearer {access_token}`, obtained for your application from IdeaBiz API Manager. |

Your token identifies your application, and you can only look up transactions that belong to it. Response bodies are described in [Response and MSISDN formats](#response-and-msisdn-formats-atelsteletel).

### Security validation (optional)

If your app is onboarded with an anti-fraud provider, send `security_vendor` and `security_ref` on the authorize URL. We validate the reference with the provider as the flow finishes.

| Provider outcome | Result you receive |
|------------------|--------------------|
| Valid | Normal result. |
| Invalid | Always `NOT_DETECTED` with `code=S1002`. |
| Provider unavailable | Depends on your app setting: the real result with `code=S1003`, or `NOT_DETECTED` with `code=E1005`. |

A missing `security_ref` on an app that requires one is treated as invalid (`S1002`).

### Code examples

#### Node.js

```javascript
const CLIENT_ID = 'a1b2c3d4-e5f6-7890-abcd-ef1234567890';

// Step 1: send the customer to the authorize URL
app.get('/detect', (req, res) => {
  const ref = encodeURIComponent(req.sessionID);
  res.redirect(`https://consent.ideamart.io/web/v1/authorize?client_id=${CLIENT_ID}&ref=${ref}`);
});

// Step 2: handle the callback, then confirm server-side
app.get('/callback', async (req, res) => {
  const { status, unique_ref } = req.query;
  if (status !== 'DETECTED') return res.redirect('/fallback-login');

  const resp = await fetch(
    `https://ideabiz.lk/apicall/msisdndetect/v1/api/ideabiz/v1/status?ref=${encodeURIComponent(unique_ref)}`,
    { headers: { Authorization: `Bearer ${accessToken}` } }
  );
  const data = await resp.json();
  if (data.statusCode === 'S1000') {
    // data.msisdn.value is your tokenized identifier for this customer
  }
});
```

#### PHP

```php
// Step 1: redirect
$clientId = 'a1b2c3d4-e5f6-7890-abcd-ef1234567890';
header('Location: https://consent.ideamart.io/web/v1/authorize?client_id=' . $clientId
     . '&ref=' . urlencode(session_id()));
exit;

// Step 2: callback
$status = $_GET['status'] ?? '';
$uniqueRef = $_GET['unique_ref'] ?? '';
if ($status === 'DETECTED') {
    $ch = curl_init('https://ideabiz.lk/apicall/msisdndetect/v1/api/ideabiz/v1/status?ref=' . urlencode($uniqueRef));
    curl_setopt($ch, CURLOPT_HTTPHEADER, ['Authorization: Bearer ' . $accessToken]);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    $response = json_decode(curl_exec($ch), true);
    curl_close($ch);
    if (($response['statusCode'] ?? '') === 'S1000') {
        $value = $response['msisdn']['value'];
    }
}
```

### Onboarding checklist

- [ ] Receive your `client_id`.
- [ ] Register your application on IdeaBiz API Manager and confirm how to obtain access tokens.
- [ ] Provide your callback URL(s).
- [ ] Provide your referral domain(s), if you want referrer checking.
- [ ] Choose your number format.
- [ ] Choose your API format (IdeaBiz or TMF681).
- [ ] Decide which detection methods you need.
- [ ] Handle both `DETECTED` and `NOT_DETECTED` on your callback.
- [ ] Call the status API server-side before acting on a number.

---

## Response and MSISDN formats (ATEL/STEL/ETEL)

### Status API responses

Call the status API with the `unique_ref` from your callback (see [Entry point and request parameters](#entry-point-and-request-parameters)). Two response formats are published. Both carry the same information.

#### IdeaBiz format

Detected:

```json
{
  "status": "SUCCESS",
  "statusCode": "S1000",
  "msisdn": {
    "value": "IB-A-000A-c1068ff9-4472-4153-8a8a-44b5deda814b",
    "mask": "9477*****61",
    "connectionType": "UNKNOWN",
    "platform": "IB",
    "format": "ATEL",
    "detectionMethod": "HE",
    "sessionId": "d66328ee-2d45-4801-90bf-f2d30979c31d",
    "identityRef": "e3b0c442-98fc-1c14-9afb-4c8996fb9242"
  }
}
```

Not detected:

```json
{
  "status": "SUCCESS",
  "statusCode": "S1001",
  "message": "MSISDN not detected"
}
```

| Field | Description |
|-------|-------------|
| `statusCode` | `S1000` detected, `S1001` not detected. Security-validation codes (`S1002`, `S1003`, `E1005`) are covered in [Error codes](#error-codes). |
| `msisdn.value` | The tokenized number, in the format chosen for your app. Store and compare this value. |
| `msisdn.mask` | A masked form of the number, safe to show to the customer, for example `9477*****61`. |
| `msisdn.format` | The format of `value`: `ATEL`, `STEL` or `ETEL`. |
| `msisdn.detectionMethod` | How the number was found: `HE` (network header), `AAA` or `AAA-C6` (network-based resolution), or `BROWSER` (recognized from an earlier detection on the same browser). |
| `msisdn.connectionType` | Reserved for prepaid or postpaid. Currently always `UNKNOWN`. Do not depend on it. |
| `msisdn.platform` | Platform identifier. Informational. |
| `msisdn.sessionId`, `msisdn.identityRef` | Opaque identifiers for the detection. Quote them when you contact support. |

The response may also include the subscriber's mobile operator name. Treat any field not listed above as optional and ignore fields you do not recognize.

#### TMF681 format

Detected:

```json
{
  "id": "HE-a1b2c3d4-12345",
  "href": "/tmf/v1/status?ref=HE-a1b2c3d4-12345",
  "@type": "IndividualIdentification",
  "identificationId": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "lifecycleStatus": "Completed",
  "validFor": { "startDateTime": "2025-01-15T10:30:45Z" },
  "verificationResult": true,
  "identifiedPerson": {
    "@type": "Individual",
    "contactMedium": [{
      "mediumType": "mobile",
      "preferred": true,
      "characteristic": {
        "phoneNumber": "IB-A-000A-c1068ff9-4472-4153-8a8a-44b5deda814b",
        "phoneNumberMask": "9477*****61",
        "phoneNumberFormat": "ATEL"
      }
    }]
  }
}
```

Not detected: the same envelope with `"verificationResult": false` and no `identifiedPerson`.

| TMF field | Equivalent IdeaBiz field |
|-----------|--------------------------|
| `verificationResult` | `statusCode` `S1000` (`true`) or `S1001` (`false`) |
| `phoneNumber` | `msisdn.value` |
| `phoneNumberMask` | `msisdn.mask` |
| `phoneNumberFormat` | `msisdn.format` |

### MSISDN formats

Your application receives the number in **one** format, chosen at onboarding. The service does not hand out the plain number by default, and tokens are different for every application, so numbers cannot be matched across vendors.

| Format | Example | What it is | What it means for you |
|--------|---------|-----------|-----------------------|
| `ATEL` | `IB-A-000A-c1068ff9-…` | Application-level token. | The same customer always produces the same token **for your application**. Different applications get different tokens for the same customer. This is the recommended choice. |
| `STEL` | `IB-S-0064-d2f8a1b3-…` | Service-level token. | Like ATEL but scoped to a single service, so it is isolated even between services of the same vendor. Use it if you run several services and do not want them to be linkable. |
| `ETEL` | `9477-eG4dpvKt46HB` | Encrypted value. | Not a stable identifier you can read. It can only be reversed by the platform. Use it when you will pass the value back to the platform in a later call. |

#### Working with tokens

- Treat the value as **opaque**. Do not parse it, truncate it or infer the operator or number from it.
- Use the same value as a stable customer key within your application. For ATEL and STEL, equal tokens mean the same customer.
- Do not compare tokens across different applications or services. They will not match, by design.
- Show the customer the `mask` value if you need to confirm "your number is 9477*****61".
- Store tokens with the same care as any customer identifier.

To change format, see [Number format choice](#number-format-choice).

### Optional extra data

The platform has settings that can add precheck or subscription status to the response, or withhold the number unless a precheck or subscription check passes. Availability for your application, and the exact response fields, must be confirmed with your account manager. Do not build against these until they are confirmed for your app.

---

## Notifications and callbacks

You can learn the outcome of a detection through three channels. Use more than one.

| Channel | Direction | Reliability | Use it for |
|---------|-----------|-------------|-----------|
| Redirect to your callback URL | Browser to you | Hint only | Continuing the customer's journey. |
| Server notification | Us to your server | Best effort | Learning the outcome even if the customer never returns. |
| Status API | Your server to us | Authoritative | Reading the number and confirming the outcome. |

### Redirect callback

See [Entry point and request parameters](#what-comes-back) for the parameters. The redirect can be lost if the customer closes the tab, so do not depend on it alone.

### Server notification

If you register a notification URL, we send a `POST` with a JSON body when detection completes.

```
POST https://yoursite.com/api/notify
Content-Type: application/json
```

```json
{
  "unique_ref": "HE-57785731-51017",
  "campaign_ref": "campaign123",
  "status": "DETECTED",
  "session_id": "8a395ff9-c181-4c7a-a786-557ab74dd846",
  "timestamp": "2026-06-11T12:00:00Z"
}
```

| Field | Description |
|-------|-------------|
| `unique_ref` | The reference to use with the status API. |
| `campaign_ref` | Your campaign reference. This corresponds to the `ref` you sent on the authorize URL. |
| `status` | `DETECTED` or `NOT_DETECTED`. |
| `session_id` | Identifier of the detection session. |
| `timestamp` | When the notification was created (UTC). |
| `service_code`, `service_name`, `params` | Included when you sent `service_code`, `service_name` or `p_<key>` parameters. |

The notification **does not contain the number**. Call the status API with `unique_ref` to read it.

#### Delivery behavior

- Delivery is attempted once, with a short timeout of about 2 seconds, and the response body is not read.
- A retry schedule is not documented. Assume a notification can be missed.
- Your endpoint should answer quickly with any `2xx` and do real work asynchronously.
- Make the handler idempotent. Key on `unique_ref`.
- Use HTTPS for your endpoint.

#### Securing your endpoint

The notification format described here has no documented signature or authentication header. Treat the body as untrusted: **never act on the notification alone**. Use `unique_ref` to call the status API, and only trust that result. If you need to restrict inbound traffic to our servers, ask your account manager for the source addresses to allow.

### Result lookup by campaign reference

The IdeaBiz API also offers a lookup by your own `ref`, which returns the matching transactions. Use this when you have lost the `unique_ref`:

```
GET https://ideabiz.lk/apicall/msisdndetect/v1/api/ideabiz/v1/lookup?ref={your_ref}
```

It uses the same `Authorization: Bearer` header as the status API. A `404` with `E4040` means no transaction matched.

### When Consent v2 is in the journey

If your application is also configured for the Consent v2 journey, the detection result is handed on to that journey. In that case, the final redirect and notification come from Consent v2 at the end of the journey, not from MSISDN Detect. See the Consent v2 section for the formats.

---

## Error codes

Errors appear in three places: the browser (while the customer is in the flow), the redirect to your callback, and the status API.

### Browser errors on the authorize URL

| HTTP | Cause | What the customer sees | What to do |
|------|-------|------------------------|------------|
| 400 | `client_id` is missing | "The client_id parameter is required." | Add `client_id` to the URL. |
| 403 | `client_id` is not valid, not registered or inactive | "The provided client_id is not valid or not registered." | Check the value. Contact your account manager if the app should be active. |
| 500 | Internal error | "Unable to continue the detection flow." | Retry. If it persists, report it with the time and your `ref`. |

If your app has a referral allowlist, a request from an unlisted site is also rejected. See [Referral URL allowlisting](#referral-url-allowlisting).

### Errors later in the flow

| HTTP | Cause | Customer sees |
|------|-------|---------------|
| 400 | The link is not valid (unknown or expired session). | "This link is not valid." |
| 400 | The link was already used or opened out of order (apps set to show errors). | "This link has already been used or accessed out of order." |
| 403 | The session check failed, for example the browser or network changed mid-flow. | "Session validation failed." |
| 500 | Internal error. | "An internal error occurred." |

For apps set to silent handling (the default), an out-of-order or replayed link does not show an error. The customer is redirected to your callback with `status=INVALID_STATE`. Start a new authorize request to retry.

### Redirect `status` values

| `status` | Meaning |
|----------|---------|
| `DETECTED` | A number was found. |
| `NOT_DETECTED` | No number was found. |
| `INVALID_STATE` | Out-of-order access or replay (apps set to silent handling). |

### Redirect `code` values

Present only when security validation is enabled for your app.

| `code` | `status` | Meaning |
|--------|----------|---------|
| `S1000` | `DETECTED` | Detected, no security event. |
| `S1001` | `NOT_DETECTED` | Not detected, no security event. |
| `S1002` | `NOT_DETECTED` | The anti-fraud provider judged the request invalid. Always blocked. |
| `S1003` | `DETECTED` or `NOT_DETECTED` | The provider was unavailable. The real result is returned and flagged. |
| `E1005` | `NOT_DETECTED` | The provider was unavailable and your app is set to block. |

### Status API: IdeaBiz format

Success responses:

| `statusCode` | HTTP | Meaning |
|--------------|------|---------|
| `S1000` | 200 | Detected. `msisdn` is present. |
| `S1001` | 200 | Not detected. |
| `S1002` | 200 | Security validation failed. Treat as not detected. `securityFlag: true`. |
| `S1003` | 200 | Detected, but the security provider was unavailable. `securityFlag: true`. |
| `E1005` | 200 | Security validation unavailable and your app is set to block. `status` is `FAILED`. |

Error responses (`status` is `FAILED`):

| `statusCode` | HTTP | Meaning | What to do |
|--------------|------|---------|------------|
| `E4001` | 400 | `ref` parameter missing. | Send `?ref={unique_ref}`. |
| `E4010` | 401 | Bearer token missing. | Add the `Authorization` header. |
| `E4011` | 401 | Token invalid or expired. | Obtain a new token. |
| `E4030` | 403 | Your application is not registered for this service. | Contact your account manager. |
| `E4040` | 404 | No transaction found for that reference. | Check the reference. It may not belong to your app. |
| `E5002` | 500 | The number could not be converted to your format. | Retry once. If it persists, report it. |

Example:

```json
{
  "status": "FAILED",
  "statusCode": "E4040",
  "message": "transaction not found"
}
```

### Status API: TMF681 format

| Code | HTTP | Meaning |
|------|------|---------|
| `ERR-0001` | 400 | Required parameter missing (`ref`). |
| `ERR-0004` | 404 | Resource not found. |
| `ERR-0005` | 500 | Internal error. |

Security results in TMF appear as `verificationResult` plus a `code` and a `securityStatus` of `INVALID`, `ERROR_CONTINUE` or `ERROR_BLOCK`.

### Handling guidance

- Treat `S1001` and `NOT_DETECTED` as a normal outcome, not an error. Offer your fallback, such as OTP or login.
- Retry `5xx` with a short back-off. Do not retry `4xx` unchanged.
- When contacting support, include the `unique_ref`, the time (with timezone) and your `client_id`. Do not send tokens.

---

## Number format choice

The format of the number you receive is set per application. You choose it when you onboard, and it applies to every response, both status API formats, and any notification.

### Options

| Format | Stable per customer? | Scope | Typical use |
|--------|----------------------|-------|-------------|
| `ATEL` | Yes | Your application | Default recommendation. A durable customer key inside your app. |
| `STEL` | Yes | One service | Several services under one vendor that must not be linkable to each other. |
| `ETEL` | Not usable as a key | Platform only | You will hand the value back to the platform later and do not need to read it. |

Examples and semantics are in [Response and MSISDN formats](#response-and-msisdn-formats-atelsteletel).

### Choosing

- Need to recognize the same customer next time, inside one app? **ATEL**.
- Run multiple services and want strict isolation between them? **STEL**.
- Only passing the value through to a platform call? **ETEL**.

### The plain number

Some platform configuration includes a `PLAIN` option that returns the unmasked number. It is not part of the standard vendor offer, since the service is designed so vendors do not receive raw numbers. If your use case requires it, discuss it with your account manager. It is approved case by case, and this guide does not cover it further.

### Changing your format

You cannot change the format per request. To change it, contact your account manager.

!!! warning "Changing format changes your identifiers"
    Tokens in a different format are different values. Before switching, plan how you will map existing stored values. If you store ATEL tokens today and move to STEL, the same customer will have a new identifier.

### What the format does not affect

- The `mask` value is always returned in the same masked form.
- The redirect to your callback URL never contains the number in any format, only `unique_ref`.

---

## Detection methods

Your application is configured with an ordered list of detection methods. The order is the priority: we try the first, and move to the next only if a number was not found. You choose the combination at onboarding, and can ask your account manager to change it later.

### Methods

| Method | How the number is found | Works when | Notes |
|--------|------------------------|------------|-------|
| `HE` | The mobile network adds an encrypted subscriber header. | Customer is on mobile data. | The default. No page is shown. |
| `STRICT_HE` | Same as `HE`, with nothing else. | Customer is on mobile data. | Exclusive: cannot be combined with other methods. No intermediate page. |
| `COOKIE` | A previous successful detection is remembered in the customer's browser and recovered on the next visit. | Return visits, including on WiFi. | Needs the short "Detecting…" page. |
| `LOCALSTORAGE` | Same idea as `COOKIE`, using browser storage. | Return visits, including on WiFi. | Needs the short "Detecting…" page. |

Two further methods exist for network-based resolution, including for customers whose connection uses IPv6. They depend on operator-side infrastructure and are enabled per application on request. If HE alone gives you a low detection rate, ask your account manager whether they are available for your traffic.

### Common combinations

| Configuration | Behavior |
|---------------|----------|
| `HE` | Network header only. If it is not on mobile data, `NOT_DETECTED`. |
| `HE,COOKIE` | Network header, else recover a previous detection from the cookie. Pure redirects, no page shown. |
| `HE,COOKIE,LOCALSTORAGE` | As above, with browser storage as a further fallback. Shows the brief detecting page. |
| `STRICT_HE` | Network header only. Never shows a page. |

### How the browser methods behave

- **First visit on mobile data:** the number is found by header, and remembered for next time.
- **Later visit on WiFi:** the header is absent, so the remembered detection is used.
- **Security:** the remembered key is rotated every time it is used, so a copied key stops working after the next legitimate use. This is handled by the platform; you do nothing.
- Recognition is tied to the customer's device and browser. A different device or a cleared browser starts again from `HE`.
- The `msisdn.detectionMethod` in the status response shows which method succeeded (`BROWSER` for cookie or storage).

### Rules for the list

- Method names are case-insensitive. Duplicates are ignored.
- `STRICT_HE` cannot be combined with anything else.
- An empty list means `HE`.
- If an unrecognized name ever appears, it is ignored and the valid ones are used. If none are valid, `HE` is used.

### Choosing

| Goal | Suggested configuration |
|------|-------------------------|
| Fastest, no extra page; mobile data only | `HE` or `STRICT_HE` |
| Recognize returning customers on WiFi without showing a page | `HE,COOKIE` |
| Maximum coverage for returning customers | `HE,COOKIE,LOCALSTORAGE` |

If you enable `COOKIE` or `LOCALSTORAGE`, the "Detecting…" page appears in the flow. You can brand it. See [Other configurable behavior](#other-configurable-behavior).

!!! note "Privacy"
    Remembering a detection in the browser is your decision to enable. Consider whether your own privacy notice covers it.

---

## Referral URL allowlisting

You can restrict which websites are allowed to start a detection for your application. This stops another site from copying your `client_id` and using it to start detections that count against your app.

### How it works

At the authorize step, we compare the referring site of the customer's request against the list of domains registered for your app. If the request comes from a domain that is not on your list, it is rejected before a session is created.

| Setting | Behavior |
|---------|----------|
| No referral domains registered | No referrer check is applied. |
| One or more domains registered | Only requests referred from those domains are accepted. |

### What to give us

Provide a comma-separated list of the domains that host the page which sends customers to the authorize URL, for example:

```
vendor.com,app.vendor.com
```

Give us domains, not full URLs or paths. Whether subdomains are covered automatically is not defined in the material available for this guide, so list every host you use (for example both `vendor.com` and `www.vendor.com`) and confirm the matching rules with your account manager.

### Things to watch for

- The check uses the browser's referrer information. If your page sends a referrer policy that removes it (for example `no-referrer`), the request may be rejected once the allowlist is on. Use a policy that sends at least the origin, such as `strict-origin-when-cross-origin`, on the page that links to the authorize URL.
- If you redirect through an intermediate page or short-link service, the referrer is that intermediate site. Register it, or avoid the hop.
- Mobile apps opening the URL in an in-app browser may not send a referrer. Test this on real devices before you turn the allowlist on.
- The exact response a rejected request receives (page or redirect) is not specified in this guide. Contact your account manager if you need it for your error handling.

### Callback URLs are separate

The referral allowlist controls who may **start** a detection. Your callback (redirect) URLs control where customers are **sent afterwards**. Both are registered at onboarding. The result is returned to the callback URL registered for your app. It is not taken from the request.

### Changing the list

Contact your account manager to add or remove domains. Do this before launching a new site or campaign domain.

---

## Other configurable behavior

Besides [number format](#number-format-choice), [detection methods](#detection-methods) and the [referral allowlist](#referral-url-allowlisting), these settings are available per application. All of them are set by us during onboarding or on request. Contact your account manager to change any of them.

### Callback URLs

One or more URLs the customer is returned to after detection. Register every URL you will use, including test and staging hosts.

### Parameter passthrough

Off by default. When on, any extra query parameters on the authorize URL are appended to your callback URL. Without it, only the parameters listed in [Entry point and request parameters](#entry-point-and-request-parameters) are returned.

### Custom parameters and service identity

You can send `service_code`, `service_name` and up to five `p_<key>` parameters on any request, with no setup. They are returned on the redirect, the result and the notification. Use them to carry campaign, region or tier information through the flow without keeping server state.

### Server notification URL

An endpoint of yours that receives a server-to-server POST when detection completes. See [Notifications and callbacks](#notifications-and-callbacks).

### Detection page branding

When your detection methods include `COOKIE` or `LOCALSTORAGE`, a short "Detecting…" page is shown. You can customize it.

| Setting | What it changes |
|---------|-----------------|
| `title` | Browser tab title. |
| `loadingText` | Main heading, for example "Please wait…". |
| `subText` | Message under the heading. |
| `bgColor`, `textColor` | Page and text colors (any CSS color). |
| `spinnerColor`, `spinnerTrack` | Progress bar color and its track. |
| `fontFamily` | Font stack. |
| `loadingIcon` | URL of an image, GIF or SVG shown instead of the default animation. |
| `gaId` | A Google Analytics measurement ID to load on the page. |

Any setting you do not provide uses the platform default.

!!! note "Custom loading icon"
    The page only loads images from approved sources. Send the icon file to your account manager to be hosted for you, rather than linking to your own server. An icon from another host will not display.

Example of what you would send us:

```json
{
  "title": "Verifying",
  "loadingText": "Please wait…",
  "subText": "Checking your connection",
  "bgColor": "#0b1020",
  "textColor": "#ffffff",
  "spinnerColor": "#ff6600"
}
```

### Link previews and crawlers

When a non-browser agent opens your authorize URL (chat apps, social previews, crawlers, scripts), the detection chain cannot work for it. Your app is set to one of:

| Policy | What happens |
|--------|--------------|
| `BLOCK` (default) | The agent receives a preview page. No detection flow is started and nothing is recorded. |
| `BLOCK_NOTIFY` | The same preview page, and the hit is recorded as not detected so you can see the traffic. Repeated identical hits are counted together. |
| `ALLOW` | The flow proceeds as normal. |

The preview page is built for link unfurling, so your link looks right when shared in chat apps. You can customize its title prefix, description, logo URL and site name.

```json
{
  "titlePrefix": "Verify your number",
  "logoUrl": "https://static.ideamart.io/static/app/box.png"
}
```

As with the loading icon, logos must be hosted on an approved source. Send the file to your account manager.

### Detection protocol

The detection hop needs plain HTTP for the network to add its header. Your app uses one of these modes:

| Mode | Behavior |
|------|----------|
| `ANY` (default) | Accepts HTTP or HTTPS. An HTTPS request proceeds without the network header and reports `NOT_DETECTED`. |
| `HTTP` | Always uses the HTTP hop. Best detection rate, but may fail in browsers that have remembered a strict-HTTPS rule for the domain. |
| `OVERRIDE` | You choose per request with `protocol=http` or `protocol=https` on the authorize URL. |

### Out-of-order and replay handling

When a customer reopens or bookmarks an intermediate link, the platform treats it as out of order. Your app is set to one of:

| Setting | Behavior |
|---------|----------|
| `SILENT` (default) | The customer is redirected to your callback with `status=INVALID_STATE`. |
| `ERROR` | The customer sees an error page instead. |

### Security validation

Optional integration with an anti-fraud provider. Available providers, and what each requires, are confirmed during onboarding. Request parameters and results are in [Entry point and request parameters](#entry-point-and-request-parameters) and [Error codes](#error-codes).

### Flow type

The default `HE_ONLY` flow has no visible steps. A `MULTI` flow, which shows the customer a page with verification options, also exists. The options it presents are not described in this guide. If you need it, ask your account manager.

### Precheck and subscription data

Settings exist to add precheck or subscription status to responses, or to withhold the number unless a check passes. These are not generally available. Ask your account manager.

### Test mode

For development, your app can be set to return the result as a JSON page instead of redirecting. Ask your account manager to switch your app to test mode and back.

---
