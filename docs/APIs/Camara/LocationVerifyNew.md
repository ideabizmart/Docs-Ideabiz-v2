/*
Title:Location Verify
Sort: 8
*/

## Content
* [Overview](#overview)
* [Requirements](#requirements)
* [API Request](#api-request)
* [Code Snippets](#code-snippets)
* [Errors Codes and Responses](#errors-codes-and-responses)

## Overview

This API provides the customer with the ability to verify the location of a device.  

### Introduction
Customers are able to verify whether the location of certain user device is within the area specified by the provided coordinates (latitude and longitude) and some expected accuracy.

### Quick Start
Device location API v1.0 exposes only one endpoint, which can be used to verify device location for user equipment. To do so the user has to pass the following parameters in request body:
1. **ueId** - an object with one fields, to pass ueId in msisdn format
2. **uePort** - if a public ipv4Addr is provided for the ueId object, the port allocated to the UE must also be specified
3. **latitude** - double number value for the latitude to be verified in decimal degrees.
4. **longitude** - double number value for the longitude to be verified in decimal degrees.
5. **accuracy** - number value for the expected accuracy in km. The allowed range is between 2 km and 200 km. 

Client asks whether the device location is within a circle with center specified by the latitude and longitude, and radius specified by the accuracy.

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
### URL
```
https://ideabiz.lk/apicall/openapi/location/v1/
```
### Method
```
POST
```


### Request
```
{
    "ueId": {
        "msisdn": "9477xxxxxxx"
    },
    "uePort": "1234",
    "userLatitude": 6.933349531096565,
    "userLongitude": 79.8553565917186,
    "accuracy": "5"
}
```


### Response
```
{
    "verificationResult": true,
    "matchRate": "100"
}
```

## Code Snippets

Snippets elaborates REST based API call with "*curl"* to request

Please note, the credentials for API authentication purposes need to be adjusted based on your configuration.

### cURL Request
```
curl --location 'https://ideabiz.lk/apicall/openapi/location/v1/' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer 4cf15b2f-6120-3c73-a492-a36da2e28d15' \
--data '{
    "ueId": {
        "msisdn": "9477xxxxxxx"
    },
    "uePort": "1234",
    "userLatitude": 6.933349531096565,
    "userLongitude": 79.8553565917186,
    "accuracy": "5"
}'
```

## Errors Codes and Responses

Since Device location API is based on REST design principles and blueprints, well defined HTTP status codes and families specified by community are followed.

Following table provides an overview of common error names, codes, and messages applicable to Device location API.

| No | Error Name | Error Code | Error Message |
| --- | --- | --- | --- |
|1	|400 |	INVALID_INPUT |	"Expected property is missing: {property}" |
|2	|401 |	UNAUTHORIZED |	"No authorization to invoke operation" |
|3	|403 |	FORBIDDEN |	"Operation not allowed" |
|4	|404 |	NOT_FOUND |	"Not found" |
|5	|500 |	INTERNAL_SERVER_ERROR |	"Internal server error" |
|6	|503 |	SERVICE_UNAVAILABLE |	"Service unavailable" |
