/*
Title: Ez Cash - Agent V1 - Test
Sort: 
*/

This eZCash testing/development API is a mandatory step if eZCash Live API is to be used. 
To use the eZCash Live API you require a Live Agent Alias and Agent PIN after signing relevant eZCash Agreements
&nbsp;


In the Test environment below test credentials are assigned. 
Test Agent Alias : 
```
IDEA_HUB 
```

Subscriber Aliases : 
```
769983684
769967784 
```

(Please call this number prior to Submit trasnaction request, to get a transaction authorized via mobile). 

### Authorization API calls
All API call request to ideabiz.lk required Authorization headers. Please refer (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) for Authorization


### API List

1.  Get Transaction Status Via Request ID
2. 	Submit Transaction Request



### Submit Transaction Request

#### Scenario 1 : Charge eZCash from end users eZCash wallet 

A. API submit transaction request – (Scenario: Over the counter payments - AOTC)

B. API get status via request ID*

* (Get status via request ID is common to all ez Cash API calls- recommended to call this API after any transaction type)




#### URL  
```
https://ideabiz.lk/apicall/ezcash/agentservice/v1-test/submitTransactionRequest 
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
    "agentAlias": "IDEA_HUB",
    "agentPin": "1234",
    "agentnotificationSend": "true",
    "channel": "WEB",
    "requestId": "ESB014",
    "subscriberMobile": "769983684",
    "subscribernotificationSend": "true",
    "txAmount": "10",
    "txReference": "887565759",
    "txType": "TX_AOTC"
  }
}

```

|Parameter Name		| Description 			|
| ------------- 	|:-------------:		|
| ownerAlias 		| Agent Alias  			|
| ownerPin			| Agent pin				|
| requestId			| Send a unique Request ID in every Transaction|
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
      "ownerAlias": IDEA_HUB,
      "ownernotificationSend": "true",
      "receivingCommission": 0,
      "recipientAlias": 769983684,
      "requestId": "IDEA_HUB_ESB014",
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
https://ideabiz.lk/apicall/ezcash/agentservice/v1-test/submitTransactionRequest 
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
    "agentAlias": "IDEA_HUB",
    "agentPin": "1234",
    "agentnotificationSend": "true",
    "channel": "WEB",
    "requestId": "ESB015",
    "subscriberMobile": "769983684",
    "subscribernotificationSend": "true",
    "txAmount": "10",
    "txReference": "887565759",
    "txType": "TX_ACWT"
  }
}
```

|Parameter Name		| Description 			|
| ------------- 	|:-------------:		|
| ownerAlias 		| Agent alias  			|
| ownerPin			| Agent pin				|
| requestId			| Send a Unique Request ID for every Transaction|
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
      "ownerAlias": IDEA_HUB,
      "ownernotificationSend": "true",
      "receivingCommission": 0,
      "recipientAlias": 769983684,
      "requestId": "IDEA_HUB_ESB015",
      "responseId": 22975,
      "serviceCharge": 0,
      "status": 10,
      "subscribernotificationSend": "true",
      "txAmount": 10,
      "txType": "TX_ACWT",
      "eZCashRefId": "20150715044527",
      
    }
  }
}

```


### Get Transaction Status Via Request ID

#### Pre-condition
Need to run `Submit Transaction Request` and get status via the same `requestId`

#### URL  
```
https://ideabiz.lk/apicall/ezcash/agentservice/v1-test/transactionStatusViaRequestId
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
    "ownerAlias": "IDEA_HUB",
    "ownerPin": "1234",
    "requestId": "ESB015"
  }
}
```

|Parameter Name		| Description 			|
| ------------- 	|:-------------:		|
| ownerAlias 		| Agent alias  			|
| ownerPin			| Agent pin				|
| requestId			| `request id` sent in the `Submit Transaction Request`  |


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
      "ownerAlias": IDEA_HUB,
      "ownernotificationSend": "false",
      "receivingCommission": 0,
      "recipientAlias": 769983684,
      "requestId": "IDEA_HUB_ESB015",
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

A method call which is used to check the wallet balance.


#### URL
```
https://ideabiz.lk/apicall/ezcash/agentservice/v1-test/subscriberBalance
``` 


#### Method
  
    POST

#### Request Parameters
 
| Parameter        | Description           | 
| ------------- |:-------------:| 
| subscriberAlias     | Wallet Alias or Agent Alias|
|subscriberPin  |  Pin |

```
{
  "emoneybalancerequest": {
    "subscriberAlias": "IDEA_HUB",
    "subscriberPin": "1234"
  }
}
```


 
#### Response Parameters 

| Parameter        | Description           | 
| ------------- |:-------------:| 
| status     |Status description Possible login Status:5–SUCCESS 
|       |Note: All error code mentioned in ANNEXURE -2|
|availableAmount  |  Availability Wallet Balance |

```
{
  " getSubscriberBalanceResponse": {
    "return": {
      "status": 5,
      "availableAmount": 206.46,
      "description": "Transaction you performed is successful.",
      "msisdn": IDEA_HUB
    }
  }
}
```



## Error Codes 

| Failure Status|Status Description |
|----	|:-----|
| 6   	|   Transaction Failure     |
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

