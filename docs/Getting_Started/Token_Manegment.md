# Token Management (OAuth 2.0)

> [!NOTE]
> **Why do we need Token Management?**
> Every request you send to Ideabiz must be accompanied by an **Access Token**. For security reasons, each Access Token expires after **1 hour (3,600 seconds)**. 
> 
> Instead of requiring your username and password every hour, Ideabiz provides a **Refresh Token**. This guide explains how to generate your initial token and how to automatically refresh it when it expires.

---

## 1. The Token Lifecycle (How It Works)

```mermaid
stateDiagram-v2
    [*] --> GenerateInitial: App launches / First time setup
    GenerateInitial --> TokenActive: Receives Access Token + Refresh Token
    
    state TokenActive {
        [*] --> MakingAPICalls: Use Bearer Access Token in API headers
        MakingAPICalls --> TokenActive: Valid for 60 minutes
    }
    
    TokenActive --> TokenExpired: 60 minutes pass (HTTP 401 Code 900903)
    TokenExpired --> RefreshingToken: Call /token with Refresh Token
    RefreshingToken --> TokenActive: Store new Access Token & new Refresh Token
```

---

## 2. Token Overview Cheat Sheet

| Token Type | Lifespan | Purpose | Where to include? |
| :--- | :--- | :--- | :--- |
| **Basic Auth Code** | Permanent | Identifies your application using `ConsumerKey:ConsumerSecret` | In the `/token` endpoint header |
| **Access Token** | **1 Hour** | Grants permission to call Ideabiz APIs (SMS, Payment, etc.) | In API headers as `Authorization: Bearer <token>` |
| **Refresh Token** | Long-lived | Used to obtain a new Access Token without re-entering credentials | Sent to `/token` when the access token expires |

---

## 3. Method 1: Creating Your Initial Token (API Call)

Use this method when setting up your application for the first time or when testing in Postman.

### Request Details

- **Endpoint URL:** `https://ideabiz.lk/apicall/token`
- **HTTP Method:** `POST`
- **Headers:**
  ```http
  Content-Type: application/x-www-form-urlencoded
  Authorization: Basic [BASE64_CONSUMER_KEY_AND_SECRET]
  ```
  *(See [Generating Your API Keys](./Generate_Token.md) for how to generate the Basic Auth header).*

- **Request Body (URL-encoded Form Data):**

| Parameter | Value | Description |
| :--- | :--- | :--- |
| `grant_type` | `password` | Specifies that you are logging in with user credentials |
| `username` | `your_username` | Your Ideabiz account username |
| `password` | `your_password` | Your Ideabiz account password |
| `scope` | `PRODUCTION` | Environment scope |

### Step-by-Step in Postman (No Code Required):
1. Create a new request in Postman with method **POST** and URL `https://ideabiz.lk/apicall/token`.
2. Go to the **Authorization** tab, select **Basic Auth**, and enter your **Consumer Key** and **Consumer Secret**.
3. Go to the **Body** tab, select **x-www-form-urlencoded**, and add the 4 key-value pairs (`grant_type`, `username`, `password`, `scope`).
4. Click **Send**.

### Successful Response (HTTP 200 OK)
```json
{
    "scope": "PRODUCTION",
    "token_type": "bearer",
    "expires_in": 3600,
    "refresh_token": "79b32c4a92df4c5fa34...",
    "access_token": "b489a243c9884e88ab1..."
}
```

> [!IMPORTANT]
> Save **both** the `access_token` and the `refresh_token` in your application or environment variables.

---

## 4. Method 2: Creating Tokens via the Ideabiz Portal (Zero Code)

If you just need a temporary token for quick manual testing:
1. Log in to [Ideabiz Portal](https://www.ideabiz.lk).
2. Go to **My Subscriptions**.
3. Under the **Keys - Production** section, you can generate and copy an active **Access Token** and **Refresh Token** directly.

---

## 5. Renewing an Expired Token (The Refresh Flow)

When your 1-hour Access Token expires, any API call you make will fail with an HTTP 401 error:
```xml
<ams:fault xmlns:ams="http://wso2.org/apimanager/security">
   <ams:code>900903</ams:code>
   <ams:message>Access Token Expired</ams:message>
   <ams:description>Access failure for API: /smsmessaging, version: v3</ams:description>
</ams:fault>
```

When this happens, use your saved **Refresh Token** to obtain a fresh pair of tokens.

### Refresh Request Details

- **Endpoint URL:** `https://ideabiz.lk/apicall/token`
- **HTTP Method:** `POST`
- **Headers:**
  ```http
  Content-Type: application/x-www-form-urlencoded
  Authorization: Basic [BASE64_CONSUMER_KEY_AND_SECRET]
  ```
- **Request Body:**

| Parameter | Value | Description |
| :--- | :--- | :--- |
| `grant_type` | `refresh_token` | Indicates you are renewing an existing session |
| `refresh_token` | `[YOUR_REFRESH_TOKEN]` | The refresh token from your previous token response |
| `scope` | `PRODUCTION` | Environment scope |

### Response (New Tokens Issued)
```json
{
    "scope": "PRODUCTION",
    "token_type": "bearer",
    "expires_in": 3600,
    "refresh_token": "a189f332c...",
    "access_token": "c782b198e..."
}
```

> [!CAUTION]
> **Important Rule for Developers:**
> Whenever you call the refresh endpoint, Ideabiz returns a **brand-new Refresh Token** along with the new Access Token. You must overwrite your stored refresh token with the new one so your next renewal cycle succeeds!

---

## 6. Troubleshooting Common Token Errors

| Error Code / Message | Probable Cause | How to Fix |
| :--- | :--- | :--- |
| **`900903`** <br> *Access Token Expired* | 1 hour has elapsed since the token was issued. | Trigger the refresh token API call (Method 5 above). |
| **`900904`** <br> *Access Token Inactive* | The token was invalidated, revoked, or regenerated elsewhere. | Perform a fresh token login using your credentials (Method 3). |
| **`invalid_client`** <br> *Client Authentication failed* | Incorrect Consumer Key or Consumer Secret in the `Basic` authorization header. | Double-check keys on the Ideabiz *My Subscriptions* page. |
| **`invalid_grant`** | The refresh token provided is invalid or has already been used. | Generate a fresh token pair using username and password. |

---

## 7. Automation & SDKs

Rather than refreshing tokens manually, your server should automatically catch HTTP `401 (900903)` responses and request a new token automatically.

- **PHP Handler Sample:** [IdeaBiz-Request-Handler-PHP (GitHub)](https://github.com/ideabizlk/IdeaBiz-Request-Handler---PHP)
