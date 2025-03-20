/*
Title: User Verification API
Sort:
*/
 

## Overview

Dialog user verification API provides user to verify a Dialog user's msisdn and NIC related details and this service can be accessed via a RESTful API service.

 

## Method

The following REST method is available: 
Submit Dilaog users msisdn and NIC to get it validated as 'ValidInfo' or 'Invalid Details'


 


### Request Header
```
Authorization: Bearer [access token]
Accept: application/json
```
 

### Request
```
https://ideabiz.lk/apicall/valid/V1.0
```
 

### Request Body
```
{

  "msisdn": "tel:+94766691500",

  "nic": "901599324V"

 }
```

### Response

#### If valid user response

 
```
{

    "statusCode": "200",

    "statusDetails": "ValidInfo"

}
```
 
#### Invalid validation
```
{

    "statusCode": "403",

    "statusDetails": "Invalid Details"

}
```

#### If invalid Process

```
{

    "statusCode": "503",

    "statusDetails": "Error"

}
```