

## Content
* [Overview](#overview)
* [Method](#method)
* [Authorization](#authorization-api-calls)
* [Charging a subscriber](#charging-a-subscriber)
* [Response Codes](#response-codes)
* [Exceptions](#exceptions)
* [Faults](#faults)

 

## Overview

The Carrier Billing API allows you to charge mobile subscribers for the use of your application or content. It also allows you to charge a user directly based on their consent. Payment services are accessible via RESTful web services.

### Method

The following REST method is available:

   * Charging a subscriber

## Requirements


**Authorization API calls** </br>
Please note that All Ideabiz APIs require an access token to be passed on the API header along with 2 more headers. Refer below documentation to get an understanding on how to generate access tokens.
Documentation: https://docs.ideabiz.lk/Getting_Started/Token_Manegment

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

### Encrypted MSISDN
Please refer the *Secure Header* (http://docs.ideabiz.lk/APIs/Header_Enrichment) document for encrypted MSISDNs.

## Charging a subscriber

This allows you to charge a subscriber for a service provided by your application. All charges are Taxable, and Standard Taxes will be charged from the subscriber  on top of the charging amount.
</br></br>
*If you are a charity service or if different tax schemes are applicable for your service, please discuss with support team.


#### **Request**

Given below is a sample request of the send service.

#### URL
```
https://ideabiz.lk/apicall/carrier-billing/{version}/payments
```
#### Sample URL for API
```
https://ideabiz.lk/apicall/carrier-billing/v1/payments
```

#### Method
```
POST
```

#### Body
```
'{
  "amountTransaction": {
    "phoneNumber": "tel:94777123456",
    "clientCorrelator": "req-12f2pgh448gh2hvrfrv",
    "paymentAmount": {
      "chargingInformation": {
        "amount": 100,
        "currency": "EUR",
        "description": "FIFA EA Sports 24",
        "isTaxIncluded": false,
        "taxAmount": 21
      },
      "chargingMetaData": {
        "merchantName": "EA Sports",
        "merchantIdentifier": "eas-12345",
        "fee": 10,
        "purchaseCategoryCode": "games",
        "channel": "web",
        "serviceId": "games-online",
        "productId": "138235321"
      },
      "paymentDetails": [
        {
          "amount": 100,
          "currency": "EUR",
          "description": "FIFA EA Sports 24",
          "isTaxIncluded": false,
          "taxAmount": 0
        }
      ]
    },
    "referenceCode": "ref-pay-834tfr2rA3v8r8vr3rv"
  },
  "webhook": {
    "notificationUrl": "https://myservice.com/payment/events",
    "notificationAuthToken": "c8974e592c2fa383d4a3960714"
  }
}'
``` 
<br>
Given below are the Request parameters of the Payment service.

|Parameter Name      |Description    | Type    | Mandatory/Optional   |
|--------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------|---------------------|
| clientCorrelator    | This uniquely identifies the Charging request. If there is a communication failure during the charging request, using the same clientCorrelator, when retrying the request, the operator can avoid applying the same charge twice. | string  | Optional             |
| phoneNumber         | In this case the endUserId is the user's MSISDN including the 'tel:' protocol identifier and the country code preceded by '+'.                                                                                                     | string  | Mandatory            |
|                     | Eg: tel:94766691500.                                                                                                                                                                                                              |         |                      |
| chargingInformation | description -This is the text to include, to easily identify what the user has been charged for. This description should be limited to 190 characters.                                                                             | string  | Mandatory            |
| 	                  |                                                                                                                                                                                                                                	   |         |                      |
| 	    amount              | This indicates the amount to be charged to the end user.                                                                                                                                                                  | decimal | Mandatory            |
| 	          currency        | This indicates the currency in which the amount is specified.                                                                                                                                                           | string  | Mandatory            |
| 	        purchaseCategoryCode          | A category defining the type of service, product or media being purchased.(Optional - if you don't have a Category code, please keep this empty or remove this property)                                    | string  | Optional             |
| 	       channel           | Indicates the source of user interaction.(Optional - to identify in case several channels are available e.g. Web/ Mobile App/ SMS)                                                                                       | string  | Optional             |
| 	         taxAmount         | Specify as '0' as Tax is already preconfigured in the platform                                                                                                                                                         | decimal | Optional             |
| 	     serviceID             | Charging Service ID. The specific service that you intend to charge the customer           | string  | Mandatory             |


### Response
```
Status code  : 201
```
```
{
  "paymentId": "AK234rfweSBuWGFUEWFGWEVWRV",
  "amountTransaction": {
    "phoneNumber": "tel:94777123456",
    "clientCorrelator": "req-12f2pgh448gh2hvrfrv",
    "paymentAmount": {
      "chargingInformation": {
        "amount": 100,
        "currency": "LKR",
        "description": "FIFA EA Sports 24",
        "isTaxIncluded": false,
        "taxAmount": 21
      },
      "chargingMetaData": {
        "merchantName": "EA Sports",
        "merchantIdentifier": "eas-12345",
        "fee": 10,
        "purchaseCategoryCode": "games",
        "channel": "web",
        "serviceId": "games-online",
        "productId": "138235321"
      },
      "paymentDetails": [
        {
          "amount": 100,
          "currency": "LKR",
          "description": "FIFA EA Sports 24",
          "isTaxIncluded": false,
          "taxAmount": 21
        }
      ]
    },
    "referenceCode": "ref-pay-834tfr2rA3v8r8vr3rv",
    "transactionOperationStatus": "processing",
    "resourceURL": "urn:payments:AK234rfweSBuWGFUEWFGWEVWRV",
    "serverReferenceCode": "ref-pay-834tfr2rA3v8r8vr3rv-serv"
  },
  "paymentCreationDate": "2024-07-01T11:06:08.952Z",
  "paymentDate": "2024-07-01T11:06:08.952Z",
  "webhook": {
    "notificationUrl": "https://myservice.com/payment/events",
    "notificationAuthToken": "c8974e592c2fa383d4a3960714"
  }
}
```
#### Errors

Response

```json
{
  "code": "INVALID_ARGUMENT",
  "status": 400,
  "message": "Schema validation failed at  ..."
}
```

```json
{
  "code": "CARRIER_BILLING.PAYMENT_DENIED",
  "status": 403,
  "message": "Transaction not allowed: outgoing is barred for this account."
}
```

```json
{
  "code": "INVALID_ARGUMENT",
  "status": 400,
  "message": "Invalid account details. Please verify the subscriber/account and try again."
}
```

```json
{
  "code": "CARRIER_BILLING.PAYMENT_DENIED",
  "status": 403,
  "message": "Not authorized to perform this transaction."
}
```

```json
{
  "code": "SERVER_ERROR",
  "status": 502,
  "message": "Temporary system error while processing the transaction. Please try again later."
}
```

```json
{
  "code": "INVALID_ARGUMENT",
  "status": 400,
  "message": "Invalid reason code provided for this transaction."
}
```

```json
{
  "code": "INVALID_ARGUMENT",
  "status": 409,
  "message": "Duplicate request detected. This transaction appears to have been submitted already."
}
```


| Camara http response code   | Response->Code                      | Response->Message                        |
|----------------------------|------------------------------------|-----------------------------------------|
|                         400 | INVALID_ARGUMENT                    | Schema validation failed at  ..          |
|                         400 | INVALID_ARGUMENT                    | Schema validation failed at  ..          |
|                         400 | INVALID_ARGUMENT                    | Schema validation failed at  ..          |
|                         400 | INVALID_ARGUMENT                    | clientCorrelator already exist on server |
|                         400 | INVALID_ARGUMENT                    | Invalid account details. Please verify the subscriber/account and try again.        |
|                         400 | INVALID_ARGUMENT                    | Invalid reason code provided for this transaction.         |
|                         403 | CARRIER_BILLING.PAYMENT_DENIED      | CARRIER_BILLING.PAYMENT_DENIED           |
|                         403 | CARRIER_BILLING.PAYMENT_DENIED      | Transaction not allowed: outgoing is barred for this account          |
|                         403 | CARRIER_BILLING.PAYMENT_DENIED      | Not authorized to perform this transaction.          |
|                         403 | CARRIER_BILLING.UNAUTHORIZED_AMOUNT | Unauthorized amount requested            |
|                         403 | CARRIER_BILLING.PAYMENT_DENIED      | Payment denied by business               |
|                         403 | CARRIER_BILLING.UNAUTHORIZED_AMOUNT | User has insufficient credit for the full transaction amount|
|                         409 | INVALID_ARGUMENT                    | Duplicate request detected. This transaction appears to have been submitted already          |
|                         502 | SERVER_ERROR                        | Temporary system error while processing the transaction. Please try again later             |
