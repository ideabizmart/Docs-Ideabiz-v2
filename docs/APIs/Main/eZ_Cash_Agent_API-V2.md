/*
Title: Ez Cash  – V2
Sort: 
*/

### Authorization API calls
All API call request to ideabiz.lk requires Authorization headers. Please refer (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) for Authorization


### API List
##### The following REST methods are available here:
1.  Get Transaction Status Via Request ID
2. 	Submit Transaction Request


In the Test environment below test credentials are assigned. 
agentAlias : 
```
IDEA_HUB 
```

agentPin : 
```
1234 
```
subscriberMobile : 
```
769983684
769967784 
```

(Please call this number prior to Submit trasnaction request, to get a transaction authorized via mobile). 

### Authorization API calls
All API call request to ideabiz.lk required Authorization headers. Please refer (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) for Authorization


### Submit Transaction Request

#### Scenario 1 : Charge ezcash from end users ez cash wallet –

    A. API submit transaction request – (scenario: Over the counter payments - AOTC)

    B. API get status via request ID*

* (Get status via request ID is common to all ez Cash API calls- recommended to call this API after any transaction type)




#### URL  
```
https://ideabiz.lk/apicall/ezcash/agentservice/v2/submitTransactionRequest
``` 

#### Method 
```
POST     
```

#### Headers
```sh
Content-Type : application/json
Accept : application/json
Authorization: Bearer [access token] 
```

#### Request Body
```
{
"txType":"TX_AOTC",
"agentAlias":"AGENT_ALIAS",
"agentPin":2222,
"txAmount":10,
"agentnotificationSend":true,
"subscribernotificationSend":true,
"requestId":"ESB12",
"subscriberMobile":"77XXXXXXX",
"channel":"WEB",
"txReference":"88756575961"
}
```

|Parameter Name		| Description 			|
| ------------- 	|:-------------:		|
| ownerAlias 		| User alias  			|
| ownerPin			| User pin				|
| requestId			| Request ID should be unique for every Transaction|
| txAmount          | txAmount should be equal or greater than 10|

#### Response Status 
```
200 OK
``` 

#### Response
```
{
    "txType": "TX_AOTC",
    "status": 62,
    "description": "Transaction is accepted",
    "ownerAlias": "AGENT_ALIAS",
    "recipientAlias": "77XXXXXXX",
    "txAmount": 10,
    "ownernotificationSend": true,
    "subscribernotificationSend": true,
    "requestId": "AGENT_ALIAS_ESB12",
    "responseId": "7436410",
    "eZCashRefId": "226570143",
    "serviceCharge": 0,
    "receivingCommission": 0,
    "netAmount": 0,
    "statusDescription": null,
    "initiatorPostBal": 0,
    "txDate": null,
    "receipientName": null,
    "initiatorName": null
}
```

__________________________


#### Scenario 2 : To transfer funds (from an agent wallet to end user wallet/s)


A. Submit transaction request – scenario wallet recharge (TX_ACWT)


B. API Get status via request ID

#### URL  
```
https://ideabiz.lk/apicall/ezcash/agentservice/v2/submitTransactionRequest
``` 

#### Method 
```
POST     
```

#### Headers
```sh
Content-Type : application/json
Accept : application/json
Authorization: Bearer [access token] 
```

#### Request Body
```
{
"agentAlias": "AGENT_ALIAS",
"agentPin": 0000,
"agentnotificationSend":true,
"channel": "WEB",
"requestId": "ESB0144",
"subscriberMobile": "77XXXXXXX",
"subscribernotificationSend":true,
"txAmount": "10",
"txReference": "887565759",
"txType": "TX_ACWT"
}


```

|Parameter Name		| Description 			|
| ------------- 	|:-------------:		|
| ownerAlias 		| User alias  			|
| ownerPin			| User pin				|
| requestId			| Request ID should be unique for every Transaction|
| txAmount          | txAmount should be equal or greater than 10|

#### Response Status 
```
200 OK
``` 

#### Response
```
{
    "txType": "TX_ACWT",
    "status": 5,
    "description": "Transaction you performed is successful.",
    "ownerAlias": "AGENT_ALIAS",
    "recipientAlias": "77XXXXXXX",
    "txAmount": 10,
    "ownernotificationSend": true,
    "subscribernotificationSend": true,
    "requestId": "AGENT_ALIAS_ESB0144",
    "responseId": "7436471",
    "eZCashRefId": "20170911038437",
    "serviceCharge": 0,
    "receivingCommission": 0,
    "netAmount": 10,
    "statusDescription": null,
    "initiatorPostBal": 0,
    "txDate": null,
    "receipientName": null,
    "initiatorName": "Ideabiz.lk"
}


```

#### Scenario 3 - Mobile Bill Payments (From an agent wallet) 

    A. Submit transaction request – scenario wallet recharge (TX_ATOB).

    B. API Get status via request ID.

#### URL  
```
https://ideabiz.lk/apicall/ezcash/agentservice/v2/submitTransactionRequest
``` 
#### Method 
```
POST     
```

#### Headers
```sh
Content-Type : application/json
Accept : application/json
Authorization: Bearer [access token] 
```

#### Request
```
{
    "agentAlias": "AGENT_ALIAS",
    "agentPin": 0000,
    "agentnotificationSend": "true",
    "channel": "WEB",
    "requestId": "IBiZ0017",
    "domain": "Dialog",
    "refernceMobileNumber": "77XXXXXXX",
    "accountNumber" : "77XXXXXXX",
    "subscribernotificationSend": "true",
    "txAmount": "20",
    "txType": "TX_ATOB"
  }

```

| Parameter     		| Description           				| 
| ------------- 		|:---------------------:				| 
| domain     			| Enter domain from bellow table 		|
| accountNumber  		| Beneficiary account/mobile number 	|
| txType				| should be `TX_ATOB` for bill payment 	|
| refernceMobileNumber 	| Agent mobile number to send status	|



### Bill payment domains
Domain used to identify bill payment account number

| Domain        | Description   | 
| ------------- |:-------------:| 
| GSM			| Dialog Mobile |
| DTV			| Dialog TV     |
| CDMA			| Dialog CDMA	|
| BB 			| Dialog WiMax  |
| ETI			| Etisalat 		|
| HUTCH			| Hutch			|


#### Response
```
{
    "txType": "TX_ATOB",
    "status": 17,
    "description": "Amount you entered is less than the minimum amount defined for the transaction.",
    "ownerAlias": "AGENT_ALIAS",
    "recipientAlias": null,
    "txAmount": 20,
    "ownernotificationSend": true,
    "subscribernotificationSend": true,
    "requestId": "AGENT_ALIAS_IBiZ0017",
    "responseId": "7441671",
    "eZCashRefId": null,
    "serviceCharge": 0,
    "receivingCommission": 0,
    "netAmount": 0,
    "statusDescription": null,
    "initiatorPostBal": 0,
    "txDate": null,
    "receipientName": null,
    "initiatorName": null
}

````

#### Sample SMS to Agent mobile number

```
Bill Payment
Hutch Mobile Payment to: 788181XXX sent by Ideabiz.lk 06/02/2017 17:55 Rs.50.00 Service Charge: Rs.0.00 sent successfully RN:20170206068373733
```

#### Sample SMS to Beneficiary user

```
Your number 788181XXX has received airtime of Rs. 50.00. Ref No:47806001
```



### Get Transaction Status Via Request ID

#### Pre-condition
Need to run `Submit Transaction Request` and get the Client’s Original `requestId`

#### URL  
```
https://ideabiz.lk/apicall/ezcash/agentservice/v1/transactionStatusViaRequestId
```
    

#### Method 
```
POST
```
     
#### Headers
```sh
Content-Type : application/json
Accept : application/json
Authorization: Bearer [access token] 
```

#### Request body
```
{
"ownerAlias":"AGENT_ALIAS",
"ownerPin":0000,
"requestId":"ESB0144"
}

```

|Parameter Name		| Description 			|
| ------------- 	|:-------------:		|
| ownerAlias 		| User alias  			|
| ownerPin			| User pin				|
| requestId			| In the `Submit Transaction Request`, Client’s original request id|


#### Response Status 
```
200 OK 
```

#### Response Body
```
{
    "txType": "TX_ACWT",
    "status": 5,
    "description": "Transaction you performed is successful.",
    "ownerAlias": "AGENT_ALIAS",
    "recipientAlias": "77XXXXXXX",
    "txAmount": 10,
    "ownernotificationSend": false,
    "subscribernotificationSend": false,
    "requestId": "AGENT_ALIAS_ESB0144",
    "responseId": "7436471",
    "eZCashRefId": "20170911038437",
    "serviceCharge": 0,
    "receivingCommission": 0,
    "netAmount": 10,
    "statusDescription": null,
    "initiatorPostBal": 0,
    "txDate": null,
    "receipientName": null,
    "initiatorName": null
}

```

## Sample request for submitTransaction on Test Environment

<b> Please generate and use a Sandbox Access Token in the header instead of a Production Token</b>
```
{
"txType":"TX_ACWT",
"agentAlias":"IDEA_HUB",
"agentPin":1234,
"txAmount":1.00,
"agentnotificationSend":true,
"subscribernotificationSend":true,
"requestId":"17thOCT00018881",
"subscriberMobile":"769967784",
"refernceMobileNumber":"769983684",
"channel":"WEB",
"txReference":"TxnRef773333878"
}
```


## Error Codes 

| Failure Status|Status Description |
|----|:-----|
| 6   |   Transaction Failure     |
| 12| Invalid Agent Status    |
|16 | Invalid Recipient Status  |
|25 | Unauthorized Transaction Type  |
| 26| Unauthorized Recipient Type  |
| 37|Invalid Agent Pin    |
| 40| Invalid Transaction Type  |
| 41|Invalid Initiator    |
| 63  | Invalid username/password   |
| 64  |Duplicate Request ID   |
|51 |Invalid Recipient    |
|55 | Account is blocked due to invalid pin retries   |
| 17|Amount less than the minimum value allowed   |
| 18| Amount exceed the maximum value allowed   |
|19 | Amount exceed the daily transaction capping  |
| 20  |Amount less than the minimum limit allow for compartment   |
|21 |Amount exceed the daily out capping  |
| 22|Insufficient amount balance    |
| 23  | Amount exceed the max limit for compartment allowed |
| 24| Amount exceed the daily in capping  |
|68 | Missing or incorrect requested parameters |

