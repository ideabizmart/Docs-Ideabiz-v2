## Admin API 
This API will provide the facility to query subscribers of the application and to deactivate if necessary. Every service provider needs to implement an admin API as per below specification and communicate the end point to Dialog IdeaBiz team. This API will be connected with Admin interface for Dialog customer care frontline to query and to take necessary actions as per customer’s request.</br></br>
Through the Admin API URL, notifications can be received whenever subscribers’ statuses change. (eg: Status ‘SUBSCRIBED’ or Status ‘UNSUBSCRIBED’ and the method of subscription). Admin API notifications give you an overall view of all the status changes of subscribers, and functions as a method of cross-checking subscriber status in your application.  

```
Version : 2
Date : 2016-06-26
```

----------

# Sample Codes
### Java classes models
https://github.com/ideabizlk/Ideabiz-adminapp-common-models

### Java Spring Sample service
https://github.com/ideabizlk/Ideabiz-adminapp-remote-api


----------

# API Specification

## Search Subscribers
Searching subscriber in an application
```
Method : POST
Accept : application/json
```

#### Example
```
http://myapp.com/adminapi/
http://myapp.com/adminapi/index.php
```

#### METHOD
```
POST
```

#### Request Body
```
{
  "action" : "STATE_CHECK",
  "msisdn" : "94777123456",
  "serviceID" : "SVC_001",
  "appID" : "APP_001"
}

```

| field  	|Description   				| mandatory  	|
|---|---|---|
|serviceID 	| micro subscription ID 	| Optional 		|
|appID		| application ID in ideabiz	| Mandatory		|

#### Response Body

```
{
  "statusCode": "SUCCESS",
  "message": "",
  "data": {
    "subscription": [{
		"msisdn":"94777123456",
		"appID":"APP_001",
		"serviceID" : "SVC_001",
		"registration-log" :{
			"datetime" : "2016-01-12 12:33:33",
			"method" : "WEB"
		},
		"unregistration-log" :{
			"datetime" : "2016-01-12 12:33:33",
			"method" : "WEB"
		},
		"status":"REGISTERED",
		"microSubscriptions" : 0
	}]
  }
}

```

| field  |Description   | mandatory  |
|---|---|---|
| number  | Subscriber mobile number. Eg: 94766691500 (If Encrypted : 9477-vl%1D%A3%F7%AC%E1%A7%C7%AF) | mandatory  |
|  status |  Current  status<br> UNSUBSCRIBED : User already unsubscribed<br>SUBSCRIBED : User has active subscription | Mandatory  |
| registration  | User registration info | Mandatory  |
| registration -> datetime  | User registration timestamp | Mandatory  |
| registration -> method  | User registration method</br> values : SMS, WebWidget, USSD| Mandatory  |
| unregistration  | User registration info</br>if there is no unregistration information, this should null  | Optional  |
| unregistration -> datetime  | User registration timestamp | Mandatory  |
| unregistration -> method  | User un-registration method</br> values : SMS, WEB, USSD, CC| Mandatory |
| microSubscriptions 	| If the app has micro subscriptions | Optional |
|serviceID 		| If you have multiple subscriptions under one app, this should return service ID  | Optional |




####  History
Similar to the above method of searching a subscriber this will return the history of the subscriber of your application. The difference is pagination is available for history.

#### Request Body

```
{
  "action" : "HISTORY",
  "msisdn" : "94766691500",
  "appID" : "APP001",
  "serviceID" : "SVC_001",
  "offset" : 0,
  "limit" : 10
}
```

#### Response Body

```
{
  "subscriberHistory": {
    "msisdn" : "94766691500",
	"appID" : "APP001",
	"serviceID" : "SVC_001",
	"offset" : 0,
	"limit" : 10
    "history": [
      {
        "datetime": "2014-05-20 10:33:11",
        "trigger": "ADMIN",
        "event": "UNSUBSCRIBE",
        "note": "",
        "status": "SUCCESS",
		"serviceID" : null
      },
      {
        "datetime": "2014-05-20 10:33:11",
        "trigger": "SYSTEM",
        "event": "CHARGING",
        "note": "no funds",
        "status": "FAILED",
		"content" : "LKR 3.00",
		"serviceID" : "SVC_001"
      },
      {
        "datetime": "2014-05-20 10:33:11",
        "trigger": "SYSTEM",
        "event": "SMS",
        "note": "",
		"content" : "Today quote is...",
        "status": "SUCCESS"
		"serviceID" : null
      },
      {
        "datetime": "2014-05-20 10:33:11",
        "trigger": "SUBSCRIBER",
        "event": "SUBSCRIBE",
        "note": "",
        "status": "SUCCESS"
      }
    ]
  }
}
```
| field  |Description   | mandatory  |
|---|---|---|
| number  | Subscriber mobile number. Eg: 94766691500 | Mandatory  |
| offset  | Offset of current response | Mandatory  |
| limit  | Limit of current response  | Mandatory  |
| history  | Last 10 events regarding the specific user<br> This array contains object of history | Mandatory  |
| history -> datetime  | Event timestamp | Mandatory  |
| history -> trigger  | Actor of the trigerred event.<br> Eg : SYSTEM,SUBSCRIBER,ADMIN| Mandatory |
| history -> event  | Event type<br>Eg: SUBSCRIBE, UNSUBSCRIBE, SMS, USSD, CHARGING | Mandatory |
| history -> note  | Any note as string | mandatory  |
| history -> status  | Specific event status<br>Eg : SUCCESS, FAILED | Mandatory  |

#### Not Found

```
{
  "subscription": {
    "number": "94777123456",
    "status": "NOTFOUND"
  }
}
```
| field  |Description   | Mandatory  |
|---|---|---|
| number  | Subscriber mobile number. Eg: 94777123456  | Mandatory  |
|  status |  current  status<br>NOTFOUND : User subscription not found | Mandatory  |



<!---
### Get microsubscription List
if application has multiple internal apps or service, should return information about that

#### METHOD
```
POST
```

#### Request Body
```
{
  "action" : "GET_MICRSUBSCRIPTIONS",
  "msisdn" : "94777123456",
  "serviceID" : null,
  "appID" : "APP_001"
}

```

| field  	|Description   				| Mandatory  	|
|---|---|---|
|serviceID 	| micro subscription ID 	| Optional 		|
|appID		| application ID in ideabiz	| Mandatory		|

#### Found
-->

## Subscribe / unsubscribe user from app or service

Based on the user's state change, we will send a json to your notify URL.</br>
You can cross check the status of the user as per your application records and update accordingly.


#### 1)When a user subscribes to the app/service we will send the following json:

Request Body
```
{  
   "action":"STATE_CHANGE",
   "method":"WEB",
   "msisdn":"tel:+94777123456",
   "appID":"APP001",
   "serviceID":"SVC_001",
   "status":"SUBSCRIBED"
}

```

#### 2)When a user unsubscribes from the app/service we will send the following json:

Request Body
```
{  
   "action":"STATE_CHANGE",
   "method":"WEB",
   "msisdn":"94777123456",
   "appID":"APP001",
   "serviceID":"SVC_001",
   "status":"UNSUBSCRIBED"
}

```


### Security check for incoming API call.

Upon receiving an API call, please validate the IP

### IP Validation

    Source IP : 202.69.200.34
<br>

	
### Rental charging responses
<br>
<b> 1. When user was charged successfully on subscription</b>
<br>
```
{  
   "action":"STATE_CHANGE",
   "method":"RENTAL",
   "msisdn":"766691500",
   "appID":"545",
   "serviceID":"0401f3c2-d2dd-4a25-bb30-4fe4cabbe988",
   "status":"SUBSCRIBE"
}
```

<b> 2. when user was not charged on renewal attempts</b>
<br>
```
{  
   "action":"STATE_CHANGE",
   "method":"RENTAL",
   "msisdn":"766691500",
   "appID":"545",
   "serviceID":"0401f3c2-d2dd-4a25-bb30-4fe4cabbe988",
   "status":"RENTAL_FAILED"
}
```

<b> 3. When user was charged successfully after a period of failed payment/s</b>
<br>
```
{  
   "action":"STATE_CHANGE",
   "method":"RENTAL",
   "msisdn":"766691500",
   "appID":"545",
   "serviceID":"0401f3c2-d2dd-4a25-bb30-4fe4cabbe988",
   "status":"RENTAL_CHARGED"
}
```

<b> 4. When the user got unregistered </b>
<br>
```
{  
   "action":"STATE_CHANGE",
   "method":"RENTAL",
   "msisdn":"766691500",
   "appID":"545",
   "serviceID":"0401f3c2-d2dd-4a25-bb30-4fe4cabbe988",
   "status":"UNSUSCRIBE"
}
```