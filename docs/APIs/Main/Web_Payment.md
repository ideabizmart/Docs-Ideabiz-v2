/*
Title: Web Widget-Web Payment
Sort: 12
*/


This allows to make one-time charges using Dialog Payment API. This is recomended for web and mobile apps that sell content, tickets, and ecommerce sites. You simply have to redirect the user to ideabiz payment page.

APP-KEY in the URL will be provided by support team
```
https://widget.ideabiz.lk/web/pay/initiate/{APP-KEY}?desc={Description}&amount{Amount}&request-ref={Refferebce}
```

|APP-KEY  		 | support team will provide this value					|

|Description	 | Description about the specific payment. (URL encoded value) 	|

|amount 		 | Charging amount 									|

|appRef 	 	 | your transaction refference number (URL encoded value) 	|



### Sample URL

```
https://widget.ideabiz.lk/web/pay/initiate/XXXX?desc=Online+Content+payment&amount=1.00&appRef=INV-123
```

If required you can provide a callback URL (to redirect the user ), notify URL,Email or mobile number to SMS
This will be configured by the support team.
If callback URL is there, we will redirect user to callback  URL with `serverRef` param after transaction has completed.
Then you have to make a `Payment-Widget` API call (link) to check the status and validate the transaction

```
https://yourdomain.com/callback?ref=u37dh3-3jdhd73-3hjdhd73-duis
```
if you provide notificationURL,
We will send API call to your notifyURL with status of transaction

If you provice Email address, we will send email to your email with the status
If you share a mobile number, we will send status sms to your mobile for each transaction

## API
You can use `Web-PIN-Payment` API to view status of server reference

```
https://www.ideabiz.lk/store/apis/info?name=Web-PIN-Payment&version=v1&provider=admin&

```

You need to call status API as bellow

```
https://ideabiz.lk/apicall/widget/pin/pay/v1/status/{ref}
```

eg:
```
https://ideabiz.lk/apicall/widget/pin/pay/v1/status/b93494ce5611408a974b3ab5b9fe
```

### Method 
```
GET
```

### Headers

```
Authorization: Bearer {Access Token}
```

Please refer `http://docs.ideabiz.lk/Getting_Started/Token_Manegment` for authorization token management


### BODY

```
{
  "datetime": "2016-12-16 18:19:41.0",
  "lastUpdatedTimestamp": "2016-12-16 18:19:41.0",
  "type": "PAY",
  "doneBy": "PIN",
  "amount": 1,
  "description": "desc",
  "msisdn": null,
  "appRef": "abcd345",
  "serverRef": "b93494ce5611408a974b3ab5b9fe",
  "status": "DONE"
}


```



## Screens of user flow


This is compatible with encrypted header enrichment as well. If you wish to bypass some steps shown above, please discuss with support team for required configurations.


Steps :


![alt text](http://docs.ideabiz.lk/static-images/IMG_3406.PNG " " )

___________________

![alt text](http://docs.ideabiz.lk/static-images/IMG_3408.PNG " ")

________________

![alt text](http://docs.ideabiz.lk/static-images/IMG_3409.PNG " ")

__________________





