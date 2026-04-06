/*
Title:PIN-Verification
Sort: 8
*/

## Content
* [Overview](#overview)
* [Requirements](#requirements)
* [API Request](#api-request)
* [Code Snippets](#code-snippets)
* [Errors Codes and Responses](#errors-codes-and-responses)
 

## Overview

This api allow user to verify MSISDN ondemand without subscribing the applicaiton. 

Flow is `Send Verify Request` -> `Submit PIN` 

### Introduction

OTP validation API API performs real-time checks to verify that the user possessed the device that carries the indicated mobile phone number. It provides a frequent method of verifying possession of the device by delivering an OTP (one-time password) through SMS and validating it afterwards.

SMS OTP (one time password) is a secure method for providing one-time access to an application or performing a single transaction. OTP is most effective and legitimate for a single transaction, unlike user-generated passwords. It is a secure authentication method where a text containing a unique alphanumeric or numeric code is sent to a mobile number (MSISDN).

The recipient then uses this code or password as an additional layer of security to login to a service, website or app.


### Quick Start

The usage of the API is based on several resources.

Before starting to use the API, the developer needs to know about the below specified details:

**API service endpoint**

Two endpoints are defined in One Time Password SMS API: <br>
- POST /apicall/openapi/one-time-password-sms/v1/validate-code : Sends an SMS with the desired message and an OTP code to the received phone number
- POST /apicall/openapi/one-time-password-sms/v1/submit-code : Verifies the received code as input is valid for the given authenticationId.

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
## API Request

### Verification Request
#### URL
```
https://ideabiz.lk/apicall/openapi/one-time-password-sms/v1/validate-code
```
### Method
```
POST
```

#### Request
```
{
    "msisdn":"9477xxxxxxx",
    "message":"Hi this is ABC Service, your code is {{code}}"
}
```

#### Response
```
{
    "statusCode": "SUCCESS",
    "message": null,
    "data": {
        "status": "PENDING_AUTH",
        "serverRef": "13b335bc7ce24b279ff250c0df106455",
        "msisdn": "tel:+9477xxxxxxx",
        "method": null,
        "description": null,
        "pinMeta": null,
        "serviceId": null
    }
}
```

### Submit PIN Request
#### URL
```
https://ideabiz.lk/apicall/openapi/one-time-password-sms/v1/submit-code
```
### Method
```
POST
```

#### Request
```
{
  "pin": "520972",
  "serverRef": "958e39b574a042c290e6019f0590e7f5"
}
```
#### Success Response

```
{
    "statusCode": "SUCCESS",
    "message": "Verification Status",
    "data": {
        "status": "SUCCESS",
        "serverRef": "76d9c3235cd84b8a9bf4389f1d0f2989",
        "msisdn": "tel:+9477xxxxxxx",
        "method": null,
        "description": null,
        "pinMeta": null,
        "serviceId": null
    }
}
```

#### Sample Error Response
```
{
  "statusCode": "ERROR",
  "message": "Wrong PIN",
  "data": null
}
```
```
{
  "statusCode": "ERROR",
  "message": "Max attempt exceeded",
  "data": null
}
```

## Code Snippets

### cURL Request - Validate Code
```
curl --location 'https://ideabiz.lk/apicall/openapi/one-time-password-sms/v1/validate-code' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer 4cf15b2f-6120-3c73-a492-a36da2e28d15' \
--header 'Cookie: JSESSIONID=B4C040834E8B6F042D89B3F1C6124BDA' \
--data '{
    "msisdn":"9477xxxxxxx",
    "message":"Hi this is ABC Service, your code is {{code}}"
}'
```

### cURL Request - Submit Code
```
curl --location 'https://ideabiz.lk/apicall/openapi/one-time-password-sms/v1/validate-code' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer 4cf15b2f-6120-3c73-a492-a36da2e28d15' \
--header 'Cookie: JSESSIONID=B4C040834E8B6F042D89B3F1C6124BDA' \
--data '{
  "pin": "684479",
  "serverRef": "76d9c3235cd84b8a9bf4389f1d0f2989"
}'
```

## Errors Codes and Responses

Since OTP validation API is based on REST design principles and blueprints, well defined HTTP status codes and families specified by community are followed.

Following table provides an overview of common error names, codes, and messages applicable to One Time Password SMS API.

| No | Error Name | Error Code | Error Message |
| --- | ---------- | ---------- | ------------- |
|1  |400 |  INVALID_ARGUMENT |  "Client specified an invalid argument, request body or query param" |
|2  |400 |  ONE_TIME_PASSWORD_SMS.VERIFICATION_EXPIRED |    "The authenticationId is no longer valid" |
|3  |400 |  ONE_TIME_PASSWORD_SMS.VERIFICATION_FAILED | "The maximum number of attempts for this authenticationId was exceeded without providing a valid OTP" |
|4  |400 |  ONE_TIME_PASSWORD_SMS.INVALID_OTP |  "The provided OTP is not valid for this authenticationId" |
|5  |401 |  UNAUTHENTICATED |  "Request not authenticated due to missing, invalid, or expired credentials" |
|6  |403 |  PERMISSION_DENIED |  "Client does not have sufficient permissions to perform this action" |
|7  |403 |  ONE_TIME_PASSWORD_SMS.MAX_OTP_CODES_EXCEEDED |  "Too many OTPs have been requested for this MSISDN. Try later." |
|8  |403 |  ONE_TIME_PASSWORD_SMS.PHONE_NUMBER_NOT_ALLOWED |  "Phone_number can't receive an SMS due to business reasons in the operator" |
|9  |403 |  ONE_TIME_PASSWORD_SMS.PHONE_NUMBER_BLOCKED |  "Phone_number is blocked to receive SMS due to any blocking business reason in the operator" |
|10  |404 |  NOT_FOUND | "A specified resource is not found" |
|11  |405 |  METHOD_NOT_ALLOWED | "The requested method is not allowed/supported on the target resource" |
|12  |406 |  NOT_ACCEPTABLE | "The server can't produce a response matching the content requested by the client through Accept-* headers" |
|13  |415 |  UNSUPPORTED_MEDIA_TYPE | "The server refuses to accept the request because the payload format is in an unsupported format" |
|14  |429 | TOO_MANY_REQUESTS |  "Either out of resource quota or reaching rate limiting" |
|15  |500 | INTERNAL | "Server error" |
|16  |503 | UNAVAILABLE | "Service unavailable" |
|17  |504 | TIMEOUT | "Request timeout exceeded. Try later." |