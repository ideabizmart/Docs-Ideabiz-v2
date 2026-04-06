
# Number Verification API

## Overview

The **Number Verification API** allows users to validate whether an action authorized by a given mobile number is genuinely initiated by the same number via the network. This process involves two steps:

1. **Authentication**
2. **Number Verification**

## Authentication: Three-Legged Authentication Management (1.0.0)

#### URL
`https://ideabiz.lk/apicall/three-legged-authentication-mgt/1.0.0/authorize`

#### HTTP Method
`GET`

#### cURL Example
```bash
curl --location 'https://ideabiz.lk/apicall/three-legged-authentication-mgt/1.0.0/authorize?response_type={response_type}&client_id={client_id}&redirect_uri={redirect_uri}&scope={scope}&nonce={nonce}&state={state}' \
```

#### Request Parameters
| Parameter      | Description                                                                                          | Mandatory |
|----------------|------------------------------------------------------------------------------------------------------|-----------|
| response_type  | The type of response desired from the authorization server. Must be `code` for Authorization Grant.  | Yes       |
| client_id      | The client identifier as assigned by Ideabiz.                                                       | Yes       |
| redirect_uri   | The URI to which the response will be sent after authorization.                                      | Yes       |
| scope          | Access request scope, e.g., `dpv:FraudPreventionAndDetection:phone_verification:verify`.             | Yes       |
| nonce          | A unique value for each transaction, consistent throughout the transaction cycle, for replay attack mitigation. | Yes |
| state          | An optional opaque value to maintain state between request and callback.                            | No        |

#### Response
**Status Code**: 302 Found

- A code and state are returned as query parameters in the redirection URL, e.g., `{redirect_uri}?code=123456&state=state_1`.

#### Error Responses
| Status Code | Description                                                                                   |
|-------------|-----------------------------------------------------------------------------------------------|
| 302 Found   | Unable to identify the user via network-based authentication. Redirects with an error message. |
| 400 Bad Request | Invalid `ClientID` or duplicate `nonce`.                                                   |

---

### Token API

#### URL
`https://ideabiz.lk/apicall/three-legged-authentication-mgt/1.0.0/token`

#### HTTP Method
`POST`

#### cURL Example
```bash
curl --location 'https://ideabiz.lk/apicall/three-legged-authentication-mgt/1.0.0/token' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Basic <Base64(Client_id:client_secret)>
--data '{
    "code": "123456",
    "redirect_uri": "https://mockurl.com/callback",
    "grant_type": "authorization_code"
}'
```

#### Request Body
```json
{
  "code": "6-digit code from authorization response",
  "redirect_uri": "URI used in authorize API",
  "grant_type": "authorization_code"
}
```

#### Response
**Status Code**: 200 OK
```json
{
  "access_token": "access-token-value",
  "token_type": "Bearer",
  "expires_in": 2847702,
  "nonce": "nonce-value"
}
```

#### Error Responses
| Status Code | Description                                                                                   |
|-------------|-----------------------------------------------------------------------------------------------|
| 401 Unauthorized | Invalid token, expired authorization code, or missing `Authorization` header.               |
| 400 Bad Request | Incorrect `grantType` or invalid input.                                                    |

---

## Number Verification: Verify MSISDN API (1.0.0)

#### URL
`https://ideabiz.lk/apicall/camara-api/number-verification/1.0.0/verify`

#### HTTP Method
`POST`

#### cURL Example
```bash
curl --location 'https://ideabiz.lk/apicall/camara-api/number-verification/1.0.0/verify' \
--header 'Authorization: Bearer ACCESS_TOKEN' \
--header 'Content-Type: application/json' \
--header 'X-Correlator: nonce-value' \
--data '{
    "phoneNumber": "+94768912579"
}'
```

#### Request Body
- Provide either `phoneNumber` or `hashedPhoneNumber`.

```json
{
  "phoneNumber": "+947689212121"
}

{
  "hashedPhoneNumber": "hashed-value-of-phone-number"
}
```

- **phoneNumber**: In E.164 format or numeric format without the '+' prefix.
- **hashedPhoneNumber**: SHA-256 hash (hexadecimal) of the phone number in E.164 format.

#### Headers
- `X-Correlator`: Nonce value from the Token API response.

#### Response
**Status Code**: 200 OK
```json
{
  "devicePhoneNumberVerified": true
}
```

#### Error Responses
| Status Code | Description                                                                                   |
|-------------|-----------------------------------------------------------------------------------------------|
| 400 Bad Request | Phone number in an incorrect format or multiple identifiers provided.                     |
| 403 Forbidden   | Access denied due to token or permission issues.                                          |
| 500 Internal Server Error | General server error.                                                             |
| 401 Unauthorized | Missing or invalid `Authorization` token.                                                |

---

## Assumptions, Risks, and Limitations

### Limitations
- **Mobile Internet Only**: MSISDN verification is only supported for users on mobile data.
- **Single MSISDN Validation**: Each request can only process one MSISDN.

---

## Appendices

### Error Codes
| Code   | Description                        |
|--------|------------------------------------|
| AE50X  | Server Error                       |
| AE50H  | Southbound Server Error            |
| AE40X  | Invalid input values               |
| AE40U  | Unauthorized                       |
| AE40N  | Not Found                          |
| AE40F  | Access forbidden for the partner   |
