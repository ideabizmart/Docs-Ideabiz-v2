/*
Title: Star Points
Sort: 8
*/



## Introduction
Star Points is Sri Lanka’s largest points based loyalty scheme operates on mobile platform.<br>
Star Points is a continuous rewards promotion that uses a points based loyalty strategy to deliver customer value at a low cost to the merchant partners. <br>
Star Points has been positioned as an alternative currency.<br>

**Obectives of star points**
<ol><li>Better customer satisfaction</li>
<li>Business Expansion</li>
<li>Non dialog catering</li>
<li>Better customer catering</li></ol>

## Authorization API calls
All API call request to ideabiz.lk required Authorization headers. Please refer (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) for Authorization


### Request Header
```sh
Authorization: Bearer [access token]
Accept: application/json
```

## Earn Starpoints
The functionality of earn points is to get the starpoints to customer from merchants. The earn point ratio is based on the merchants defined deliverable point ratio. 

**Operation Name:**
```
earnStarpoints()
```
<br>
**Earn Request**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------|
| *counterAlias | Counter name given by the star point system | String |
| *counterAuth | Counter password given by the star point system | String |
| *subscriberType  | [MOBILE, BAR_CODE ,ACCOUNT,WEB] | String |
| *subscriberValue | Value of the above selected type  | String |
| identificationNumber | Other Identification number.  | String |
| *billNumber  | Reference number to external system | String |
| *transactionAmount | Total value of the transaction |  String|
| *accessMode | Service Access mode(POS) | String |

<br>
**EARN Response**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------| 
| numberOfPointEarned  | Earned Star Points for received transaction | double |
| totalPoints | Remaining Total Star Points  | double |
| transactionReferance  | Unique identification of the transaction  | String |
| errorDesc  | Description of any error  | String |
| status  | Status of the transaction. 0 is success.  | int |

**URL**

```
https://ideabiz.lk/apicall/startpoint/v1/earnStarpoints
```
**Body**
```
{  
    "counterAlias": "TEST_COUNTER",
    "counterAuth" : "TEST_AUTH",
    "subscriberType" : "MOBILE",
    "subscriberValue" : "77XXXXXXX",
    "identificationNumber" : "09876",
    "billNumber" : "132345C001",
    "transactionAmount" :"200.00",
    "accessMode" :"POS"
}
```
## Burn Starpoints with authentication

Burn points API is performing the redeem points functionality of the starpoints system. It can be done after authenticating the customer by sending the PIN code to the mobile or by the random NIC based question. Points can be redeemed before expiration and there should be at least defined points by the system should be remaining after redeeming.

<br>
**Operation Name:**

```
burnStarpointsWithAuth()
```
**Burn Starpoints with Auth Request**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------|
| *counterAlias | Counter name given by the star point system | String |
| *counterAuth | Counter password given by the star point system | String |
| identificationNumber | This is optional number to identifying transaction mode(card number …)  | String |
| *billNumber | Merchant reference number | String |
| *noOfPoints | No of Star Points to be burnt | double |
| *billValue | Bill Amount | double |
| *subscriberType | [MOBILE, BAR_CODE ,ACCOUNT,EMAIL] | String |
| *subscriberValue  | [MOBILE_No, BAR_CODE No, ACCOUNT No,EMAIL Addr] | String |
| *subscriberAuth | Authentication Code of the Subscriber | String |
| *accessMode  | This can be POS or WEB  | String |

**Burn Starpoints with Auth Response**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------| 
| totalPoints | Total number of points of the customer. | double |
| transactionReferance  | Unique identification of the transaction  | String |
| errorDesc  | Description of any error  | String |
| status  | Status of the transaction. 0 is success.  | int |
<br>

**URL**
```
https://ideabiz.lk/apicall/startpoint/v1/burnStarpointsWithAuth
```

**Body**

```
{  
  "counterAlias":"TEST_COUNTER",
  "counterAuth":"TEST_AUTH",
  "identificationNumber": "77XXXXXXX",
  "billNumber":"132345C001",
  "noOfPoints":12.5,
  "billValue":100.00,
  "subscriberType":"MOBILE",
  "subscriberValue":"774715762",
  "subscriberAuth":"7460",
  "accessMode":"POS"
}
```

## Burn Starpoints without Authentication

“Burn points” function facilitates the customer to redeem the starpoints from his own accounts to other accounts.

**Operation Name:**

```
burnStarpoints()
```
<br>
**Burn Starpoints without Auth Request**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------|
| *counterAlias | Counter name given by the star point system | String |
| *counterAuth | Counter password given by the star point system | String |
| identificationNumber | This is optional number to identifying transaction mode(card number …)  | String |
| *billNumber | Merchant reference number  | String |
| *noOfPoints | No of Star Points to be burnt | double |
| *billValue | Bill Amount | double |
| *subscriberType | [MOBILE, BAR_CODE ,ACCOUNT,EMAIL] | String |
| *subscriberValue  | [MOBILE_No, BAR_CODE No, ACCOUNT No,EMAIL Addr] | String |
| *accessMode  | This can be POS or WEB  | String |

**Burn Starpoints without a Auth Response**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------| 
| totalPoints | Total number of points of the customer. | double |
| transactionReferance  | Unique identification of the transaction  | String |
| errorDesc  | Description of any error  | String |
| status  | Status of the transaction. 0 is success.  | int |

**URL**
```
https://ideabiz.lk/apicall/startpoint/v1/burnStarpoints
```
<br>
**Body**

```
{  
   "counterAlias": "TEST_COUNTER",
   "counterAuth" : "TEST_AUTH",
   "identificationNumber" : "77XXXXXXX",
   "billNumber":"132345C001",
   "noOfPoints":100,
   "billValue":100.00,
   "subscriberType":"MOBILE",
   "subscriberValue":"774715762",
   "accessMode" :"POS"
}
```
<br>
## Balance Check
Balance check is the function that is used to check the available amount, Redeemable amount and expiry amount of the account that is related to starpoints.

**Operation Name:**
```
balanceCheck()
```

**Balance Check Request**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------| 
| *counterAlias | Counter name given by the starpoint system | String |
| *counterAuth | Counter password given by the starpoint system | String |
| *subscriberType | [MOBILE, BAR_CODE, ACCOUNT…etc] | String |
| *subscriberValue | Value of the above selected type | String |
| *accessMode | This can be POS or WEB | String |
<br>
**Balance Check Response**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------| 
| currentBalance | Remaining Star Point Balance | double |
| redeemableBalance | Redeemable Star Point Balance | double |
| expiredAmount | Expired Star point amount  | double |
| status | Status of the transaction. 0 is success.  | int |
| errorDesc | Description of any error | String |
| expiredDate | Last Expiry calculated date | String |
<br>
**URL**
```
https://ideabiz.lk/apicall/startpoint/v1/balanceCheck
```
**Body**
```
{  
   "counterAlias":"TEST_COUNTER",
   "counterAuth":" TEST_AUTH",
   "subscriberType":"MOBILE",
   "subscriberValue":"77XXXXXXX",
   "accessMode":"POS"
}
```
<br>

## Earn Reversal
According to the customer or merchant request, earned points can be rollback by this functionality. The process of rolling back is done by the transaction reference number and the merchant counter.
<br>
**Operation Name:**
```
starpointsEarnReversal()
```
<br>
**Earn Reversal Request**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------|
| *counterAlias | Counter name given by the star point system | String |
| *counterAuth | Counter password given by the star point system | String |
| subscriberAuth | Customer authentication if required | String |
| *subscriberType | [MOBILE, BAR_CODE ,ACCOUNT,WEB] | String |
| *subscriberValue | Value of the above selected type | String |
| *billNumber | Bill number of the transaction | String |
| *transactionReference | References number of the reverse transaction | String |
| *accessMode  | Service Access mode(POS) | String |
<br>

**Earn Reversal Response**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------| 
| transactionReference | References number of the reverse transaction | String |
| errorDesc | Description of the  error | String |
| status | Status of the transaction. 0 is success.  | int |

**URL**
```
https://ideabiz.lk/apicall/startpoint/v1/starpointsEarnReversal
```
**Body**
```
{  
   "counterAlias": "TEST_COUNTER",
   "counterAuth" : "TEST_AUTH",
   "subscriberType":"MOBILE",
   "subscriberValue":"77XXXXXXX",
   "billNumber":"132345C001",
   "transactionReference":"791289004",
   "accessMode":"POS"
}
```
<br>

## Burn Reversal Starpoints
The burn reversal of starpoints system functionality is handling all reversals of points burn that was done in past. The time and the reference number should be the mandatory parameters that the merchant should be given to the system for the success of the functionality.
<br><br>
###Burn reversal using reference number
**Operation Name:**
```
starpointsBurnReversal()
```
<br>
**Burn Reversal Request**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------|
| *counterAlias | Counter name given by the star point system | String |
| *counterAuth | Counter password given by the star point system | String |
| subscriberAuth | Customer authentication if required | String |
| *subscriberType | [MOBILE, BAR_CODE ,ACCOUNT,WEB] | String |
| *subscriberValue | Value of the above selected type | String |
| Identification Number | The number that is identified for the bill | String |
| *billNumber | Bill number of the reverse transaction | String |
| *transactionReference | References number of the reverse transaction | String |
| *accessMode  | Service Access mode(POS) | String |


**Burn Reversal Response**



| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------| 
| transactionReference | Unique identification of the reverse transaction | String |
| errorDesc | Description of any error  | String |
| status | Status of the transaction. 0 is success.  | int |

<br>
**URL**
```
https://ideabiz.lk/apicall/startpoint/v1/starpointsBurnReversal
```
<br>
**Body**
```
{  
   "counterAlias": "TEST_COUNTER",
   "counterAuth" : "TEST_AUTH",
   "subscriberType":"MOBILE",
   "subscriberValue":"774715762",
   "identificationNumber":"77XXXXXXX",
   "billNumber":"132345C001",
   "accessMode":"POS"
}
```
### Burn reversal using bill number
<br>
**Operation Name**
```
starpointsBurnReversalByBillNo()
```
**Burn Reversal Request**

| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------|
| *counterAlias | Counter name given by the star point system | String |
| *counterAuth | Counter password given by the star point system | String |
| subscriberAuth | Customer authentication if required | String |
| *subscriberType | [MOBILE, BAR_CODE ,ACCOUNT,WEB] | String |
| *subscriberValue | Value of the above selected type | String |
| Identification Number | The number that is identified for the bill | String |
| *billNumber | Bill number of the transaction | String |
| *transactionReference | References number of the reverse transaction | String |
| *accessMode  | Service Access mode(POS) | String |

**Burn Reversal Response**
| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------| 
| transactionReference | Unique identification of the reverse transaction | String |
| errorDesc | Description of any error  | String |
| status | Status of the transaction. 0 is success.  | int |

**URL**
```
https://ideabiz.lk/apicall/startpoint/v1/starpointsBurnReversalByBillNo
```
**Body**
```
{  
   "counterAlias": "TEST_COUNTER",
   "counterAuth" : "TEST_AUTH",
   "subscriberType":"MOBILE",
   "subscriberValue":"77XXXXXXX",
   "identificationNumber":"777371371",
   "billNumber":"132345C001",
   "accessMode":"POS"
}
```

## Profile Registration
To access to the starpoints through an application, first of all it needs to register to the starpoints system through profile registration process.
<br>
**Operation Name:**
```
profileRegister(), profileRegisterCommit()
```
<br>
**Profile Register Request**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------|
| *counterAuth | Counter password given by the starpoint system | String |
| *counterAlias | Counter name given by the starpoint system | String |
| typeBeans | This is array list of IdentityTypeBean  | IdentityTypeBean[] (subscriberType,subscriberValue) |
| cxName | Value of the above selected type | String |
| address | This can be POS or WEB | String |
| *identificationType | Customer Identification type NIC/PP | String |
| *identifiacationValue | Customer Identification value.| String |
| email | Customer Email. Email is mandatory for if the accessMode is WEB | String |
| *accessMode  | This can be POS or WEB | String |
<br>

**Profile Registration Response**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------| 
| transactionNumber | Reference number for commit request | String |
| errorDesc | Description of any error  | String |
| status | Status of the transaction. 0 is success.  | int |
| isPinAvailable | If true need to get customer pin | Boolean |
| pinQuestion | Question need to ask for get PIN | String |
<br>
**URL**
```
https://ideabiz.lk/apicall/startpoint/v1/profileRegister
```
<br>
**Body**
```
{  
  "counterAlias":"TEST_COUNTER",
  "counterAuth":"TEST_AUTH",
  "typeBeans":[{"subscriberType":"BAR_CODE",
"subscriberValue":"11112230"}],
  "identificationType":"NIC",
  "identificationValue": "888888888V",
  "accessMode" : "POS"
}
```
**Profile Register Commit Request**

| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------|
| *counterAuth | Counter password given by the starpoint system | String |
| *counterAlias | Counter name given by the starpoint system | String |
| *referenceNumber | Reference number return by registration request | String |
| customerPin | Value of the above selected type | String |
| *accessMode  | This can be POS or WEB | String |
<br>
**Profile Register Commit Response**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------| 
| errorDesc | Description of any error  | String |
| status | Status of the transaction. 0 is success.  | int |
| applicationAliases | This is array list of IdentityTypeBean <br> In web mode system return ACCOUNT for future transactions. | This is array list of IdentityTypeBean <br>In web mode system return ACCOUNT for future transactions. |
<br>
**URL**
```
https://ideabiz.lk/apicall/startpoint/v1/profileRegisterCommit
```
<br>
**Body**
```
{  
  "counterAlias":"TEST_COUNTER",
  "counterAuth":"TEST_AUTH",
  "referenceNumber":"2529",
  "accessMode" : "POS"
}
```
<br>
## Authentication for 3rd party Applications
These APIs are provided for 3rd party applications which are connecting with Dialog Starpoints and it is authenticating the mobile application to do the transaction and view the balance check.
<br>
**Operation Name:**
```
sendAuthCodeToCustomer(),validateCustomerAuthentication()
```
**Send Auth Code to Customer**

| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------|
| *counterAlias | Counter name given by the starpoint system | String |
| *counterAuth | Counter password given by the starpoint system | String |
| *subscriberType | [MOBILE, BAR_CODE ,ACCOUNT,WEB] | String |
| *subscriberValue | Value of the above selected type | String |
| *accessMode  | This can be POS or WEB | String |
<br>
**Send Auth Code Customer Response**

| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------| 
| errorCode | Unique identification of the reverse transaction | double |
| errorMessage | Description of any error  | double |
| isPinSend | Unique identification of the transaction | String |
| randomQuestion | Question generated from NIC number | String |
<br>
**URL**
```
https://ideabiz.lk/apicall/startpoint/v1/sendAuthCodeToCustomer
```
<br>
**Body**
```
{  
  "counterAlias":"TEST_COUNTER",
  "counterAuth":"TEST_AUTH",
  "subscriberType":"MOBILE",
  "subscriberValue":"77XXXXXXX",
  "accessMode":"POS"
}
```
<br>
**Validate Customer Aunthentication Request**

| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------|
| *counterAlias | Counter name given by the starpoint system | String |
| *counterAuth | Counter password given by the starpoint system | String |
| *subscriberType | [MOBILE, BAR_CODE ,ACCOUNT,WEB] | String |
| *subscriberValue | Value of the above selected type | String |
| *customerPin | PIN value that the customer received | String |
| *accessMode  | This can be POS or WEB | String |
<br>
**Validate Customer Authentication Response**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------| 
| errorCode | Defined status for particular Error  | String |
| errorDescription | Description of the error code  | String |
<br>
**URL**
```
https://ideabiz.lk/apicall/startpoint/v1/validateCustomerAuthentication
```
<br>
**Body**
```
{  
  "counterAlias":"TEST_COUNTER",
  "counterAuth":"TEST_AUTH",
  "subscriberType":"MOBILE",
  "subscriberValue":"77XXXXXXX",
  "accessMode":"POS"
}
```
<br>
## Point Transfer
This function is to fulfill the requirement of transferring star points between mobile numbers within same NIC, BRC or Passport.
<br>
**Operation Name:**
```
transferStarpoints()
```
<br>
**Points Transfer Request**

| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------|
| *counterAlias | Counter name given by the starpoint system | String |
| *counterAuth | Counter password given by the starpoint system | String |
| *serviceAccessMode | Service Access mode(POS) | String |
| *fromsubscriberType | [MOBILE, BAR_CODE ,ACCOUNT,EMAIL] | String |
| *fromsubscriberValue | Value of the above selected type | String |
| *tosubscriberType | [MOBILE, BAR_CODE ,ACCOUNT,EMAIL] | String |
| *tosubscriberValue | Value of the above selected type | String |
| *amount | Star point amount | double |
<br>

**Point Transfer Response**

| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------| 
| errorCode | Defined status for particular Error  | String |
| errorDescription | Description of the error code  | String |
<br>
**URL**
```
https://ideabiz.lk/apicall/startpoint/v1/transferStarpoints
```
<br>
**Body**
```
{  
  "counterAlias":"TEST_COUNTER",
  "counterAuth":"TEST_AUTH",
  "serviceAccessMode":"POS",
  "fromSubscriberType":"MOBILE",
  "fromSubscriberValu":"77XXXXXXX"    ,
  "toSubscriberType":"MOBILE",
  "toSubscriberValue":"77XXXXXXX ",
  "amount":200.00
}
```

## Point Donation
This function is to highlight the scenario where customer will be able to donate star points from his account to predefined list of charitable institutes.<br>
E.g.: <br>
<ul><li>HelpAge</li>
<li>Unicef</li>
</ul>
<br>
**Operation Name:**

```
starpointsDonate()
```
<br>
**Points Donation Request**

| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------|
| *counterAlias | Counter name given by the starpoint system | String |
| *counterAuth | Counter password given by the starpoint system | String |
| *donationAlias  | Donation name given by the star point system | String |
| *subscriberType | [MOBILE, BAR_CODE ,ACCOUNT,WEB] | String |
| *subscriberValue | Value of the above selected type | String |
| *amount  | Star point amount | String |
<br>
**Point Donation Response**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------| 
| statusCode | Defined status for particular Error  | String |
| errorDescription | Description of the error code  | String |
<br>
**URL**
```
https://ideabiz.lk/apicall/startpoint/v1/starpointsDonate
```
<br>
**Body**
```
{  
  "counterAlias":"TEST_COUNTER",
  "counterAuth":"TEST_AUTH",
  "donationAlias":"UNISEF",
  "subscriberType":"MOBILE",
  "subscriberValue":"77XXXXXXX ",
  "amount":200.00
}
```
<br>
## Points Redemption
This API provides the functionality of burning starpoints from different types of domains like DTV, GSM from the user’s own account.
<br>

**Operation Name:**

```
starpointRedemption()
```
<br>
**Points Redemption Request**

| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------|
| *counterAlias | Counter name given by the starpoint system | String |
| *counterAuth | Counter password given by the starpoint system | String |
| *subscriberType | [MOBILE, BAR_CODE ,ACCOUNT,EMAIL] | String |
| *subscriberValue | [MOBILE_No, BAR_CODE No,ACCOUNT No,EMAIL Addr] | String |
| *amount  | amount to be redeem | double |
| *domainName| Domain need to be paid the amount | String |
| *domainValue  | Mobile number or account Number | String |
<br>
**Points Redemption Response**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------| 
| statusCode | Defined status for particular Error  | String |
| errorDescription | Description of the error code  | String |
<br>
**URL**
```
https://ideabiz.lk/apicall/startpoint/v1/starpointsRedemption
```
<br>
**Body**
```
{  
  "counterAlias":"TEST_COUNTER",
  "counterAuth":"TEST_AUTH",
  "subscriberType":"MOBILE",
  "subscriberValue":"77XXXXXXX ",
  "amount":200.00,
  "domainName":"DTV",
  "domainValue":"77XXXXXXX "
}
```
<br>
## Notify Customer 
Third party application sends notification of the customer state changes.
<br>
**Operation Name:**
```
notifyCustomerEvent()
```
<br>
**Event Notification Request**

| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------|
| *currentSubscriber  | Current identification of the customer (MOBILE,77XXXXXXX) | IdentityTypeBean[] (subscriberType,subscriberValue) |
| newSubscriber | Subscriber new identification (MOBILE,77XXXXXXX) | IdentityTypeBean[] (subscriberType,subscriberValue) |
| *subscriberEvent | Received customer state change event (NUMBER_CHANGE, PROFILE_CHANGE, ACCOUNT_TRANSFER, CONTRACT_TRANSFER, RECYCLED, BLACK_LIST) | Enumeration |
| *actualUser  | User of the action preform | String |
<br>
**Event Notification Response**


| Parameter Name       | Description          | Data Type |
| ------------- |-----------------------| 
| statusCode | Defined status for particular Error  | String |
| errorDescription | Description of the error code  | String |
<br>
**URL**
```
https://ideabiz.lk/apicall/startpoint/v1/notifyCustomerEvent
```
<br>
**Body**
```
{  
"currentSubscriber":{"subscriberType" : "MOBILE","subscriberValue" : "77XXXXXXX ",},
"newSubscriber":{"subscriberType" : "MOBILE","subscriberValue" : "77XXXXXXX ",},  
"subscriberEvent":"ACCOUNT_TRANSFER",
"actualUser":"Q12"             
}
```