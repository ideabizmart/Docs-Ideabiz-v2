/*
Title: Ez Cash  – Agent V1
Sort: 
*/

### Authorization API calls
All API call request to ideabiz.lk required Authorization headers. Please refer (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) for Authorization


### API List

1.  Get Transaction Status Via Request ID
2. 	Submit Transaction Request



### Submit Transaction Request

#### Scenario 1 : Charge ezcash from end users ez cash wallet –

A. API submit transaction request – (scenario: Over the counter payments - AOTC)

B. API get status via request ID*

* (Get status via request ID is common to all ez Cash API calls- recommended to call this API after any transaction type)




#### URL  
```
https://ideabiz.lk/apicall/ezcash/agentservice/v1/submitTransactionRequest 
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
"agenttransactionrequest": {
"agentAlias": "AGENT_ALIAS",
"agentPin": "2222",
"agentnotificationSend": "true",
"channel": "WEB",
"requestId": "ESB014",
"subscriberMobile": "777333015",
"subscribernotificationSend": "true",
"txAmount": "10",
"txReference": "887565759",
"txType": "TX_AOTC"
}
}

```

|Parameter Name		| Description 			|
| ------------- 	|:-------------:		|
| ownerAlias 		| User alias  			|
| ownerPin			| User pin				|
| requestId			| Request ID should be unique for every Transaction|
| txAmount          | txAmount shoud be equal or greater than 10|

#### Response Status 
```
200 OK
``` 

#### Response
```
{
"submitTransactionRequestResponse": {
"return": {
"description": "Transaction is accepted",
"netAmount": 5,
"ownerAlias": 773331194,
"ownernotificationSend": "true",
"receivingCommission": 0,
"recipientAlias": 777333015,
"requestId": "773331194_ESB014",
"responseId": 22975,
"serviceCharge": 0,
"status": 10,
"subscribernotificationSend": "true",
"txAmount": 10,
"txType": "TX_AOTC",
"eZCashRefId": "20150715044527",

}
}
}

```

__________________________


#### Scenario 2 : To transfer funds (from an agent wallet to end user wallet/s)


A. Submit transaction request – scenario wallet recharge (TX_ACWT)


B. API Get status via request ID

#### URL  
```
https://ideabiz.lk/apicall/ezcash/agentservice/v1/submitTransactionRequest 
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
"agenttransactionrequest": {
"agentAlias": "773331194",
"agentPin": "2222",
"agentnotificationSend": "true",
"channel": "WEB",
"requestId": "ESB014",
"subscriberMobile": "777333015",
"subscribernotificationSend": "true",
"txAmount": "10",
"txReference": "887565759",
"txType": "TX_ACWT"
}
}

```

|Parameter Name		| Description 			|
| ------------- 	|:-------------:		|
| ownerAlias 		| User alias  			|
| ownerPin			| User pin				|
| requestId			| Request ID should be unique for every Transaction|
| txAmount          | txAmount shoud be equal or greater than 10|

#### Response Status 
```
200 OK
``` 

#### Response
```
{
"submitTransactionRequestResponse": {
"return": {
"description": "Transaction is accepted",
"netAmount": 5,
"ownerAlias": 773331194,
"ownernotificationSend": "true",
"receivingCommission": 0,
"recipientAlias": 777333015,
"requestId": "773331194_ESB014",
"responseId": 22975,
"serviceCharge": 0,
"status": 10,
"subscribernotificationSend": "true",
"txAmount": 10,
"txType": "TX_AOTC",
"eZCashRefId": "20150715044527",

}
}
}

```

#### Scenario 3 - Mobile Bill Payments (From an agent wallet) 

A. Submit transaction request – scenario wallet recharge (TX_ATOB)

B. API Get status via request ID

#### Request
```
{
  "agenttransactionrequest": {
    "agentAlias": "SAMPLE_AGENT",
    "agentPin": "0000",
    "agentnotificationSend": "true",
    "channel": "WEB",
    "requestId": "IBiZ0015",
    "domain": "HUTCH",
    "refernceMobileNumber": "77733xxxx",
    "accountNumber" : "78818xxxx",
    "subscribernotificationSend": "true",
    "txAmount": "50",
    "txType": "TX_ATOB"
  }
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
  "submitTransactionRequestResponse": {
    "return": {
      "description": "Transaction you performed is successful.",
      "netAmount": 49.33,
      "ownerAlias": "SAMPLE_AGENT",
      "ownernotificationSend": true,
      "receivingCommission": 0.67,
      "recipientAlias": "HUTCH MOBILE",
      "requestId": "IDEABIZ_LK_IBiZ0015",
      "responseId": 32500000,
      "serviceCharge": 0,
      "status": 5,
      "subscribernotificationSend": true,
      "txAmount": 50,
      "txType": "TX_ATOB",
      "eZCashRefId": 20170206068373733,
      "initiatorName": "Ideabiz.lk"
    }
  }
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
  "requesttransactionstatus": {
    "ownerAlias": "773331194",
    "ownerPin": "2222",
    "requestId": "ESB015"
  }
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
  "getTransactionStatusViaRequestIdResponse": {
    "return": {
      "description": "Transaction you performed is successful.",
      "netAmount": 2,
      "ownerAlias": 773331194,
      "ownernotificationSend": "false",
      "receivingCommission": 0,
      "recipientAlias": 777333015,
      "requestId": "773331194_ESB015",
      "responseId": 22992,
      "serviceCharge": 0,
      "status": 5,
      "subscribernotificationSend": "false",
      "txAmount": 2,
      "txType": "TX_AOTC",
      "eZCashRefId": "20150716044532"
    }
  }
}
```

### Get Subscriber Balance

A method call is used to check the wallet balance.


#### URL
```
https://ideabiz.lk/apicall/ezcash/agentservice/v1/subscriberBalance
``` 


#### Method
  
    POST

#### Request Parameters
 
| Parameter        | Description           | 
| ------------- |:-------------:| 
| subscriberAlias     | Wallet Alias e.g. agent alias|
|subscriberPin  |  Pin |

      
          {
            "emoneybalancerequest": {
              "subscriberAlias": "AGENT_ALIAS",
              "subscriberPin": "2222"
            }
          }


 
#### Response Parameters 

| Parameter        | Description           | 
| ------------- |:-------------:| 
| status     |Status description Possible login Status:5–SUCCESS 
|       |Note: All error code mentioned in ANNEXURE -2|
|availableAmount  |  Availability Balance |


      {
        " getSubscriberBalanceResponse": {
              "return": {
                 "status": 5,
                  "availableAmount": 206.46,
                  "description": "Transaction you performed is successful.",
                  "msisdn": 773331194
                }
          }
      }    


## Sample request for submit Transaction


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

