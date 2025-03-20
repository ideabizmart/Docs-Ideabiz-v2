## Content
* [Overview](#overview)
* [Method](#method)
* [Requirements](#requirements)
* [Sending Subscription Request](#sending-the-subscription-request)
* [Status Notifications](#status-notifications)
* [Response Codes](#response-codes)

## Overview
The Subscription Manager API allows applications to create their own subscriptions. However prior to being allowed access to the API, certain configurations need to be made. In order to make these configurations a callback URL is necessary in order to notify when the status of the subscription changes.

## Requirements</br>
**Subscription Manager Configurations**</br>
The following parameters are required in order to create a subscription manager record:
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
			<p>callbackURL</p>
			</td>
			<td>
			<p>The subscription name, it should be unique per application(i.e One application cannot contain two subscriptions of the name "Subscription", the API will reject this request).</p>
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
			<p>SMS Port</p>
			</td>
			<td>
			<p>This is the Port number configured for you, by the ideabiz Support team.</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory, if you require SMS subscriptions</p>
			</td>
		</tr>
			<tr>
			<td>
			<p>USSD Port</p>
			</td>
			<td>
			<p>The USSD Port number configured for you, by the ideabiz Support team.</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory, if you require USSD subscriptions</p>
			</td>
		</tr>
		
	</tbody>
</table>

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

## Sending the subscription request

This allows you to send an subscription request from your Web application.

#### **Request**

Given below is a sample request:

#### URL
```
https://ideabiz.lk/apicall/submn/v1.0/new
```

#### Sample
```
https://ideabiz.lk/apicall/submn/v1.0/new
```

#### Method
```
POST
```
 
#### Body
```
{
  "subscriptionName": "New Subscription",
  "subscriptionDescription": "Subscription description",
  "regSms": "You've been successfully subscribed to New Subscription", 
  "unregSms": "You have successfully unsubscribed from New Subscription",
  "senderName": "IdeabizLK",
  "subscriptionUrl": "https://apps.ideabiz.lk/notify", 
  "operator": "Dialog",
  "apiVersion": "V3",
  "defaultConfig": true,
  "comment": "comment" 
}
```

#### **Request for SMS (Subscription SMS)**

Given below is a sample request:

#### URL
```
https://ideabiz.lk/apicall/submn/v1.0/new
```

#### Sample
```
https://ideabiz.lk/apicall/submn/v1.0/new
```

#### Method
```
POST
```
 
#### Body
```
{
  "subscriptionName": "New Subscription",
  "subscriptionDescription": "Subscription description",
  "regSms": "You've been successfully subscribed to New Subscription", 
  "unregSms": "You have successfully unsubscribed from New Subscription",
  "senderName": "IdeabizLK",
  "subscriptionUrl": "https://apps.ideabiz.lk/notify", 
  "operator": "Dialog",
  "apiVersion": "V3",
  "defaultConfig": true,
  "comment": "comment",
  "subscriptionSms":{
    "smsRegCode": "REG APP",
    "smsUnregCode": "UNREG APP",
    "smsNotifyUrl": "https://apps.ideabiz.lk/notify",
    "keyword": "HEY",
    "operator":"Dialog"
  } 
}
```

#### **Request for USSD (Subscription USSD)**

Given below is a sample request:

#### URL
```
https://ideabiz.lk/apicall/submn/v1.0/new
```

#### Sample
```
https://ideabiz.lk/apicall/submn/v1.0/new
```

#### Method
```
POST
```
 
#### Body
```
{
  "subscriptionName": "New Subscription",
  "subscriptionDescription": "Subscription description",
  "regSms": "You've been successfully subscribed to New Subscription", 
  "unregSms": "You have successfully unsubscribed from New Subscription",
  "senderName": "IdeabizLK",
  "subscriptionUrl": "https://apps.ideabiz.lk/notify", 
  "operator": "Dialog",
  "apiVersion": "V3",
  "defaultConfig": true,
  "comment": "comment",
  "subscriptionUssd":{
    "ussdRegKeyword":  456 ,
    "ussdUnregKeyword": 123,
    "ussdWelcomeMsg":  "Welcome!",
    "ussdUnregMsg": "Thank you for using the service!",
    "operator":"Dialog",
    "ussdNotifyUrl": "https://apps.ideabiz.lk/notify"
  }
}
```

#### **Request for Charging (Payment V4)**

Given below is a sample request:

#### URL
```
https://ideabiz.lk/apicall/submn/v1.0/new
```

#### Sample
```
https://ideabiz.lk/apicall/submn/v1.0/new
```

#### Method
```
POST
```
 
#### Body
```
{
  "subscriptionName": "New Subscription",
  "subscriptionDescription": "Subscription description",
  "regSms": "You've been successfully subscribed to New Subscription", 
  "unregSms": "You have successfully unsubscribed from New Subscription",
  "senderName": "IdeabizLK",
  "subscriptionUrl": "https://apps.ideabiz.lk/notify", 
  "operator": "Dialog",
  "apiVersion": "V3",
  "defaultConfig": true,
  "comment": "comment",
  "paymentV4Config":{
     "maxAmount":500
  }
}
```

#### **Request for Pin Subscription (Pin Subscription )**

Given below is a sample request:

#### URL
```
https://ideabiz.lk/apicall/submn/v1.0/new
```

#### Sample
```
https://ideabiz.lk/apicall/submn/v1.0/new
```

#### Method
```
POST
```
 
#### Body
```
{
    "subscriptionName": "New Subscription",
    "subscriptionDescription": "Subscription description",
    "regSms": "You've been successfully subscribed to New Subscription",
    "unregSms": "You have successfully unsubscribed from New Subscription",
    "senderName": "IdeabizLK",
    "subscriptionUrl": "https://apps.ideabiz.lk/notify",
    "operator": "Dialog",
    "apiVersion": "V3",
    "defaultConfig": true,
    "comment": "comment",
    "pinSubscriptionConfig": {
        "allowedPrefix": "9477;9476;",
        "senderName": "ideabizLK",
        "pinLen": 6,
        "pinMsg": "<#> Your PIN is ##PIN## for ##APPNAME##  KEY"
    }
}
```

#### **Request with all configurations**

Given below is a sample request:

#### URL
```
https://ideabiz.lk/apicall/submn/v1.0/new
```

#### Sample
```
https://ideabiz.lk/apicall/submn/v1.0/new
```

#### Method
```
POST
```
 
#### Body
```
{
    "subscriptionName": "New Subscription",
    "subscriptionDescription": "Subscription description",
    "regSms": "You've been successfully subscribed to New Subscription",
    "unregSms": "You have successfully unsubscribed from New Subscription",
    "senderName": "IdeabizLK",
    "subscriptionUrl": "https://apps.ideabiz.lk/notify",
    "operator": "Dialog",
    "apiVersion": "V3",
    "defaultConfig": true,
    "comment": "comment",
    "subscriptionSms": {
        "smsRegCode": "REG APP",
        "smsUnregCode": "UNREG APP",
        "smsNotifyUrl": "https://apps.ideabiz.lk/notify",
        "keyword": "HEY",
        "operator": "Dialog"
    },
    "subscriptionUssd": {
        "ussdRegKeyword": 456,
        "ussdUnregKeyword": 123,
        "ussdWelcomeMsg": "Welcome!",
        "ussdUnregMsg": "Thank you for using the service!",
        "operator": "Dialog",
        "ussdNotifyUrl": "https://apps.ideabiz.lk/notify"
    },
    "paymentV4Config": {
        "maxAmount": 500
    },
    "pinSubscriptionConfig": {
        "allowedPrefix": "9477;9476;",
        "senderName": "ideabizLK",
        "pinLen": 6,
        "pinMsg": "<#> Your PIN is ##PIN## for ##APPNAME##  KEY"
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
			<p>subscriptionName</p>
			</td>
			<td>
			<p>The subscription name, it should be unique per application(i.e One application cannot contain two subscriptions of the name "Subscription", the API will reject this request).</p>
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
			<p>subscriptionDescription</p>
			</td>
			<td>
			<p>A brief description of the subscription.</p>
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
			<p>regSms</p>
			</td>
			<td>
			<p>SMS sent once the user subscribes to the subscription/service.</p>
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
			<p>unregSms</p>
			</td>
			<td>
			<p>SMS sent once the user unsubscribes from the subscription/service.</p>
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
		<tr>
			<td>
			<p>subscriptionUrl</p>
			</td>
			<td>
			<p>Notification URL for subscriptions</p>
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
			<p>operator</p>
			</td>
			<td>
			<p>Operator(Dialog)</p>
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
			<p>comment</p>
			</td>
			<td>
			<p>Any comments to be noted by the support team/business team.</p>
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
			<p>SMS Reg Code</p>
			</td>
			<td>
			<p>SMS sent once the user subscribes to the subscription/service.</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory, if you require SMS subscriptions</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>SMS Unreg Code</p>
			</td>
			<td>
			<p>SMS sent once the user unsubscribes from the subscription/service.</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory, if you require SMS subscriptions</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>SMS Notify URL</p>
			</td>
			<td>
			<p>The URL to which you would like to receive a notification of delivery of SMS.</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory, if you require SMS subscriptions</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>Keyword</p>
			</td>
			<td>
			<p>SMS Keyword</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory, if you require SMS subscriptions</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>operator</p>
			</td>
			<td>
			<p>Operator(Dialog)</p>
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
			<p>USSD Reg Keyword</p>
			</td>
			<td>
			<p>USSD keyword to register to the subscription.(When a short code/port is shared among multiple applications, the keyword should be used to uniquely identify the application.NOTE: This parameter can be empty if the short code is unique for an application)</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory, if you require USSD subscriptions</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>USSD Unreg Keyword</p>
			</td>
			<td>
			<p>USSD keyword to unregister from the subscription.</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory, if you require USSD subscriptions</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>USSD Notify URL</p>
			</td>
			<td>
			<p> If the user responds to the USSD menu, the response should be forwarded to this URL. </p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory, if you require USSD subscriptions</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>USSD Welcome Message</p>
			</td>
			<td>
			<p> The welcome message which is displayed once the user is subscribed. </p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory, if you require USSD subscriptions</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>USSD Unreg Message</p>
			</td>
			<td>
			<p> The Unreg message which is displayed once the user is unsubscribed. </p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory, if you require USSD subscriptions</p>
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
			<p>Mandatory, if you require Pin Subscription</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>allowedPrefix</p>
			</td>
			<td>
			<p> The prefixes which are allowed to be subscribed to the service.It should contain the country code along with the operator code(Eg:9477;9476;).After each code there should be a semi-colon(;).</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory, if you require Pin Subscription</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>pinLen</p>
			</td>
			<td>
			<p>The length of the PIN sent to the user. It's usually set at 6.</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory, if you require Pin Subscription</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>pinMsg</p>
			</td>
			<td>
			<p>If you require a customized pin message, send this in the request body.As seen in the example above (<#> Your PIN is ##PIN## for ##APPNAME##  KEY) the placeholders for PIN and APPNAME are mandatory. The KEY is your Google OTP hash.</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Optional, if you require Pin Subscription</p>
			</td>
		</tr>
	</tbody>
</table>

<br>

<br>

## Response Codes

200 – Success! <br>
400 – Bad request; check the error message for details <br>
401 – Authentication failure, check your authentication details <br>
403 – Forbidden; please provide authentication credentials <br>
404 – Not found: mistake in the host or path of the service URI <br>
405 – Method not supported: for example you mistakenly used a HTTP GET instead of a POST<br>
500 – The server encountered an unexpected condition. This could be wrong authentication details or limited user permission<br>
503 – Server busy and service unavailable. Please retry the request.

