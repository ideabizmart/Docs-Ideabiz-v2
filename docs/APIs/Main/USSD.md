
# ussd-v1/v3

## Content

 * [Initiating mobile terminated USSD](#initiating-mobile-terminated-ussd)
 * [Receive mobile originated USSD](#receive-moblie-originated-ussd)
 * [Response Codes & Exceptions](#response-codes-&-exceptions)
 * [Exceptions](#exceptions)
 * [Service Exceptions](#service-exceptions)
 * [Policy Exceptions](#policy-exceptions)
 

### Method

The following REST methods are available <br>
* Intiate mobile terminated USSD.
* Receive mobile originated USSD.
* Stop subscription to notifications of MO USSD.

## Authorization API calls
All API call request to ideabiz.lk required Authorization headers. Please refer (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) for Authorization

##### Request Header
<pre>
```
Content-Type: application/json
Authorization: Bearer [access token]
Accept: application/json
```
</pre>

### Initiating Mobile Terminated USSD (NI USSD/MT)

Initiates a USSD session with the intended end user. This request would pop up a USSD menu on the end user's device.

#### Request

Following is a sample request of initiating mobile terminated USSD.

##### URL
```sh
https://ideabiz.lk/apicall/ussd/{version}/outbound/{Subscriber Number}
```

##### Method
```sh
POST
```
##### Body
```sh
{
    "outboundUSSDMessageRequest": {
    "address": "tel:+94777123456",
    "shortCode": "tel:1234",
    "keyword": "123",
    "outboundUSSDMessage": "Login to service?\n1. Ok\n2. Cancel",
    "clientCorrelator": "123456",
    "responseRequest": {
      "notifyURL": "http://ussd.response.receive.url ",
      "callbackData": "some-data-useful-to-the-requester"
    },
    "ussdAction": "mtinit"
  }
}
```
Following are the request parameters of initiating mobile terminated USSD. 

| **Parameter name** | **Description** | **Usage** |
| --- | --- | --- |
| address | The recipients MSISDN including the ‘tel:’ protocol identifier and the country code preceded by ‘+’. i.e., tel:+94770000000. | Mandatory |
| shortCode | The short assigned to an application. End users dialling this short code should be routed to the corresponding application. NOTE: The short code can be unique to an application or can be shared my multiple applications | Mandatory |
| keyword | When a short code is shared among multiple applications, the keyword should be used to uniquely identify the application. NOTE: This parameter can be empty if the short code is unique for an application | Optional |
| outboundUSSDMessage | (string) The message to be displayed on the end user device. | Mandatory |
| clientCorrelator | (string) uniquely identifies this request. If there is a communication failure during the request, using the same clientCorrelator when retrying the request allows the operator to avoid sending duplicate messages to the end user. | Mandatory |
| ussdAction | The desired USSD action, valid actions are; **mtinit:** Initiate MT USSD session **mtcont:** Continue a MT USSD session **moinit:** An USSD session initiated by an end user. **mocont:** Continue a MO USSD session **mtfin:** End a USSD Session | Mandatory |
| notifyURL | (string) If the user responds to the USSD menu, the response should be forwarded to this URL. NOTE: If this parameter is not set, a subscription to receive MO USSD should be created. If either notifyURL or subscription to receive MO is created, the application has no way of receiving end user input. | Optional |
| callbackData | (string) will be passed back in this notification. It’s used to identify the message the receipt relates to (or any other useful data, such as a function name). This is only valid if notifications are required – sent with the notifyURL parameter within the responseRequest element. | Optional |

<br>


 

#### Response

Following is a sample response of initiating mobile terminated USSD.
```sh
{
    "outboundUSSDMessageRequest": {
    "address": " tel:+94777123456",
    "keyword": "123",
    "shortCode": "tel:1721",
    "outboundUSSDMessage": " Login to service?\n1. Ok\n2. Cancel ",
    "clientCorrelator": "123456",
    "responseRequest": {
      "notifyURL": "http://ussd.response.receive.url ",
      "callbackData": "some-data-useful-to-the-requester"
    },
    "ussdAction": "mtinit",
    "deliveryStatus": "SENT"
  }
}
```
#### Response Parameters

All the parameters of the original request should be relayed back with the addition of the deliveryStatus parameter.


| **Parameter Name** | **Description** | **Usage** |
| --- | --- | --- |
| deliveryStatus | The status of the NI USSD command should be sent back in this field. If the request was accepted by the underlying USSD platform, then the value of the parameter will be set to “SENT”. | Mandatory |

<br>

#### Receive Response

If the notification URL was set in the responseRequest object in outboundUSSDMessageRequest (Refer Initiating Mobile Terminated USSD (NI USSD/MT)) or a subscription to receive MO USSD was created, then the end users response should be posted to the notification URL.
```sh
 {

    "inboundUSSDMessageRequest": {
    "address": " tel:+94777123456",
    "shortCode": "tel:1721",
    "keyword": "123",
    "inboundUSSDMessage": "2",
    "clientCorrelator": "123456",
    "sessionID": "XXXXXX",
    "responseRequest": {
      "notifyURL": "http://ussd.response.receive.url ",
      "callbackData": "some-data-useful-to-the-requester"
    },
    "ussdAction": "mtcont"
  }
}
```
#### Receive Response Parameters

| **Parameter name** | ** Description ** | ** Usage ** |
| --- | --- | --- |
| address | The recipients MSISDN including the ‘tel:’ protocol identifier and the country code preceded by ‘+’. i.e., tel:+94770000000. | Mandatory |
| sessionID | A unique identifier for the session should be returned. Any subsequent communication between MIFE and the application, should contain this session ID. | Mandatory |
| shortCode | The short assigned to an application. End users dialling this short code should be routed to the corresponding application. NOTE: The short code can be unique to an application or can be shared my multiple applications | Mandatory |
| keyword | When a short code is shared among multiple applications, the keyword should be used to uniquely identify the application. NOTE: This parameter can be empty if the short code is unique for an application | Optional |
| inboundUSSDMessage | (string) The response message provided by the end user. | Mandatory |
| clientCorrelator | (string) The clientCorrelator of the original request should be set here. | Mandatory |
| ussdAction | The desired USSD action, valid actions are; **mtinit:** Initiate MT USSD session **mtcont:** Continue a MT USSD session **moinit:** An USSD session initiated by an end user **mocont:** Continue a MO USSD session **mtfin:** End a USSD Session | Mandatory |
| responseRequest |  |  |
| notifyURL | (string) If the user responds to the USSD menu, the response should be forwarded to this URL. | Optional |
| callbackData | (string) Will be passed back in this notification. It’s used to identify the message the receipt relates to (or any other useful data, such as a function name). This is only valid if notifications are required – sent with the notifyURL parameter within the responseRequest element. | Optional |

<p>&nbsp;</p>

<br>


### Continue USSD Session

Continue an USSD session with the intended end user.

##### Method
```sh
POST
```
##### Request
```sh
{
    "outboundUSSDMessageRequest": {
    "address": "tel:+123456789",
    "shortCode": "tel:1234",
    "keyword": "123",
    "outboundUSSDMessage": "Login to service?\n1. Ok\n2. Cancel",
    "clientCorrelator": "123456",
    "responseRequest": {
      "notifyURL": "http://ussd.response.receive.url ",
      "callbackData": "some-data-useful-to-the-requester"
    },
    "ussdAction": "mtcont"
  }
}

```

| **Parameter name** | ** Description ** | ** Usage ** |
| --- | --- | --- |
| address | The recipients MSISDN including the ‘tel:’ protocol identifier and the country code preceded by ‘+’. i.e., tel:+94770000000. | Mandatory |
| shortCode | The short assigned to an application. End users dialling this short code should be routed to the corresponding application. NOTE: The short code can be unique to an application or can be shared my multiple applications | Mandatory |
| keyword | When a short code is shared among multiple applications, the keyword should be used to uniquely identify the application. NOTE: This parameter can be empty if the short code is unique for an application | Optional |
| outboundUSSDMessage | (string) The message to be displayed on the end user device. | Mandatory |
| clientCorrelator | (string) uniquely identifies this request. If there is a communication failure during the request, using the same clientCorrelator when retrying the request allows the operator to avoid sending duplicate messages to the end user. | Mandatory |
| ussdAction | The desired USSD action, valid actions are; mtinit: Initiate MT USSD session mtcont: Continue a MT USSD session moinit: An USSD session initiated by an end user mocont: Continue a MO USSD session mtfin: End a USSD Session | Mandatory |
| responseRequest |  |  |
| notifyURL | (string) If the user responds to the USSD menu, the response should be forwarded to this URL. NOTE: If this parameter is not set, a subscription to receive MO USSD should be created. If either notifyURL or subscription to receive MO is created, the application has no way of receiving end user input. | Optional |
| callbackData | (string) will be passed back in this notification. It’s used to identify the message the receipt relates to (or any other useful data, such as a function name). This is only valid if notifications are required – sent with the notifyURL parameter within the responseRequest element. | Optional |

<p>&nbsp;</p>


#### Response

Refer the Receive Response Message for the response body and description.

 

### Receive Mobile Originated USSD

When an end user dials USSD code, if a subscription to receive MO USSD was created for that code, then the request will be posted to the notification URL.

 
```sh
{
    "inboundUSSDMessageRequest": {
    "address": " tel:+94777123456",
    "shortCode": "tel:1721",
    "keyword": "123",
    "inboundUSSDMessage": "2",
    "clientCorrelator": "123456",
    "responseRequest": {
      "notifyURL": "http://ussd.response.receive.url ",
      "callbackData": "some-data-useful-to-the-requester"
    },
    "ussdAction": "mtcont"
  }
}
```
 #### Receive Response Parameters

| **Parameter name** | ** Description ** | ** Usage ** |
| --- | --- | --- |
| address | The recipients MSISDN including the ‘tel:’ protocol identifier and the country code preceded by ‘+’. i.e., tel:+94770000000. | Mandatory |
| sessionID | A unique identifier for the session should be returned. Any subsequent communication between MIFE and the application, should contain this session ID. | Mandatory |
| shortCode | The short assigned to an application. End users dialling this short code should be routed to the corresponding application. NOTE: The short code can be unique to an application or can be shared my multiple applications | Mandatory |
| keyword | When a short code is shared among multiple applications, the keyword should be used to uniquely identify the application. NOTE: This parameter can be empty if the short code is unique for an application | Optional |
| inboundUSSDMessage | (string) The response message provided by the end user. | Mandatory |
| clientCorrelator | (string) The clientCorrelator of the original request should be set here. | Mandatory |
| ussdAction | The desired USSD action, valid actions are; **mtinit:** Initiate MT USSD session **mtcont:** Continue a MT USSD session **moinit:** An USSD session initiated by an end user **mocont:** Continue a MO USSD session **mtfin:** End a USSD Session | Mandatory |
| responseRequest |  |  |
| notifyURL | (string) If the user responds to the USSD menu, the response should be forwarded to this URL. | Optional |
| callbackData | (string) Will be passed back in this notification. It’s used to identify the message the receipt relates to (or any other useful data, such as a function name). This is only valid if notifications are required – sent with the notifyURL parameter within the responseRequest element. | Optional |

<p>&nbsp;</p>




### Response Codes & Exceptions

##### Response Codes

HTTP response codes should be used to indicate:

200 – Success! <br>
400 – Bad request; check the error message for details <br>
401 – Authentication failure, check your authentication details<br>
403 – Forbidden; please provide authentication credentials<br>
404 – Not found: mistake in the host or path of the service URI<br>
405 – Method not supported: for example you mistakenly used a HTTP GET instead of a POST <br>
500 – The server encountered an unexpected condition. This could be wrong authentication details or limited user permission <br>
503 – Server busy and service unavailable. Please retry the request.
Exceptions<br>

If the transaction is immediately confirmed, the response is displayed as follows:

This section lists the available error codes, the possible reasons why the exception may have occured, and possible solutions.

 

#### Service Exceptions

If the transaction is immediately confirmed, the response is displayed as follows,
```sh
HTTP/1.1404BadRequestAccept: application/json{
  "requestError": {
    "serviceException": {
      "messageId": "SVC0002",
      "text": " Invalid input value for message part %1",
      "variables": " clientCorrelator Value 12345"
    }
  }
}
```

##### Common Service Exceptions


| Exception | Variables |
| --- | --- |
| SVC0001: A service error occurred. Error code is %1 | %1 – explanation of the error |
| SVC0002: Invalid input value for message part %1 | %1 – the part of the request that is invalid |
| SVC0003: Invalid input value for message part %1, valid values are %2 | %1 – message part %2 – list of valid values |
| SVC0004: No valid addresses provided in message part %1 | %1 – message part |
| SVC0005: Correlator %1 specified in message part %2 is a duplicate | %1 – Correlator %2 – message part |
| SVC0006: Group %1 in message part %2 is not a valid group | %1 – identifier for the invalid group %2 – message part |
| SVC0007: Invalid charging information | none |
| SVC0008: Overlapped criteria %1 | %1 Message Part with the overlapped criteria |
| SVC1000: No resources | none |


Following service exceptions may be thrown for SMS specifically:


| Exception | Variables |
| --- | --- |
| SVC0280: Message too long. Maximum length is %1 characters | %1 – number of characters allowed in a message |
| SCV0283: Delivery receipt notification not supported | none |
| SVC0284: Address format is invalid. Expected format is %1 | %1 - "tel:+94771211212" |
| SVC0285: Message Not Delivered %1 | %1 – Errors occurred while sending the request for all the destinations. |

<p>&nbsp;</p>


Following service exceptions may be thrown for Payment specifically:


| Exception | Variables |
| --- | --- |
| SVC0270: Charging operation failed, the charge was not applied | none |
| SVC0271: endUserId format invalid. Expected format is %1 | %1 - "tel:+94771211212" |

<p>&nbsp;</p>

<p>&nbsp;</p>

<p>&nbsp;</p>

<div>&nbsp;</div>

 

#### Policy Exceptions

If a request is invalid, a policy exception is thrown.

POL0001 - Policy error occured.

The above exception may be thrown to indicate a fault relating to a policy associated with the service.

 
```sh
HTTP 403Forbidden{
  "requestError": {
    "policyException": {
      "messageId": "POL0001",
      "text": "A policy error occurred. Error code is maxBatchSize exceeded. The maximum allowed maxBatchSize is %1.",
      "variables": "20"
    }
  }
}
```
Following exceptions may be thrown:


| Exception | Variables |
| --- | --- |
| POL0001: A policy error occurred. Error code is %1 | %1 – explanation of the error |
| POL0002: Privacy verification failed for address %1, request is refused | %1 – address privacy verification failed for |
| POL0003: Too many addresses specified in message part %1 | %1 – message part |
| POL0004: Unlimited notification request not supported | none |
| POL0005: Too many notifications requested | none |
| POL0006: Group specified in message part %1 not allowed | %1 – message part. Note: group means an address which refers to more than one end user |
| POL0007: Nested groups specified in message part %1 not allowed | %1 – message part. Note: group means an address which refers to more than one end user. Groups cannot contain addresses which are themselves groups. |
| POL0008: Charging is not supported | none |
| POL0009: Invalid frequency requested | none |
| POL0010: Requested information unavailable as the retention time interval has expired | none |
| POL0011: Media type not supported | none |
| POL0012: Too many description entries specified in message part %1 | %1 – message part |
| POL0013: Duplicated addresses %1 | %1 – duplicated addresses |
| POL0253: Payment operation refused by user | none |
| POL0254: The amount exceeds the operator limit for a single charge | none |
| POL0255: Address format invalid. Expected format is %1 | %1 - "tel:+94771211212" |
| POL0256: Invalid currency specified. %1 | %1-Check your SLA for valid currency types |
| POL0257: Message not delivered %1 | %1- Request failed. Errors occurred while sending the request for all the destinations |
| POL0299: Unexpected Errors | none |
| POL1000: User has insufficient credit for transaction | none |
| POL1001: The %1 operator charging limit for this user has been exceeded | %1 – the time period for which the charging limit has been reached |
| POL1007: Refunds not supported | none |
| POL1009: User has not been provisioned for %1 | %1 – name of the service |
| POL1010: User has been suspended from %1 | %1 – the name of the service |

<p>&nbsp;</p>


 

 

 