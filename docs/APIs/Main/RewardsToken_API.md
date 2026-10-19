
API allows application to reward and redeem data, and SMS bundles to/from a users mobile rewards wallet

## Content

* [Method](#method)
* [Generate Rewards Token](#generate-rewards-token)
* [Redeem Rewards Token](#redeem-rewards-token)
* [Apply Rewards](#apply-rewards)

## Authorization API calls
All API call requests to ideabiz.lk require Authorization headers. Please refer (http://docs.ideabiz.lk/Getting_Started/Generate_Token) for Authorization


## MSISDN 
```
Eg : 766691500 
```

## Method

The following REST methods are available:

   * Generate Rewards Token
   * Redeem Rewards Token
   * Apply Rewards



## Generate Rewards Token 

### Request

Given below is a sample request of Generate Rewards Token.

##### Resource URL
```sh
https://ideabiz.lk/apicall/digitalrewards/v0.1/rewards/data/generate
```

##### HTTP Method
```sh
POST
```

##### Headers
```sh
Content-Type : application/json
Authorization: Bearer [access token] 
Accept : application/json
```

##### Sample Headers
```sh
Content-Type : application/json
Authorization: Bearer bc4dcd7bre35rd4a95e6b1f347db93 
Accept : application/json
```

Body
```
 
{
	"rewardsGenerateRequest":{
	"amount":10,
	"clientCorrelator":"123456",
	"redemptionRequest": {
		"notifyURL":"http://rewards-creating-application.com/notifyOfRedemptionURL",
		"callbackData":"some-data-useful-to-the-requester"
	},
	"rewardsTag":"Noodles Promo"}
}

``` 
<br>
Given below are the Request parameters of Generate Rewards Token.
 

| **Parameter name** | **Description** | **Usage** |
| --- | --- | --- |
| amount | (decimal) the value of the amount parameter will differ based on the {typeOfReward}; sms – The number of SMS data – The amount of data in MB | Mandatory |
| callbackData | (string) will be passed back in this notification. This is used to identify the tokens generated related to requests (or any other useful data, such as a function name). This is only valid if notifications are required | Optional |
| clientCorrelator | (string) uniquely identifies this request. | Mandatory |
| notifyURL | (URI) the URL to which you would like to send the notification of redemption. (Currently cannot call external URL as micro-service server has no internet access) | Reserved |
| rewardsTag | (string) tag information for the rewards token | Optional |


<br>
### Response

Given below is a sample response of Generate Rewards Token.
```sh
{
	"rewardsGenerateResponse":{
	"amount":10,
	"pin":"123456789",
	"clientCorrelator":"123456",
	" redemptionRequest ": {
		"notifyURL":"http://rewards-creating-application.com/notifyOfRedemptionURL",
		"callbackData":"some-data-useful-to-the-requester"
	},
	"rewardsTag":"Noodles Promo",
	"resourceURL": "http://tokens.com/v1/rewards/data/tokens/abc123"
} 

```

<br>
The following are the Response parameters of Generate Rewards Token. All Request parameters will be relayed in the response. Additionally pin and resourceURL parameters will be added to the response.


| **Parameter name** | **Description** | **Usage** |
| --- | --- | --- |
| amount | (decimal) the value of the amount parameter will differ based on the {typeOfReward}; sms – The number of SMS data – The amount of data in MB | Mandatory |
| pin | (numeric) Unique PIN to redeem a generated token. | Mandatory |
| clientCorrelator | (string) uniquely identifies this request. | Mandatory |
| callbackData | (string) will be passed back in this notification. This is used to identify the tokens generated related to requests (or any other useful data, such as a function name). This is only valid if notifications are required | Optional |
| notifyURL | (URI) the URL to which you would like to send the notification of redemption. | Reserved |
| rewardsTag | (string) tag information for the rewards token | Optional |
| resourceURL | A reference URL to the request | Mandatory |


 <br>


## Redeem Rewards Token 



### Request

Given below is a sample request of Redeem Rewards Token.

##### Resource URL
```sh
https://ideabiz.lk/apicall/digitalrewards/v0.1/rewards/{msisdn}/
```
##### Sample URL
```sh
https://ideabiz.lk/apicall/digitalrewards/v0.1/rewards/766691500/
```
##### HTTP Method
```sh
POST
```

##### Headers
```sh
Content-Type : application/json
Authorization: Bearer [access token]
Accept : application/json
```

##### Sample Headers
```sh
Content-Type : application/json
Authorization: Bearer bc4dcd7bre35rd4a95e6b1f347db93 
Accept : application/json
```

Body 
```
 {
	"rewardsRedeemRequest":{
	"pin":"634304281914",
	"clientCorrelator":"123456",
	"redemptionRequest":
		{
		"notifyURL":"http://rewards-creating-application.com/notifyOfRedemptionURL",
		"callbackData":"some-data-useful-to-the-requester"
		}
	}
}
``` 
<br>
Given below are the Request parameters of Redeem Rewards Token.
 

| **Parameter name** | **Description** | **Usage** |
| --- | --- | --- |
| msisdn | Mobile number of the subscriber. | Mandatory |
| pin | (numeric) Unique PIN to redeem a generated token. | Mandatory |
| clientCorrelator | (string) uniquely identifies this request. If there is a communication failure during the request, by using the same clientCorrelator, it will avoid creating the same reward twice when retrying the request. | Mandatory |
| notifyURL | (URI) the URL to which you would like to send the notification of redemption. | Reserved |
| callbackData | (string) will be passed back in this notification. This is used to identify the tokens generated related to requests (or any other useful data, such as a function name). This is only valid if notifications are required | Optional |


<br>
### Response

Given below is a sample response of Redeem Rewards Token.
```sh
{
    "rewardsRedeemResponse": {
    "pin": "472110542908",
    "amount": 100,
    "type": "data",
    "clientCorrelator": "123456",
    "redemptionResponse": {
      "notifyURL": "http://rewards-creating-application.com/notifyOfRedemptionURL",
      "callbackData": "some-data-useful-to-the-requester"
    },
    "resourceURL": "https://ideabiz.lk/apicall/digitalrewards/v0.1/rewards/777334035/redeem"
  }
}
```

<br>
The following are the Response parameters of Redeem Rewards Token. All request parameters will be relayed in the response. Additionally type and resourceURL parameters will be added to the response.


| **Parameter name** | **Description** | **Usage** |
| --- | --- | --- |
| amount | (decimal) the value of the amount parameter will differ based on the {type}; sms – The number of SMS data – The amount of data in MB | Mandatory |
| type | (string) the type of reward, if its sms or data. | Mandatory |
| pin | (numeric) Unique PIN to redeem a generated token. | Mandatory |
| clientCorrelator | (string) uniquely identifies this request. | Mandatory |
| callbackData | (string) will be passed back in this notification. This is used to identify the tokens generated related to requests (or any other useful data, such as a function name). This is only valid if notifications are required. | Optional |
| notifyURL | (URI) the URL to which you would like to send the notification of redemption. | Reserved |
| resourceURL | A reference URL to the request. | Mandatory |


<br>



## Apply Rewards





#### Request
Given below is a sample request of Apply Rewards.

##### Resource URL
```sh
https://ideabiz.lk/apicall/digitalrewards/v0.1/rewards/{msisdn}/apply
```

##### Sample URL
```sh
https://ideabiz.lk/apicall/digitalrewards/v0.1/rewards/766691500/apply
```

##### HTTP Method
```sh
POST
```

##### Headers
```sh
Content-Type : application/json
Authorization: Bearer [access token]
Accept : application/json
```
##### Sample Headers
```sh
Content-Type : application/json
Authorization: Bearer bc4dcd7bre35rd4a95e6b1f347db93 
Accept : application/json
```

##### Body
```sh
{
  "rewardsApplyRequest": {
    "type": "data",
    "amount": 1,
    "clientCorrelator": "123458",
    "redemptionRequest": {
      "notifyURL": "http://rewards-creating-application.com/notifyOfRedemptionURL",
      "callbackData": "some-data-useful-to-the-requester"
    },
    "rewardsTag":"ABC",
    "smsText": "You have been awareded 1MB"
  }
}
```
The following are the Request parameters of Apply Rewards.



| **Parameter name** | **Description** | **Usage** |
| --- | --- | --- |
| msisdn | Mobile number of the subscriber. | Mandatory |
| type | (string) the type of reward, if its sms or data. | Mandatory |
| Amount | (decimal) the value of the amount parameter will differ based on the {type}; sms – The number of SMS data – The amount of data in MB | Mandatory |
| clientCorrelator | (string) uniquely identifies this request. | Mandatory |
| notifyURL | (URI) the URL to which you would like to send the notification of redemption. | Reserved |
| callbackData | (string) will be passed back in this notification. This is used to identify the tokens generated related to requests (or any other useful data, such as a function name). This is only valid if notifications are required. | Optional |
| smsText | (string) Full SMS notification message. | Optional |


<br>
#### Response
Following is a sample response of Apply Rewards.

```sh
{
  "rewardsApplyResponse": {
    "pin": null,
    "amount": 25,
    "type": "data",
    "clientCorrelator": "123456",
    "redemptionResponse": {
      "notifyURL": "http://rewards-creating-application.com/notifyOfRedemptionURL",
      "callbackData": "some-data-useful-to-the-requester"
    },
    "resourceURL": "https://ideabiz.lk/apicall/digitalrewards/v0.1/rewards/777334035/apply"
  }
}

```

The following are the Response parameters of Apply Rewards.
All request parameters will be relayed in the response. Additionally resourceURL parameter will be added to the response.


| **Parameter name** | **Description** | **Usage** |
| --- | --- | --- |
| amount | (decimal) the value of the amount parameter will differ based on the {type}; sms – The number of SMS data – The amount of data in MB | Mandatory |
| type | (string) the type of reward, if its sms or data | Mandatory |
| clientCorrelator | (string) uniquely identifies this request. | Mandatory |
| callbackData | (string) will be passed back in this notification. This is used to identify the tokens generated related to requests (or any other useful data, such as a function name). This is only valid if notifications are required | Optional |
| notifyURL | (URI) the URL to which you would like to send the notification of redemption. | Reserved |
| resourceURL | A reference URL to the request | Mandatory |



<br>
