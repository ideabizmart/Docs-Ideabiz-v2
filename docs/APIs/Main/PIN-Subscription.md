

This api allows user to subscribe an MSISDN on demand, upon successful verification of a SMS PIN number . MSISDN will not be subscribed to the application if incorrect PIN is entered.

API Flow is as below
1.`Send Subscribe Request` and capture the server reference `serverRef`
2.`Submit PIN` use the PIN entered by user and the `serverRef` to get subscription  status

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


### Subscribe Request
#### URL
```
https://ideabiz.lk/apicall/pin/subscription/v1/subscribe
```
### Method
```
POST
```


#### Request
```
{
    "method":"AndroidApp",
    "msisdn":"94777339033"
}
```

#### Response
```
{
  "statusCode": "SUCCESS",
  "message": null,
  "data": {
    "status": "PENDING_AUTH",
    "serverRef": "f07927b4030048be99dc051314b5f40e",
    "msisdn": "tel:+94777339033",
    "method": "ANC"
  }
}

```

#### Request for Micro Services (Subscriptions)


#### Request
```
{
    "method":"AndroidApp",
    "msisdn":"94777339033",
	"serviceId" : "xxxxx-xxxx-xxxxxx"
}
```

#### Response
```
{
  "statusCode": "SUCCESS",
  "message": null,
  "data": {
    "status": "PENDING_AUTH",
    "serverRef": "f07927b4030048be99dc051314b5f40e",
    "msisdn": "tel:+94777339033",
	"serviceId" : "xxxx-xxxx-xxxxxxx",
    "method": "ANC"
  }
}

```

### Submit PIN
#### URL
```
https://ideabiz.lk/apicall/pin/subscription/v1/submitPin
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
#### Sample Error Responses
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
```
{
    "statusCode": "ERROR",
    "message": "Max pin sent per MSISDN in 1min reached for  947xxxxxxxx",
    "data": null
}
```
```
{  
   "statusCode":"ERROR",
   "message":"Number not allowed. Blacklisted for all services 947xxxxxxxx",
   "data":null
}
```
#### Insufficient Balance Response
```
{
    "statusCode": "SUCCESS",
    "message": "Subscription Status",
    "data": {
        "status": "INSUFFICIENT_BALANCE",
        "serverRef": "49131c41cb00476b9d29ed52a0b07f51",
        "msisdn": "tel:+94768061510",
        "method": "WTEL: AndroidApp",
        "description": null,
        "pinMeta": null,
        "serviceId": null
    }
}
```
#### Sample Success Response

```
{
  "statusCode": "SUCCESS",
  "message": "Subscription Status",
  "data": {
    "status": "SUBSCRIBED",
    "serverRef": "8f27e17b51de4b64baa6cd5525506516",
    "msisdn": "tel:+94777123456",
    "method": "ANC"
  }
}

```

## Detailed PIN Request

this is same request as above, but include `pinMeta` data. You can send more details about user. It will help Ideabiz team to support for customer queries
#### URL
```
https://ideabiz.lk/apicall/pin/subscription/v1/subscribe
```
### Method
```
POST
```

### Body 

```
{
  "method": "AndroidApp",
  "msisdn": "94777339033",
  "pinMeta": {
    "client": "WEBBROWSER",
    "device": "iPhone",
    "version": "6",
    "appCode": "lk.ideabiz.app.web",
    "headers": "",
    "remoteRef": "m@2.com",
    "utmSource": "http://abc.com/asa",
    "IP": "1.1.1.1"
  }
}
```

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
			<p>client</p>
			</td>
			<td>
			<p>Client type. values will be `WEBBROWSER`,`MOBILEAPP` 
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
			<p>device</p>
			</td>
			<td>
			<p>Type/OS of device `iPhone 6`,`Galaxy S5`,`PC` 
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
			<p>version</p>
			</td>
			<td>
			<p>OS/Device version `Android 6` , `iOS 5`, `Windows 10` 
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
			<p>appCode</p>
			</td>
			<td>
			<p>If you use app in  store, your app  identifier. if you use web browser, web url `lk.ideabiz.app.web` or `https://widget.ideabiz.lk/xxxxx`
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
			<p>headers</p>
			</td>
			<td>
			<p>Any special headers or data. For multiple data, use `;` to seperate
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
			<p>remoteRef</p>
			</td>
			<td>
			<p>If you have username/email for this user in your system or User specific identifier, sent it here.
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
			<p>utmSource</p>
			</td>
			<td>
			<p>If user installed your app via any referal, send it here. 
			<br> How to catch referal in android : https://android-developers.googleblog.com/2017/11/google-play-referrer-api-track-and.html or https://developer.android.com/google/play/installreferrer/library.html
			<br> for web, pass `referal` header to us. 
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
			<p>IP</p>
			</td>
			<td>
			<p>Client/User original IP
.</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory</p>
			</td>
		</tr>
	</tbody>
</table>
