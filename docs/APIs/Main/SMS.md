

## Content

* [Overview](#overview)
* [Method](#method)
* [Requirements](#requirements)
* [Sending SMS](#sending-sms)
* [Recieving SMS](#receiving-sms)
* [Delivery Notifications](#delivery-notifications)
* [Delivery Report](#delivery-report)
* [Response Codes](#response-codes)
* [Exceptions](#exceptions)
* [Faults](#faults)
* [Libraries and Samples](#libraries-and-samples)
 

## Overview
The Dialog SMS API allows an application to send and receive SMS messages. These services are accessible via RESTful web services.</br>Standard SMS messages (English, Alpha Numeric) are limited to 160 characters per message. If a message exceeds this limit, it is broken up into multiple segments of 160 characters each, and transmitted through the network,
</br>If SMS is sent/received as a Unicode string(Special characters & non-English text), such SMS are limited to approx.70 characters, If a message exceeds this limit, it is broken up into multiple segments of 70 characters each, and transmitted through the network,</br>Note that the Operator or the API platform does not block any characters that exceed this length. It is advised to adhere to these limits within your application. (operator charges are applied per SMS transmitted and not per API call)


### Method

The following REST methods are available: 

 * Send an SMS from your Web Application 
 * Query the Delivery Status of an SMS
 * Retrieve SMS Sent to your Web Application (which is identified by the registrationId)
 
## Requirements</br>

**Authorization API Calls**</br>
All API call requests to ideabiz.lk require Authorization headers. Please refer the *Token Management* (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) document for Authorization.

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

If sending a unicode string with the message, please set `Content-Type` charset to `UTF-8`

```
Content-Type: application/json;charset=UTF-8
```
</br> Note: If sending Unicode string, such string is broken up into multiple segments of approx.70 characters each, and transmitted through the network as multiple SMS.

### Encrypted MSISDN</br> 

SMS API is compatible with Encrypted MSISDN</br>
Please refer the *Secure Header* (http://docs.ideabiz.lk/APIs/Header_Enrichment) document for encrypted MSISDNs.


## Sending SMS

This allows you to send an SMS from your Web application to one or more addresses (MSISDNs).</br> Even though several MSISDNs can be included in one API call, they will be transmitted through the network as separate SMS. 

#### **Request**

Given below is a sample request of the send service.

#### URL
```
https://ideabiz.lk/apicall/smsmessaging/{version}/outbound/{port}/requests
```

#### Sample
```
https://ideabiz.lk/apicall/smsmessaging/v3/outbound/87798/requests
```

#### Method
```
POST
```
 
#### Body
```
{"outboundSMSMessageRequest": {
        "address": [
            "tel:+ 94766691500"
        ],
        "senderAddress": "tel:87711",
        "outboundSMSTextMessage": {
            "message": " Test SMS "
        },
        "clientCorrelator": "123456",
        "receiptRequest": {
            "notifyURL": "http://138.189.164.230:1080/sms/report",
            "callbackData": "some-data-useful-to-the-requester"
        },
        "senderName": "ABCCo"
    }
}
```
### States of Parameters</br>
States of the Request parameters of send service.
<br>
<table border="1">
	<thead>
		<tr>
			<th>
			<p><strong>Parameter Name</strong></p>
			</th>
			<th>
			<p><strong>Description</strong></p>
			</th>
			<th>
			<p><strong>Type</strong></p>
			</th>
			<th>
			<p><strong>Mandatory /Optional</strong></p>
			</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>
			<p>address</p>
			</td>
			<td>
			<p>At least one address must be provided.</br></br>In this case it is the recipients MSISDN including the "tel:" protocol identifier and the country code preceded by "+".i.e., tel:+94766691500.</br></br>OneAPI also supports the Anonymous Customer Reference (ACR) if provided by the operator.
(However ACR is not supported in either versions of the SMSmessaging API)
.</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>senderAddress</p>
			</td>
			<td>
			<p>This is the Port number configured for you, by the ideabiz Support team.</br></br>This is also the address to which, the SMS recipient may send a reply SMS. (Unless the senderAddress is hidden by the use of senderName.(See below))</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>message</p>
			</td>
			<td>
			<p>Must be provided within the outboundSMSTextMessage element. Messages over 160 characters may end up being sent as two or more messages by the operator.</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>clientCorrelator</p>
			</td>
			<td>
			<p>Uniquely identifies this SMS request. If there is a communication failure during the request, using the same clientCorrelator, when retrying, the request allows the operator to avoid sending the same SMS twice.</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>notifyURL</p>
			</td>
			<td>
			<p>The URL to which you would like to receive a notification of delivery of SMS.</br></br>The format of this notification is shown below.</br>(See [Delivery Notifications](#delivery-notifications))</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Optional.Keep empty or null if you don't want delivery notifications.</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>callbackData</p>
			</td>
			<td>
			<p>If Delivery Notifications were requested, this string will be passed back, so you can use it to identify the message, the receipt notification relates to (or any other useful data, such as a function name).</br></br>This is only valid if notifications were requested. (See the notifyURL above)</p>
			</td>
			<td>
			<p>&nbsp;</p>
			</td>
			<td>
			<p>Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>senderName</p>
			</td>
			<td>
			<p>Use this only if you want to show a number other than the senderAddress for the recipient to respond to, or if you want to show an Alphanumeric port (Mask) in the SMS recipients phone.</br></br>If this is kept blank, senderAddress will be used in its place.</br></br>A Mask (Maximum 11 Char) has to be approved & configured by the ideabiz Support team before it can be used.</br></br>Mask cannot contain special characters such as ~!@#$%^&*_=+-()`;:'"/?.,<></p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Optional</p>
			</td>
		</tr>
	</tbody>
</table>

<br>

#### **Response**</br>
Given below is a sample response of the send service.

#### Body
```
{
  "outboundSMSMessageRequest": {
    "deliveryInfoList": {
      "deliveryInfo": [
        {
          "address": "tel:+94766691500 ",
          "deliveryStatus": "MessageWaiting",
          "messageReferenceCode": "D-O-42-12f93c0e5b1f470cb03947096f01fef9-1455613873"
        }
      ],
      "resourceURL": "https://ideabiz.lk/apicall/smsmessaging/v2/outbound/94766691500 /requests/D-O-42-12f93c0e5b1f470cb03947096f01fef9"
    },
    "serverReferenceCode": "D-O-42-12f93c0e5b1f470cb03947096f01fef9",
    "address": [
      "tel:+94766691500"
    ],
    "senderAddress": "tel:87711",
    "clientCorrelator": "123456",
    "senderName": "ABCCo",
    "outboundSMSTextMessage": {
      "message": " Test SMS "
    },
    "receiptRequest": {
      "notifyURL": "http://138.189.164.230:1080/sms/report",
      "callbackData": "some-data-useful-to-the-requester"
    }
  }
}
```
 

## Receiving SMS

This allows you to retrieve any SMS that have been sent to the port assigned to your Web application. Once your port or port:keyword receives an SMS, it will be pushed to your application as a JSON request.

<pre>
{
  "inboundSMSMessageNotification": {
    "callbackData": "callbackdata",
    "inboundSMSMessage": {
      "dateTime": "2016-01-01 03:00:00",
      "destinationAddress": "tel:+87711",
      "messageId": "D-I-42-12f93c0e5b1f0cb03947096f01fef",
      "message": "Test SMS",
      "senderAddress": "94766691500"
    }
  }
}
</pre>
 
 
## Delivery Notifications

When you send the [SMS API Call](#sending-sms), you can mention a callback/notify URL. Delivery notifications will be pushed to the mentioned URL once the SMS is delivered.
 
<pre>
{
  "deliveryInfoNotification": {
    "callbackData": "some-data-useful-to-the-requester",
    "serverReferenceCode": "D-O-46-39524b230e3c4da9b95eb0053f7f",
    "messageReferenceCode": "D-O-46-39524b230e3c4da9b95eb0053f7f-40692",
    "senderAddress": "tel:+87711",
    "deliveryInfo": {
      "operatorCode": "DIALOG",
      "filterCriteria": "",
      "address": "tel:+ 94766691500 ",
      "deliveryStatus": "DeliveredToNetwork"
    }
  }
}
</pre>
 
 <!--
### Subscribing for a keyword

##### Request

Following is a sample request of subscribing for a keyword.
<pre>
{
 "subscription": {
 "callbackReference": {
 "callbackData": "doSomething()",
 "notifyURL": "http://128.199.174.220/ideamart/test/"
 },
 "criteria": "malinda",
 "destinationAddress": "26451",
 "notificationFormat": "JSON",
 "clientCorrelator": "12345"
 }
}
</pre>
##### Response

Following is a sample response of subscribing for a keyword.
 <pre>
{
 "subscription": {
 "callbackReference": {
 "callbackData": "doSomething()",
 "notifyURL": "http://128.199.174.220/ideamart/test/"
 },
 "criteria": "malinda",
 "destinationAddress": "26451",
 "notificationFormat": "JSON",
 "clientCorrelator": "12345",
 "resourceURL": "https://apistore.dialog.lk/apicall/smsmessaging/1.0/inbound/subscriptions/sub352"
 }
}
</pre>
 
### Delete Subscription

##### URL
<pre>
https://apistore.dialog.lk/apicall/smsmessaging/1.0/inbound/subscriptions/sub352
</pre>

##### Method
<pre>
DELETE
</pre>


### Reading SMS

##### URL
<pre>
https://ideabiz.lk/apicall/smsmessaging/v1/inbound/registrations/26451/messages?maxBatchSize=2
</pre>

##### Method
<pre>
GET
</pre>

##### Response
<pre>
{
 "inboundSMSMessageList": {
 "inboundSMSMessage": [
 {
 "dateTime": "1412569381451",
 "destinationAddress": "26451",
 "message": "Uuu",
 "resourceURL": "Not Available",
 "senderAddress": "94771234567"
 }
 ],
 "numberOfMessagesInThisBatch": "1",
 "resourceURL": "Not Available",
 "totalNumberOfPendingMessages": "1"
 }
}
</pre>
-->


## Response Codes

200 – Success! <br>
400 – Bad request; check the error message for details <br>
401 – Authentication failure, check your authentication details <br>
403 – Forbidden; please provide authentication credentials <br>
404 – Not found: mistake in the host or path of the service URI <br>
405 – Method not supported: for example you mistakenly used a HTTP GET instead of a POST<br>
500 – The server encountered an unexpected condition. This could be wrong authentication details or limited user permission<br>
503 – Server busy and service unavailable. Please retry the request.

## Exceptions
##### Types <br>
##### Policy Exception_ <br>
Message Id starts with<code> PL</code>

##### Server Exception_

Message Id starts with <code>SV </code>

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
HTTP Respose code <code> 400 </code>
#### Fault Response Body
<pre>
{
Invalid input value for message part %1
}
</pre>

HTTP Respose code <code> 400 </code>
#### Fault Response Body
<pre>
{
    "requestError": {
        "serviceException": {
            "messageId": "POL7101",
            "text": "Mask not allowed to current application. Mask is %1",
            "variables": "%1 - Test"
        }
    }
}
</pre>

HTTP Respose code <code> 500 </code>
#### Fault Response Body
<pre>
{
    "requestError": {
        "serviceException": {
            "messageId": "SVC7109",
            "text": "URL param port and sender address in request body is not match. %1 - URL param port, %2 - sender address in request body ",
            "variables": "%1 - 87798, %2 - tel:87711"
        }
    }
}
</pre>

HTTP Respose code <code> 500 </code>
#### Fault Response Body
<pre>
{  
   "deliveryInfoNotification":{  
      "callbackData":"CB1525108371514-560212557",
      "deliveryInfo":{  
         "address":"tel:+94778395180",
         "deliveryStatus":"DeliveredToNetwork",
         "operatorCode":"DIALOG",
         "filterCriteria":"?"
      }
   }
}
</pre>

HTTP Respose code <code> 503 </code>
#### Fault Response Body
<pre>
{
    "fault": {
        "code": "900800",
        "message": "Message Throttled Out",
        "description": "You have exceeded your quota"
    }
}
</pre>

## Libraries and Samples

* Java sample sms app (https://github.com/ideabizlk/Sample-JAVA-SMS-API)
* Java SMS handler that can use to send and read sms (https://github.com/ideabizlk/IdeaBiz-SMS-API-Handler-JAVA)
* Java Request handler that handle oAuth authentication (https://github.com/ideabizlk/IdeaBiz-Request-Handler-JAVA)
* PHP Request handler that handle oAuth authentication (https://github.com/ideabizlk/IdeaBiz-Request-Handler---PHP)
* C Sharp Request handler that handle oAuth authentication (https://github.com/ideabizlk/IdeaBiz-Request-Handler-CSharp)
* NodeJS Request handler that handle oAuth authentication (https://github.com/ideabizlk/IdeaBiz-Request-Handler---NodeJS)
* Java Class that inclue SMS classes (https://github.com/ideabizlk/Classes-JAVA)

