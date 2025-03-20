/*
Title: Wallet API
Sort: 8
*/




## Overview

Dialog Payment API allows you to charge mobile subscribers for use of your web application or content. And also it allows you to charge a user directly based on their consent. Payment services are accessible via RESTful web services.

 ## Authorization API calls
All API call request to ideabiz.lk required Authorization headers. Please refer (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) for Authorization



**Operation Name:**
```
payment()
```

#### Payment Request 

| Parameter name        | Description           | Usage  |
| ------------- |:-------------:| -----|
|endUserId|Any URI. The endUserId does not need URL encoding If sent in the POST body for application/json requests, but does need encoding when used in the URL or form encoded post. In this case the endUserId is the user's MSISDN including the 'tel:' protocol identifier and the country code preceded by '+',  e.g. tel:+00123456789.|Mandatory|
|referenceCode  |String. This is unique per charge event. It is your reference for reconciliation purposes. WSO2.TELCO HUB will include this in reports so that you can match WSO2.TELCO HUB reference of what has been sold with your own reference|Mandatory|
|clientCorrelator|String. This uniquely identifies the create charge request. If there is a communication failure during the charge request, using the same clientCorrelator when retrying the request allows WSO2.TELCO HUB to avoid applying the same charge twice. |Optional|
|notifyURL|Any URI. Make payment APIs can be asynchornous. The status of the transaction will be posted to this URL|Optional|



#### chargingInformation

| Parameter name        |  Description| Usage |
| ----------------- |:-------------:| -----:|
|description|xsd:string. This is the text to appear on the user's bill to allow them to easily identify what they have bought|Mandatory|
|currency |xsd:string. This is the 3 figure code as per ISO 4217 |Mandatory|
|Amount| xsd:decimal. This is the actual amount being charged. It can bea whole number or two digit decimal|Mandatory|

#### chargingMetaData type
| Parameter name        |  Description| Usage |
| ----------------- |:-------------:| -----:|
|onBehalfOf|xsd:string. Allows aggregators/partners to specify the actual payee.|Optional|
|purchaseCategoryCode|xsd:string. A category defining the type of service, product or media being purchased.|Optional|
|channel |xsd:string. Indicates the source of user interaction| Optional|

#### Payment Response

| Parameter name        |  Description| Usage |
| ----------------- |:-------------:| -----:|
|transactionOperationStatus|String. “Charged” confirms that the resource state is „Charged‟ – the end user‟s account has been charged.|Mandatory|
|amount|Decimal. Indicates the amount that the developer asked to charge in the request. Amount should contain two decimal places.|Mandatory|
|serverReferenceCode|String. Is a reference to the charge or refund, provided by WSO2.TELCO HUB, and meaningful to WSO2.TELCO HUB‟s backend system for the purpose of reconciliation.|Mandatory|



**URL**

```
https://ideabiz.lk/apicall/wallet/v1/transaction/{user}/payment
```
**Body**
```
{
    "makePayment": 
    {
        "clientCorrelator": "54321",
        "endUserId": "tel:+94777123456",
        "paymentAmount": {
            "chargingInformation": {
                "amount": "2",
                "currency": "LKR",
                "description": "Alien Invaders Game"
            },
            "chargingMetaData" : {
                "onBehalfOf" : "Example Games Inc",
                "purchaseCategoryCode" : "Game",
                "channel" : "Mobile"
            }
        },
        "referenceCode": "REF-12345",
        “notifyURL”: “http://myapplication/mynotifyurl”
    }
}
```

**Sample response**
```
{
    "makePayment": {
        "clientCorrelator": "54321",
        "endUserId": "tel:+94777123456",
        "paymentAmount": {
            "chargingInformation": {
                "amount": "2",
                "currency": "LKR",
                "description": "Alien Invaders Game"
            },
            "chargingMetaData" : {
                "onBehalfOf" : "Example Games Inc",
                "purchaseCategoryCode" : "Game",
                "channel" : "Mobile"
            }
        },
        "referenceCode": "REF-12345",
        “serverReferenceCode”: “WALLET_REF_XX67675XX”,
        “notifyURL”: “http://myapplication/mynotifyurl”,
        “transactionOperationStatus”: “Charged”
    }
}
```


## Refund the User

**Operation Name:**
```
refund()
```

#### Refund Request

| Parameter name        | Description           | Usage  |
| ------------- |:-------------:| -----:|
| clientCorrelator      | (string) uniquely identifies this refund request. If there is a communication failure during the charge request, using the same clientCorrelator when retrying the Optional WSO2.Telco Gatewayd API Specification V1.1 89 request allows WSO2.TELCO HUB to avoid applying the same charge twice. This field SHOULD be present. | Optional |
| endUserId     | Unique identifier for the end user‟s account (e.g. „tel‟ URI for user‟s MSISDN, „acr‟ URI). endUserId is part of the request URL and the post body, these MUST have the same value.     |   Mandatory |
|originalReferenceCode| (string) The reference code sent with the original payment request.|Mandatory|
|originalServerReferenceCode| (string) if provided by WSO2.TELCO HUB when a charge transaction was created the originalServerReferenceCode MUST be used in the  refund API |Mandatory|
|referenceCode| String. This is unique per charge event. It is your reference for this request. WSO2.TELCO HUB will include this in reports so that you can match WSO2.TELCO HUB reference of what has been sold with your own reference |Mandatory|

#### chargingInformation

| Parameter name        |  Description| Usage |
| ----------------- |:-------------:| -----:|
|description|xsd:string. This is the text to appear on the user's bill to allow them to easily identify what they have bought| Mandatory|
|currency |xsd:string. This is the 3 figure code as per ISO 4217 |Mandatory|
|Amount| xsd:decimal. This is the actual amount being charged. It can be a whole number or two digit decimal| Mandatory|

#### chargingMetaData type
| Parameter name        |  Description| Usage |
| ----------------- |:-------------:| -----:|
|onBehalfOf|xsd:string. Allows aggregators/partners to specify the actual payee.|Optional|
|purchaseCategoryCode|xsd:string. A category defining the type of service, product or media being purchased.|Optional|
|channel |xsd:string. Indicates the source of user interaction |Optional|


#### Refund Response

| Parameter name        |  Description| Usage |
| ----------------- |:-------------:| -----:|
|transactionOperationStatus|String. "Charged" confirms that the resource state is "Charged" – the end user's account has been charged.|Mandatory|
|resourceURL|This is URI for this transaction. The service provider can invoke this URI to recall this transaction at a later time.|Mandatory|
|serverReferenceCode |String. Is a reference to the charge or refund, provided by WSO2.TELCO HUB, and meaningful to WSO2.TELCO HUB‟s backend system for the purpose of reconciliation.|Mandatory|



**URL**

```
https://ideabiz.lk/apicall/wallet/v1/{user}/refund
```
**Body**
```
{
    "refundTransaction": {
        "clientCorrelator": "54321",
        "endUserId": "tel:+94777123456",
        “originalReferenceCode”:”ABC-12345”
        "originalServerReferenceCode": " WALLET_REF_XX67675XX",
        "paymentAmount": {
            "chargingInformation": {
                "amount": "10",
                "currency": "USD",
                "description": "Alien Invaders Game"
            },
        "chargingMetaData" : {
            "onBehalfOf" : "Example Games Inc",
            "purchaseCategoryCode" : "Game",
            "channel" : "WAP"
        }
    },
    "referenceCode": "REF-12345"
    }

}
```

**Sample response**



    {
        "refundTransaction": {
        "clientCorrelator": "54321",
        "endUserId": "tel:+94777123456",
        “originalReferenceCode”:”ABC-12345”
        "originalServerReferenceCode": " WALLET_REF_XX67675XX",
        "paymentAmount": {
            "chargingInformation": {
                "amount": "10",
                "currency": "USD",
                "description": "Alien Invaders Game"
            },
                "chargingMetaData" : {
                "onBehalfOf" : "Example Games Inc",
                "purchaseCategoryCode" : "Game",
                "channel" : "WAP"
            }
        },
        "referenceCode": "REF-12345",
        "serverReferenceCode":"ABC-123",
        "resourceURL": "<Resource URL>",
        "transactionOperationStatus": "Refunded"
        }
    }