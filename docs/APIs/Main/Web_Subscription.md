

You can use this widget to register a user to your app over web or mobile app.
Simply redirect user after Registration on your web or mobile app, to the url that is provided below (APP-KEY will be provided by support team). User will provide Registration confirmation on Subscription widget which will manage user Subscription.

`Request-Ref` is optional value that you can pass to dialog end. that value will return in status check API. so you can match which user has returned back.

```
https://widget.ideabiz.lk/web/reg/initiate/{APP-KEY}?request-ref={reference}
```

If you provide a callback URL, we will redirect user to callback  URL with `ref` param after the user is subscribed.(See sample `ref` below)
This will be configured by the support team.
Then you can make a `Subscription-Widget` API call to check the status, validate subscription and get the msisdn of the user (Plain MSISDN or Encrypted MSISDN)


```
https://yourdomain.com/callback?ref=u37dh3-3jdhd73-3hjdhd73-duis

https://yourdomain.com/callback?ref=u37dh3-3jdhd73-3hjdhd73-duis?request-ref=abc

```

If you provide an admin API url, you can recieve an API call whenever the subscription status of a user gets updated. 

## API
You can Subscribe to `Web-PIN-Subscription` API to view status of server reference. API Direct link is as below.

```
https://www.ideabiz.lk/store/apis/info?name=Web-PIN-Subscription&version=v1&provider=admin&

```

You need to call Status API as shown below with the `Ref` that you received.

```
https://ideabiz.lk/apicall/widget/pin/subscription/v1/status/{ref}
```

eg:
```
https://ideabiz.lk/apicall/widget/pin/subscription/v1/status/b93494ce5611408a974b3ab5b9fe
```

### Method 
```
GET
```

### Headers

```
Content-Type: application/json 
Authorization: Bearer [access token] 
Accept: application/json
```

Please refer `http://docs.ideabiz.lk/Getting_Started/Token_Manegment` for authorization token management


### Response 

		{
                datetime: "2017-01-03 17:22:45.0",
                lastUpdatedTimestamp: "2017-01-03 17:22:45.0",
                type: "REG",
                doneBy: "PIN",
                description: null,
                msisdn: "94777339033",
                appRef: "N/A",
				requestRef : "ABC",
                serverRef: "5dd7e7bc-9c28-4b5c-b557-ca11575d6168",
                status: "ALREADY_SUBSCRIBED"
		}
```

```

 
		{
                datetime: "2017-01-03 17:22:45.0",
                lastUpdatedTimestamp: "2017-01-03 17:22:45.0",
                type: "REG",
                doneBy: "PIN",
                description: null,
                msisdn: "94777339033",
                appRef: "N/A",
				requestRef : "ABC",
                serverRef: "5dd7e7bc-9c28-4b5c-b557-ca11575d6168",
                status: "SUBSCRIBED"
		}




## Screens

###### This widget is compatible with encrypted header enrichment as well. If you wish to implement user flow 3 listed below, please discuss with support team for required special configurations.

1. Mobile User flow with PIN Authentication
2. WiFi User flow with PIN Authentication
3. Mobile User flow without PIN Authentication
4. Mobile User flow with only CAPTCHA 
5. Mobile User flow with PIN and CAPTCHA


### Flows
#### Flow 1
![alt text](http://docs.ideabiz.lk/static-images/1.HEflow-PIN.png " ")

___________________
#### Flow 2
![alt text](http://docs.ideabiz.lk/static-images/2.WIFI-flow pin.png " ")

________________
#### Flow 3
![alt text](http://docs.ideabiz.lk/static-images/3.HEflow.png " ")

__________________

#### Flow 4
![alt text](http://docs.ideabiz.lk/static-images/4.Captcha.png " ")

__________________
#### Flow 5
![alt text](http://docs.ideabiz.lk/static-images/5.Pincaptcha.png " ")

### How to use web widget

If application want to get user msisdn via Subscribing Dialog user to application, can use this service

1. Redirect user to web widget
2. Ideabiz will detect MSISDN via header enrichment
3. If user using WiFi, Ideabiz will verify user via OTP SMS
4. 	1. If Registered user, we will redirect user back to your callback URL
		in this case, no click required from user
	2. If new user, Ideabiz will ask consent from user for subscription. 
		Then we will redirect user back to your callback URL
5. When redirecting user to callback, we are sending `ServerRef`. Application need to read that and call status check API
6. That reference generate only for that specific redirection. So API will return specific redirection status including MSISDN
7. Application need to use this as Single Sign on method and keep save information in session. Then app can continue do the necessary transaction with that specific user
8. If user visit back without session data, you can redirect user to Ideabiz and get the MSISDN
9. When you need to unsubscribe user, you need to call Unsubscribe method on Subscription API
10. When you call status API with server Ref, you can see info about specific transaction only
11. When you need to unsubscribe user, you need to call  `Unsubscribe` method of (Subscription API) [http://docs.ideabiz.lk/APIs/SubscriptionAPI]
