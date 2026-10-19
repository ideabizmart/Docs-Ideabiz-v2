

## Content
* [Overview](#overview)
* [Method](#method)
* [Authorisation](#authorisation)
* [Query the location of one mobile terminal](#query-the-location-of-one-mobile-terminal)
* [Response Codes](#response-codes)
* [Exceptions](#exceptions)
* [Faults](#faults)
 
## Overview

Dialog LBS (Location Based Service) API allows you to query the location of one or multiple mobile terminls. LBS services are accessible via RESTful web services.

 

## Method

The following REST methods are available.

   * Query the location of one mobile terminal.

   * Query the location of multiple mobile terminals.

 
## Authorisation

### Request Header
```sh
Content-Type: application/json
Authorization: Bearer [access token]
Accept: application/json
```

## Query the location of one mobile terminal

 

### Request

Following is a sample request of Location service.

##### URL
```sh
https://ideabiz.lk/apicall/location/v1/queries/location?address={Subscriber Number}&requestedAccuracy=1000
```

##### Method
```sh
GET
``` 

### Response

Following is a sample response of Location service.
```sh
{
    "terminalLocationList": {
        "terminalLocation": {
            "address": "tel:+94777123456",
            "currentLocation": {
                "accuracy": "10.0",
                "altitude": "",
                "latitude": "6.893967",
                "longitude": "79.857292",
                "timestamp": "2014-09-03T08:27:08"
            },
            "locationRetrievalStatus": "Retrieved"
        }
    }
}
```

<br>
Following are the Response parameters of Location service.


| **Parameter name** | **Description** | **Type** | **Mandatory/Optional** |
| --- | --- | --- | --- |
| terminalLocation | The terminal location information. | string | Mandatory |
| address | MSISDN of the mobile terminal to locate. in this case the recipients MSISDN including the ‘tel:’ protocol identifier and the country code preceded by ‘+’. i.e. tel:+ 94771234567. | string | Mandatory |
| currentLocation | Current location of the terminal. | string | Mandatory |
| accuracy | Result accuracy in metres. | string | Mandatory |
| altitude | Metres. | string | Mandatory |
| latitude | Decimal Degrees, ISO 6709. | string | Mandatory |
| longitude | Decimal Degrees, ISO 6709. | string | Mandatory |
| timestamp | DateTime format. | string | Mandatory |
| locationRetrievalStatus | NotRetrieved - Unable to retrieve the terminal location. Error - Error retrieving the terminal location. | string |  |
| locationCheckWith"latitude&longitude" | Use this format to check the location in Maps. latitude,longitude eg: 6.893967,79.857292 | string |  |

 <br>
## Response Codes

200 – Success!<br>
400 – Bad request; check the error message for details<br>
401 – Authentication failure, check your authentication details<br>
403 – Forbidden; please provide authentication credentials<br>
404 – Not found: mistake in the host or path of the service URI<br>
405 – Method not supported: for example you mistakenly used a HTTP GET instead of a POST<br>
500 – The server encountered an unexpected condition. This could be wrong authentication details or limited user permission<br>
503 – Server busy and service unavailable. Please retry the request.
 

 

### Exceptions

##### Types

###### Policy Exception

Message Id start with <code>PL</code>

 

###### Server Exception

Message Id start with <code>SV</code>

 

##### Exception Body
```sh
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

 

### Faults

HTTP Respose code <code>503</code>

##### Fault Response Body
```sh
{
    "fault": {
        "code": "900800",
        "message": "Message Throttled Out",
        "description": "You have exceeded your quota"
    }
}
```

			



 

 