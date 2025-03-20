/*
Title: Subscription API
*/

## Contents

* [Overview](#overview)
* [Method](#method)
* [Mandatory For](#mandatory-for)
* [Requirements](#requirements)
* [Subscribe MSISDN](#subscribe-msisdn)
* [Unsubscribe MSISDN](#unsubscribe-msisdn)
* [Individual Subscription Check](#individual-subscription-check)
* [Get Subscriber Base](#get-subscriber-base)
* [Get Daily User Info](#get-daily-user-info)
* [Admin Notify URL](#admin-notify-url)
* [Subscription Status](#subscription-status)
* [Error Responses](#error-responses)


## Overview
* Implements the subscription and un-subscription functions of users from ideabiz services.
* The API also allows multiple user subscriptions through their respective MSISDN parameters.
* The Subscription API should only be called, following a customer request on Web/App/SMS. Calling the API without a customer request is considered misuse of the API.
Note:
    + The Subscription API is currently provided only for the un-subscription function and status check function. 
    + For the subscription function, please use one of the following methods:
         +	SMS
         +	USSD
         +	Web Widget
+ All the dynamic parameters are included inside brackets, which are to be omitted when they’re being used in API requests. (As shown below in “sample” under each function.)




### Method
The following REST methods are available:

+ Subscribe to your service
+ Unsubscribe from your service
+ Check the subscription status of a user
+ Get Subscriber Base
+ Get daily information on users'status

### Mandatory for

```
SMS API, Charging API, Balance Check API
```

## Requirements


**Authorization**

All API call requests to ideabiz.lk require Authorization headers. Please refer the Token Management (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) documentation for Authorization.


**Request Header**

```
Content-Type: application/json 
Authorization: Bearer [access token]
Accept: application/json
```

**Sample Request Header**

```
Content-Type: application/json 
Authorization: Bearer a92ba8e8hjgjhgjh3fa1609cabcd79
Accept: application/json
```

### States of Parameters

| Parameter Name | Description | Type | Mandatory /Optional |
| ------ | ------ | ------ | ------ |
| method | This is the Subscription Type. This is how the event was triggered: Eg: Android app, Web Site. (Max: 15 Characters) | string | Mandatory |
| msisdn | The user's number in the long format. Either Plain MSISDN or Encrypted MSISDN. </br> Eg:</br> PLAIN - tel:+94766691500 </br>ENCRYPTED -  etel:+9477-v%jkfdjkfh3#4 | string | Mandatory |
| serviceID | The serviceID is only required to be included in requests, when there are micro subscriptions available in the service. In all other cases, this parameter is optional.| string | Optional |

### Encrypted MSISDN

Please refer the Secure Header (http://docs.ideabiz.lk/APIs/Header_Enrichment) document for more information on encrypted MSISDN.


### Obtaining a serviceID

+ The serviceID is only used for micro subscriptions. If the app has only one subscription, this will be optional.
+ Please contact the Developer Support Team to obtain a serviceID when needed.

**Sample Request body with the serviceID included:**
```
{
    "method":"WEB",      
    "msisdn":"tel:+94766691500",
    "serviceID": "35EF979B-ART4-4101-9E11-315078D20BFE "       
}
```
## Subscribe MSISDN

#### URL

```
    https://ideabiz.lk/apicall/subscription/v3/subscribe
```
#### Method
```
    POST
```
#### Request Body
```
{
    "method":"WEB",      
    "msisdn": "tel:+94766691500"
}
```
#### Response Body
```
{
      "statusCode": "SUCCESS",
      "message": "",
      "data": {
        "subscribeResponse”: {
            "msisdn": "tel:+94766691500",
	    "status": "SUBSCRIBED",
            "serviceID": null        

          }
      }
}
```

## Unsubscribe MSISDN

#### URL

```
    https://ideabiz.lk/apicall/subscription/v3/unsubscribe
```
#### Method
```
    POST
```
#### Request Body
```
{
      "method":"WEB",
      "msisdn": "tel:+94766691500",    
      "serviceID": "ajer78-weer2-werd-wrfed"  
}
```
#### Response Body
```
{
     "statusCode": "SUCCESS",
     "message": "",
     "data": {
        "subscribeResponse":{
            "msisdn": "tel:+94766691500",
            "status": "UNSUBSCRIBED",
	    "serviceID": null
          }
      }
}
```
## Individual Subscription Check
#### URL

```
    https://ideabiz.lk/apicall/subscription/v3/status/{msisdn}
```
The msisdn should be URL encoded, and in long format (tel:+94766691500, etel:+9477-v1xxxxxxxx)
#### Sample
```
    https://ideabiz.lk/apicall/subscription/v3/status/ tel%3A%2B94766691500
```
#### Method
```
    GET
```
#### Response Body
```
{
      "statusCode": "SUCCESS",
      "message": "",
      "data": {
        "subscribeResponse": 
            {
                "msisdn": "94766691500",
                "status": "NOT_SUBSCRIBED",
                "serviceID": null
        }
      }
}

```
## Multiple Service ID Check 
#### URL

```
    https://ideabiz.lk/apicall/subscription/v3/status/
```
When app have multiple service ids, please use bellow method  to check status
#### Sample
```
    https://ideabiz.lk/apicall/subscription/v3/status/
```
#### Method
```
    POST
```
#### Request Body
```
{

"msisdn":"94777339033",

"serviceID": "xxxxxxxxxxxxxxxxxx-xxx-xxxx-xx"

}
```


#### Response Body
```
{
      "statusCode": "SUCCESS",
      "message": "",
      "data": {
        "subscribeResponse": 
            {
                "msisdn": "94766691500",
                "status": "NOT_SUBSCRIBED",
                "serviceID": "xxxxxxxxxxxxxxx-xxx-xxxx-xx"
        }
      }
}
```
## Get Subscriber Base
#### URL

```
    https://ideabiz.lk/apicall/subscription/v3/info/currentBase
```
#### Method
```
    GET
```
#### Response Body
```
{
     "statusCode": "SUCCESS",
     "data": {
        "currentBase": 5       
              }
}
```
## Get Daily User Info
#### URL

```
    https://ideabiz.lk/apicall/subscription/v3/info/daily/{date}
```
#### Sample
```
    https://ideabiz.lk/apicall/subscription/v3/info/daily/2017-07-11
```
#### Method
```
    GET
```
#### Response Body
```
{
    "statusCode": "SUCCESS",
    "data": [
        {
            "status": "UNSUBSCRIBED",
            "count": 1
        }
    ]
}

```
## Admin Notify URL

Please refer the Admin API documentation
+ When the user’s state changes, the following JSON payload will be sent to your Admin Notify URL.

#### Request Body
```
{
      "action" : "STATE_CHANGE",
      "method" : "WEB",
      "msisdn" : "tel:+94766691500",
      "appID"  : "APP001",
      "status" : "SUBSCRIBED"
    "serviceID" : "SVC_001"
    }
```
## Subscription Status

| Status | Description |
| ------ | ------ | 
| SUBSCRIBED | Subscription success |
| NOT_HOME_NETWORK | Number not in home network |
| WRONG_FORMAT | Number is in wrong format |
| ALREADY_SUBSCRIBED | Number already subscribed |
| NOT_SUBSCRIBED | Number not subscribed for application |

## Error Responses

| Error Responses | Action to be taken |
| ------ | ------ | 
| Application Suspended | Contact the Developer Support Team |
| Application Terminated | Contact the Developer Support Team |
| MSISDN Blacklisted all services | This user has been blacklisted for all your services. You cannot subscribe any services for this number|
| MSISDN Blacklisted for your application | This user has been blacklisted for your service. You cannot subscribe your application for this number |

**Sample Error Response for "MSISDN Blacklisted all services":**
```
{
      "statusCode": "ERROR",
      "message": "MSISDN Blacklisted for all services",
      "data": null
}
```

**Sample Error Response for "MSISDN Blacklisted your application":**
```
{
      "statusCode": "ERROR",
      "message": "MSISDN Blacklisted for your application",
      "data": null
}
```

## Rental Callback

**Sample response for success Rental Callback:**
```
{ 
		"action" : "STATE_CHANGE", 
		"method" : "RENTAL", 
		"msisdn" : "tel:+94766691500", 
		"appID" : "APP001", 
		"status" : "RENTAL_CHARGED", 
		"serviceID" : "SVC_001"
}
```

**Sample response for Fail Rental Callback:**
```
{ 
		"action" : "STATE_CHANGE", 
		"method" : "RENTAL", 
		"msisdn" : "tel:+94766691500", 
		"appID" : "APP001", 
		"status" : "RENTAL_FAILED", 
		"serviceID" : "SVC_001" 
}
```