# Number Lifecycle API

## Overview

The **Number Lifecycle API** allows users to check the status of a given mobile number to determine if any of the following events have occurred within a specified period:

- **SIM Swap**
- **Number Recycle**
- **Ownership Change**

These events help API users assess whether a specific transaction or event authorized by a mobile number is attributable to the identified user. The API enhances security, prevents fraud, ensures communication reliability, and protects sensitive information.

---

## Relevant Terms and Definitions

### SIM Swap
A SIM swap occurs when a user's mobile number (MSISDN) is associated with a new SIM card (IMSI). This can happen for reasons such as:
- Lost or damaged SIM cards
- Upgrading to a new phone
- Changing phone numbers or service providers
- Activating a new SIM for the same phone number (e.g., multi-SIM service)

### Number Recycle
Number recycling occurs when an MSISDN is reclaimed by the operator after the connection is permanently disconnected. 
- **Postpaid connections**: Recycling happens one year after permanent disconnection.
- **Prepaid connections**: Recycling occurs after a 52-day grace period following the validity period of the prepaid connection.

### Ownership Change
Ownership change involves transferring the legal ownership of a mobile connection to a new user after a formal written request from the original owner. The phone number is then registered under the identity (KYC data) of the new user.

---

## API Details

### URL
`https://ideabiz.lk/apicall/camara-api/number-lifecycle/1.0.0/check`

### HTTP Method
`POST`

### cURL Example
```bash
curl --location 'https://ideabiz.lk/apicall/camara-api/number-lifecycle/1.0.0/check' \
--header 'Authorization: Bearer ACCESS TOKEN' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--data '{
    "phoneNumber": "+94751238037",
    "maxAge": 240
}'
```

### Request Body
```json
{
  "phoneNumber": "+947689212121",
  "maxAge": 240
}
```

- **phoneNumber**: The phone number in E.164 format (e.g., +94765095827) or numeric format without the '+' prefix (e.g., 94765095827).
- **maxAge** (optional): Period (in hours) to check for SIM swap. Defaults to 240 if not provided.

### Headers
- `X-Correlator`: Optional

---

## Responses

### Success Response
**Status Code**: 200 OK  
```json
{
  "swapped": false,
  "recycled": false,
  "ownerChanged": false
}
```
- Boolean values: `true | false`

---

### Error Responses
| Status Code | Code                          | Message                                               | Cause                                      |
|-------------|-------------------------------|-------------------------------------------------------|--------------------------------------------|
| 400         | `INVALID.INPUT`              | `maxAge: must be greater than or equal to 1`          | Invalid `maxAge` value                     |
| 400         | `INVALID.INPUT`              | `maxAge: must be less than or equal to 2400`          | Invalid `maxAge` value                     |
| 400         | `INVALID.INPUT`              | `phoneNumber: must not be null`                       | Missing `phoneNumber`                      |
| 400         | `SIM_SWAP.UNKNOWN_PHONE_NUMBER` | `SIM Swap can't be checked because the phone number is unknown.` | Phone number not in the Dialog network |
| 400         | `INVALID_ARGUMENT`           | `Client specified an invalid argument, request body or query param` | Incorrect JSON request format         |
| 500         | `INTERNAL`                   | `Internal server error`                               | Server issue                               |
| 401         | `900901`                     | `Access Token Expired`                                | Authorization token is missing or invalid  |

---

## Assumptions, Risks, and Limitations

### Limitations
- **Operator**: The API only supports MSISDN verification for users connected with Dialog/Airtel Operators (not other networks).
- **Single MSISDN Validation**: The API processes one MSISDN per request (no batch processing).

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
