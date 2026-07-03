# KYC / Information Verification Platform API

## Overview

The KYC / Information Verification Platform (IVP) API provides verification services for customer identity and profile-related checks using MSISDN and NIC information.

> **Security note:** These APIs process sensitive customer information such as MSISDN and NIC. Avoid logging full identifiers in application logs, mask values where possible, and ensure callback endpoints are protected with HTTPS and access controls.

## Requirements

**Authorization API calls** </br>
All API call request to ideabiz.lk required Authorization headers. Please refer the *Token Management* (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) document for Authorization.

**Request Header**
```
Content-Type: application/json 
Authorization: Bearer [access token] 
Accept: application/json
```
 
**Sample Request Header**
```
Content-Type: application/json 
Authorization: Bearer a92ba8hjgjhgjh3fa1609cabcd79
Accept: application/json
```


## Consent and Callback Behaviour

Most APIs in this specification accept a `callback_api` value in the request body. For those APIs, the platform can return either:

- an immediate acceptance response when consent is requested; or
- a final result response through the callback API / when consent is not requested.

```json
{
  "success": true,
  "message": "Request accepted!"
}
```

## Available APIs

The links below point to the relevant section within this same Markdown file.

| # | API | Method |
|---:|---|---|
| 1 | [Location Verification](#api-1-location-verification) | `POST` |
| 2 | [Account Status](#api-2-account-status) | `POST` |
| 3 | [Adoption Score](#api-3-adoption-score) | `POST` |
| 4 | [Preferred OTP Channel](#api-4-preferred-otp-channel) | `POST` |
| 5 | [Device Profile](#api-5-device-profile) | `POST` |
| 6 | [Mobile Spend Category](#api-6-mobile-spend-category) | `POST` |
| 7 | [Preferred Communication Language](#api-7-preferred-communication-language) | `POST` |
| 8 | [Network Stay](#api-8-network-stay) | `POST` |
| 9 | [NIC Verification](#api-9-nic-verification) | `POST` |


---

<a id="api-1-location-verification"></a>

## 1. Location Verification

### Overview

Verifies whether the provided latitude/longitude is associated with the customer MSISDN/NIC combination for either home or work location.


### Endpoint

| Item | Value |
|---|---|
| Resource URI | `https://ideabiz.lk/apicall/KycLocationVerification/v1/location` |
| HTTP Method | `POST` |

### Request Headers

| Header | Required | Value |
|---|---:|---|
| `Accept` | Yes | `application/json` |
| `Content-Type` | Yes | `application/json` |


### Request Body

```json
{
  "msisdn": "<msisdn>",
  "nic": "<nic>",
  "lat": "<lat>",
  "long": "<long>",
  "type": "home or work",
  "callback_api": "<callback API endpoint>"
}
```

### Sample Request

```json
{
  "msisdn": "765654584",
  "nic": "943153658v",
  "lat": "7.6556",
  "long": "80.4525",
  "type": "home",
  "callback_api": "https://example.com/callback"
}
```

### Request Parameters

| Parameter | Type | Mandatory | Description |
|---|---|---:|---|
| `msisdn` | String | Yes | MSISDN must start with `74`, `76`, or `77`. |
| `nic` | String | Yes | NIC value. Both old and new NIC formats should be supported. |
| `lat` | Double | Yes | Latitude. |
| `long` | Double | Yes | Longitude. |
| `type` | String | Yes | Location type. Allowed values: `home`, `work`. |
| `callback_api` | String | Yes | Callback API endpoint. |

### Response Information

#### Success Response - On Consent Requested

```json
{
  "success": true,
  "message": "Request accepted!"
}
```

#### Success Response - Callback API / On Consent Not Requested

```json
{
  "success": true,
  "message": "Location true!",
  "data": {
    "location": true,
    "distance": 2223.898532891127,
    "active_percentage": 94.6,
    "no_of_days": 32,
    "unique_active_hours": 5,
    "msisdn": 776072666
  }
}
```

### Response Parameters

| Parameter | Type | Description |
|---|---|---|
| `success` | Boolean | `true` when the request is successful; `false` when the request fails. |
| `message` | String | Response message. |
| `data` | Object | Result object returned for successful verification responses. |


### Data Object Fields

| Field | Type | Description |
|---|---|---|
| `location` | Boolean | Indicates whether the location verification is true or false. |
| `distance` | Double | Calculated distance value returned by the platform. |
| `active_percentage` | Double | Active percentage value. |
| `no_of_days` | Integer | Number of days considered for the result. |
| `unique_active_hours` | Integer | Unique active hours considered for the result. |
| `msisdn` | Integer | Customer MSISDN. |

### Error Handling

The source specification does not define detailed error response formats or HTTP status-code mappings. Before production publication, confirm and document standard error responses such as `400`, `401`, `403`, `404`, `429`, and `500` if they are applicable.


---


---

<a id="api-2-account-status"></a>

## 2. Account Status

### Overview

Checks the account status for the provided MSISDN and NIC.

> **Note:** Replace `{{url}}` with the provisioned API host/base URL for the production or test environment. The source specification lists `Accept` and `Content-Type` headers only. Add any Ideabiz gateway authorization headers required for publishing/subscription before production release.

### Endpoint

| Item | Value |
|---|---|
| Resource URI | `https://ideabiz.lk/apicall/account-status/v1/account-status` |
| HTTP Method | `POST` |

### Request Headers

| Header | Required | Value |
|---|---:|---|
| `Accept` | Yes | `application/json` |
| `Content-Type` | Yes | `application/json` |


### Request Body

```json
{
  "msisdn": "<msisdn>",
  "nic": "<nic>",
  "callback_api": "<callback API endpoint>"
}
```

### Sample Request

```json
{
  "msisdn": "765654584",
  "nic": "943153658v",
  "callback_api": "https://example.com/callback"
}
```

### Request Parameters

| Parameter | Type | Mandatory | Description |
|---|---|---:|---|
| `msisdn` | String | Yes | MSISDN must start with `74`, `76`, or `77`. |
| `nic` | String | Yes | NIC value. Both old and new NIC formats should be supported. |
| `callback_api` | String | Yes | Callback API endpoint. |

### Response Information

#### Success Response - On Consent Requested

```json
{
  "success": true,
  "message": "Request accepted!"
}
```

#### Success Response - Callback API / On Consent Not Requested

```json
{
  "success": true,
  "message": "Account Status get successfully!",
  "data": {
    "status": "YES",
    "msisdn": 776072667
  }
}
```

### Response Parameters

| Parameter | Type | Description |
|---|---|---|
| `success` | Boolean | `true` when the request is successful; `false` when the request fails. |
| `message` | String | Response message. |
| `data` | Object | Result object returned for successful verification responses. |


### Data Object Fields

| Field | Type | Description |
|---|---|---|
| `status` | String | Account status value. |
| `msisdn` | Integer | Customer MSISDN. |

### Error Handling

The source specification does not define detailed error response formats or HTTP status-code mappings. Before production publication, confirm and document standard error responses such as `400`, `401`, `403`, `404`, `429`, and `500` if they are applicable.


---


---

<a id="api-3-adoption-score"></a>

## 3. Adoption Score

### Overview

Returns the adoption score for the provided MSISDN and NIC.

> **Note:** Replace `{{url}}` with the provisioned API host/base URL for the production or test environment. The source specification lists `Accept` and `Content-Type` headers only. Add any Ideabiz gateway authorization headers required for publishing/subscription before production release.

### Endpoint

| Item | Value |
|---|---|
| Resource URI | `https://ideabiz.lk/apicall/KycAdoptionScore/v1/adoption-score` |
| HTTP Method | `POST` |

### Request Headers

| Header | Required | Value |
|---|---:|---|
| `Accept` | Yes | `application/json` |
| `Content-Type` | Yes | `application/json` |


### Request Body

```json
{
  "msisdn": "<msisdn>",
  "nic": "<nic>",
  "callback_api": "<callback API endpoint>"
}
```

### Sample Request

```json
{
  "msisdn": "765654584",
  "nic": "943153658v",
  "callback_api": "https://example.com/callback"
}
```

### Request Parameters

| Parameter | Type | Mandatory | Description |
|---|---|---:|---|
| `msisdn` | Integer | Yes | MSISDN must start with `74`, `76`, or `77`. |
| `nic` | String | Yes | NIC value. Both old and new NIC formats should be supported. |
| `callback_api` | String | Yes | Callback API endpoint. |

### Response Information

#### Success Response - On Consent Requested

```json
{
  "success": true,
  "message": "Request accepted!"
}
```

#### Success Response - Callback API / On Consent Not Requested

```json
{
  "success": true,
  "message": "Adoption Score get successfully!",
  "data": {
    "score": 10,
    "msisdn": 776072666
  }
}
```

### Response Parameters

| Parameter | Type | Description |
|---|---|---|
| `success` | Boolean | `true` when the request is successful; `false` when the request fails. |
| `message` | String | Response message. |
| `data` | Object | Result object returned for successful verification responses. |


### Data Object Fields

| Field | Type | Description |
|---|---|---|
| `score` | Integer | Adoption score value. |
| `msisdn` | Integer | Customer MSISDN. |

### Error Handling

The source specification does not define detailed error response formats or HTTP status-code mappings. Before production publication, confirm and document standard error responses such as `400`, `401`, `403`, `404`, `429`, and `500` if they are applicable.


---


---

<a id="api-4-preferred-otp-channel"></a>

## 4. Preferred OTP Channel

### Overview

Returns the preferred OTP channel for the provided MSISDN and NIC.

> **Note:** Replace `{{url}}` with the provisioned API host/base URL for the production or test environment. The source specification lists `Accept` and `Content-Type` headers only. Add any Ideabiz gateway authorization headers required for publishing/subscription before production release.

> **Implementation note:** The response field name `preffred_chennel` is misspelled in the source specification. It has been preserved here to avoid changing the API contract.

### Endpoint

| Item | Value |
|---|---|
| Resource URI | `https://ideabiz.lk/apicall/KycPreferredOTPChannel/v1/preferred-channel` |
| HTTP Method | `POST` |

### Request Headers

| Header | Required | Value |
|---|---:|---|
| `Accept` | Yes | `application/json` |
| `Content-Type` | Yes | `application/json` |


### Request Body

```json
{
  "msisdn": "<msisdn>",
  "nic": "<nic>",
  "callback_api": "<callback API endpoint>"
}
```

### Sample Request

```json
{
  "msisdn": "765654584",
  "nic": "943153658v",
  "callback_api": "https://example.com/callback"
}
```

### Request Parameters

| Parameter | Type | Mandatory | Description |
|---|---|---:|---|
| `msisdn` | Integer | Yes | MSISDN must start with `74`, `76`, or `77`. |
| `nic` | String | Yes | NIC value. Both old and new NIC formats should be supported. |
| `callback_api` | String | Yes | Callback API endpoint. |

### Response Information

#### Success Response - On Consent Requested

```json
{
  "success": true,
  "message": "Request accepted!"
}
```

#### Success Response - Callback API / On Consent Not Requested

```json
{
  "success": true,
  "message": "Preffred channel get successfully!",
  "data": {
    "preffred_chennel": "viber",
    "msisdn": 776072666
  }
}
```

### Response Parameters

| Parameter | Type | Description |
|---|---|---|
| `success` | Boolean | `true` when the request is successful; `false` when the request fails. |
| `message` | String | Response message. |
| `data` | Object | Result object returned for successful verification responses. |


### Data Object Fields

| Field | Type | Description |
|---|---|---|
| `preffred_chennel` | String | Preferred OTP channel value returned by the API. Field name is preserved as specified in the source document. |
| `msisdn` | Integer | Customer MSISDN. |

### Error Handling

The source specification does not define detailed error response formats or HTTP status-code mappings. Before production publication, confirm and document standard error responses such as `400`, `401`, `403`, `404`, `429`, and `500` if they are applicable.


---


---

<a id="api-5-device-profile"></a>

## 5. Device Profile

### Overview

Returns device profile information for the provided MSISDN and NIC.

> **Note:** Replace `{{url}}` with the provisioned API host/base URL for the production or test environment. The source specification lists `Accept` and `Content-Type` headers only. Add any Ideabiz gateway authorization headers required for publishing/subscription before production release.

### Endpoint

| Item | Value |
|---|---|
| Resource URI | `https://ideabiz.lk/apicall/KycDeviceProfile/v1/device-profile` |
| HTTP Method | `POST` |

### Request Headers

| Header | Required | Value |
|---|---:|---|
| `Accept` | Yes | `application/json` |
| `Content-Type` | Yes | `application/json` |


### Request Body

```json
{
  "msisdn": "<msisdn>",
  "nic": "<nic>",
  "callback_api": "<callback API endpoint>"
}
```

### Sample Request

```json
{
  "msisdn": "765654584",
  "nic": "943153658v",
  "callback_api": "https://example.com/callback"
}
```

### Request Parameters

| Parameter | Type | Mandatory | Description |
|---|---|---:|---|
| `msisdn` | Integer | Yes | MSISDN must start with `74`, `76`, or `77`. |
| `nic` | String | Yes | NIC value. Both old and new NIC formats should be supported. |
| `callback_api` | String | Yes | Callback API endpoint. |

### Response Information

#### Success Response - On Consent Requested

```json
{
  "success": true,
  "message": "Request accepted!"
}
```

#### Success Response - Callback API / On Consent Not Requested

```json
{
  "success": true,
  "message": "Device profile get successfully!",
  "data": {
    "device_profile": {
      "MSISDN": "94776072667",
      "Type": "get",
      "Status": "OK,0,Successful",
      "handset": {
        "Vendor": "SAMSUNG",
        "Model": "Galaxy TV",
        "IMSI": "454545455",
        "IMEI": "45454545",
        "TAC": "45454545",
        "SubGroup": "POSTPAID"
      }
    },
    "msisdn": 776072666
  }
}
```

### Response Parameters

| Parameter | Type | Description |
|---|---|---|
| `success` | Boolean | `true` when the request is successful; `false` when the request fails. |
| `message` | String | Response message. |
| `data` | Object | Result object returned for successful verification responses. |


### Data Object Fields

| Field | Type | Description |
|---|---|---|
| `device_profile` | Object | Device profile object. |
| `device_profile.MSISDN` | String | MSISDN returned inside the device profile. |
| `device_profile.Type` | String | Request/result type. |
| `device_profile.Status` | String | Device profile status. |
| `device_profile.handset` | Object | Handset details object. |
| `device_profile.handset.Vendor` | String | Handset vendor. |
| `device_profile.handset.Model` | String | Handset model. |
| `device_profile.handset.IMSI` | String | IMSI value. |
| `device_profile.handset.IMEI` | String | IMEI value. |
| `device_profile.handset.TAC` | String | TAC value. |
| `device_profile.handset.SubGroup` | String | Subscriber group. |
| `msisdn` | Integer | Customer MSISDN. |

### Error Handling

The source specification does not define detailed error response formats or HTTP status-code mappings. Before production publication, confirm and document standard error responses such as `400`, `401`, `403`, `404`, `429`, and `500` if they are applicable.


---


---

<a id="api-6-mobile-spend-category"></a>

## 6. Mobile Spend Category

### Overview

Returns the mobile spend category value for the provided MSISDN and NIC.

> **Note:** Replace `{{url}}` with the provisioned API host/base URL for the production or test environment. The source specification lists `Accept` and `Content-Type` headers only. Add any Ideabiz gateway authorization headers required for publishing/subscription before production release.

### Endpoint

| Item | Value |
|---|---|
| Resource URI | `https://ideabiz.lk/apicall/KycMobileSpendCategory/v1/spend-category` |
| HTTP Method | `POST` |

### Request Headers

| Header | Required | Value |
|---|---:|---|
| `Accept` | Yes | `application/json` |
| `Content-Type` | Yes | `application/json` |


### Request Body

```json
{
  "msisdn": "<msisdn>",
  "nic": "<nic>",
  "callback_api": "<callback API endpoint>"
}
```

### Sample Request

```json
{
  "msisdn": "765654584",
  "nic": "943153658v",
  "callback_api": "https://example.com/callback"
}
```

### Request Parameters

| Parameter | Type | Mandatory | Description |
|---|---|---:|---|
| `msisdn` | Integer | Yes | MSISDN must start with `74`, `76`, or `77`. |
| `nic` | String | Yes | NIC value. Both old and new NIC formats should be supported. |
| `callback_api` | String | Yes | Callback API endpoint. |

### Response Information

#### Success Response - On Consent Requested

```json
{
  "success": true,
  "message": "Request accepted!"
}
```

#### Success Response - Callback API / On Consent Not Requested

```json
{
  "success": true,
  "message": "Mobile spend category get successfully!",
  "data": {
    "value": 2,
    "msisdn": 776072666
  }
}
```

### Response Parameters

| Parameter | Type | Description |
|---|---|---|
| `success` | Boolean | `true` when the request is successful; `false` when the request fails. |
| `message` | String | Response message. |
| `data` | Object | Result object returned for successful verification responses. |


### Data Object Fields

| Field | Type | Description |
|---|---|---|
| `value` | Integer | Mobile spend category value. |
| `msisdn` | Integer | Customer MSISDN. |

### Error Handling

The source specification does not define detailed error response formats or HTTP status-code mappings. Before production publication, confirm and document standard error responses such as `400`, `401`, `403`, `404`, `429`, and `500` if they are applicable.


---


---

<a id="api-7-preferred-communication-language"></a>

## 7. Preferred Communication Language

### Overview

Returns the preferred communication language for the provided MSISDN and NIC.

> **Note:** Replace `{{url}}` with the provisioned API host/base URL for the production or test environment. The source specification lists `Accept` and `Content-Type` headers only. Add any Ideabiz gateway authorization headers required for publishing/subscription before production release.

### Endpoint

| Item | Value |
|---|---|
| Resource URI | `https://ideabiz.lk/apicall/PreferredCommunicationLanguage/v1/preferred-language` |
| HTTP Method | `POST` |

### Request Headers

| Header | Required | Value |
|---|---:|---|
| `Accept` | Yes | `application/json` |
| `Content-Type` | Yes | `application/json` |


### Request Body

```json
{
  "msisdn": "<msisdn>",
  "nic": "<nic>",
  "callback_api": "<callback API endpoint>"
}
```

### Sample Request

```json
{
  "msisdn": "765654584",
  "nic": "943153658v",
  "callback_api": "https://example.com/callback"
}
```

### Request Parameters

| Parameter | Type | Mandatory | Description |
|---|---|---:|---|
| `msisdn` | Integer | Yes | MSISDN must start with `74`, `76`, or `77`. |
| `nic` | String | Yes | NIC value. Both old and new NIC formats should be supported. |
| `callback_api` | String | Yes | Callback API endpoint. |

### Response Information

#### Success Response - On Consent Requested

```json
{
  "success": true,
  "message": "Request accepted!"
}
```

#### Success Response - Callback API / On Consent Not Requested

```json
{
  "success": true,
  "message": "Preferred language get successfully!",
  "data": {
    "language": "SIN",
    "msisdn": 776072666
  }
}
```

### Response Parameters

| Parameter | Type | Description |
|---|---|---|
| `success` | Boolean | `true` when the request is successful; `false` when the request fails. |
| `message` | String | Response message. |
| `data` | Object | Result object returned for successful verification responses. |


### Data Object Fields

| Field | Type | Description |
|---|---|---|
| `language` | String | Preferred communication language value. |
| `msisdn` | Integer | Customer MSISDN. |

### Error Handling

The source specification does not define detailed error response formats or HTTP status-code mappings. Before production publication, confirm and document standard error responses such as `400`, `401`, `403`, `404`, `429`, and `500` if they are applicable.


---


---

<a id="api-8-network-stay"></a>

## 8. Network Stay

### Overview

Returns the customer network stay period for the provided MSISDN and NIC.

> **Note:** Replace `{{url}}` with the provisioned API host/base URL for the production or test environment. The source specification lists `Accept` and `Content-Type` headers only. Add any Ideabiz gateway authorization headers required for publishing/subscription before production release.

### Endpoint

| Item | Value |
|---|---|
| Resource URI | `https://ideabiz.lk/apicall/KycNetworkStay/v1/network-stay` |
| HTTP Method | `POST` |

### Request Headers

| Header | Required | Value |
|---|---:|---|
| `Accept` | Yes | `application/json` |
| `Content-Type` | Yes | `application/json` |


### Request Body

```json
{
  "msisdn": "<msisdn>",
  "nic": "<nic>",
  "callback_api": "<callback API endpoint>"
}
```

### Sample Request

```json
{
  "msisdn": "765654584",
  "nic": "943153658v",
  "callback_api": "https://example.com/callback"
}
```

### Request Parameters

| Parameter | Type | Mandatory | Description |
|---|---|---:|---|
| `msisdn` | Integer | Yes | MSISDN must start with `74`, `76`, or `77`. |
| `nic` | String | Yes | NIC value. Both old and new NIC formats should be supported. |
| `callback_api` | String | Yes | Callback API endpoint. |

### Response Information

#### Success Response - On Consent Requested

```json
{
  "success": true,
  "message": "Request accepted!"
}
```

#### Success Response - Callback API / On Consent Not Requested

```json
{
  "success": true,
  "message": "Network stay returned successfully",
  "data": {
    "network_stay": "1 - 3 Years",
    "msisdn": 776072667
  }
}
```

### Response Parameters

| Parameter | Type | Description |
|---|---|---|
| `success` | Boolean | `true` when the request is successful; `false` when the request fails. |
| `message` | String | Response message. |
| `data` | Object | Result object returned for successful verification responses. |


### Data Object Fields

| Field | Type | Description |
|---|---|---|
| `network_stay` | String | Network stay period. |
| `msisdn` | Integer | Customer MSISDN. |

### Error Handling

The source specification does not define detailed error response formats or HTTP status-code mappings. Before production publication, confirm and document standard error responses such as `400`, `401`, `403`, `404`, `429`, and `500` if they are applicable.


---


---

<a id="api-9-nic-verification"></a>

## 9. NIC Verification

### Overview

Verifies whether the provided NIC is associated with the provided MSISDN.

> **Note:** Replace `{{url}}` with the provisioned API host/base URL for the production or test environment. The source specification lists `Accept` and `Content-Type` headers only. Add any Ideabiz gateway authorization headers required for publishing/subscription before production release.

### Endpoint

| Item | Value |
|---|---|
| Resource URI | `https://ideabiz.lk/apicall/KycNICVerification/v1/nic-verification` |
| HTTP Method | `POST` |

### Request Headers

| Header | Required | Value |
|---|---:|---|
| `Accept` | Yes | `application/json` |
| `Content-Type` | Yes | `application/json` |


### Request Body

```json
{
  "msisdn": "<msisdn>",
  "nic": "<nic>"
}
```

### Sample Request

```json
{
  "msisdn": "765654584",
  "nic": "943153658v"
}
```

### Request Parameters

| Parameter | Type | Mandatory | Description |
|---|---|---:|---|
| `msisdn` | Integer | Yes | MSISDN must start with `74`, `76`, or `77`. |
| `nic` | String | Yes | NIC value. Both old and new NIC formats should be supported. |

### Response Information

#### Success Response

```json
{
  "success": true,
  "message": "NIC verification successfully",
  "data": {
    "status": true,
    "msisdn": 776072667,
    "nic": "943153663v"
  }
}
```

### Response Parameters

| Parameter | Type | Description |
|---|---|---|
| `success` | Boolean | `true` when the request is successful; `false` when the request fails. |
| `message` | String | Response message. |
| `data` | Object | Result object returned for successful verification responses. |


### Data Object Fields

| Field | Type | Description |
|---|---|---|
| `status` | Boolean | NIC verification status. |
| `msisdn` | Integer | Customer MSISDN. |
| `nic` | String | NIC returned in response. |

### Error Handling

The source specification does not define detailed error response formats or HTTP status-code mappings. Before production publication, confirm and document standard error responses such as `400`, `401`, `403`, `404`, `429`, and `500` if they are applicable.


---
