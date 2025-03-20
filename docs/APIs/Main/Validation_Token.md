/*
Title: Widget Token Validation API
Sort: 
*/

## Content

* [Overview](#overview)
* [Method](#method)
* [Requirements](#authorization-api-calls)
* [Validation Token Generation](#Validation-Token-Generation)
* [Response Codes](#response-codes)
* [Exceptions](#exceptions)
* [Faults](#faults)

## Overview
Widget Token Validation API allows you to generate validation token for your service.

### Method

The following REST methods are available: 
+  Obtaining validation token of a service.

## Requirements</br>

**Authorization API Calls**</br>
All API call requests to ideabiz.lk require Authorization headers. Please refer the Token Management (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) documentation for Authorization. 

**Request Header**
```
Authorization: Bearer [access token] 
Accept: application/json
```
 
**Sample Request Header**
```
Authorization: Bearer a92ba8hjgjhgjh3fa1609cabcd79
Accept: application/json
```

## Validation Token Generation

This allows you to send an SMS from your Web application to one or more addresses (MSISDNs).

#### URL
```
https://widget.ideabiz.lk/web/widget/token 
```

##### Method
```
GET
```

#### Response
```
{
  "validationToken": "722bc9c531d04fc6bf55854b09f28fcb"
}
```

<br>
### Response Codes
 <br>
200 – Success!<br>
400 – Bad request; check the error message for details<br>
401 – Authentication failure, check your authentication details<br>
403 – Forbidden; please provide authentication credentials<br>
404 – Not found: mistake in the host or path of the service URI<br>
405 – Method not supported: for example you mistakenly used a HTTP GET instead of a POST<br>
500 – The server encountered an unexpected condition. This could be wrong authentication details or limited user permission<br>
503 – Server busy and service unavailable. Please retry the request.<br>
 


### Exceptions

#### Types

##### Policy Exception

Message Id start with<code>PL</code>

##### Server Exception

Message Id start with <code>SV</code>

 

#### Exception Body
```
{
    "requestError": {
        "serviceException": {
            "messageId": "SVC0002",
            "text": " Invalid input value for message part %1",
            "variables": " clientCorrelator Value 12345"
        }
    }
}
```

## Faults

HTTP Respose code <code>503</code>
 
#### Fault Response Body
```
{
    "fault": {
        "code": "900800",
        "message": "Message Throttled Out",
        "description": "You have exceeded your quota"
    }
}
```

