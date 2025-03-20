
## Overview

Initiate push notification based payment transaction request and returns initiated transaction details or relevant error details.

## Requirements</br>

**Authorization API Calls**</br>
All API call requests to ideabiz.lk require Authorization headers. Please refer the *Token Management* (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) document for Authorization.

**Request Header**
```
Content-Type: application/json 
Authorization: Bearer [access token] 
Accept: application/json
X-IH-SECRETCODE: `{Merchant Secret Code}`(Onetime secret code provided for the merchant integration).
X-IH-PSW: `{Merchant Password}`(Onetime password provided for the merchant integration).
```
 
**Sample Request Header**
```
Content-Type: application/json 
Authorization: Bearer a92ba8hjgjhgjh3fa1609cabcd79
Accept: application/json
X-IH-SECRETCODE:xxxxxx
X-IH-PSW:xxxxxx
```

If sending a unicode string with the message, please set `Content-Type` charset to `UTF-8`

```
Content-Type: application/json;charset=UTF-8
```


# APIs

## Initiate Push Transaction

`https://ideabiz.lk/apicall/geniepos/v1/axipay/external/merchant/transaction/initiate/push`


#### Method
```
POST
```

## Parameters

| Parameter | Description | Data Type | Mandatory/ Optional |
|-----------|------|-----|-----------|----------------|
| merchantPgIdentifier | Merchant identification number. | String | Mandatory |
| counterId | Counter id of the merchant | Integer | Mandatory |
| genieAccountNumber | Genie account number of the customer (Mobile Number).| String | Mandatory |
| externalMerchantTransactionId | External merchant transaction Id. | String | Mandatory |
| chargeTotal | Transaction amount. | Decimal | Mandatory |
| transactionDateTime | Transaction date time. | DateTime | Mandatory |
| invoiceNumber | Invoice number of the merchant. | String | Optional |
| paymentReference | Payment reference. | String | Optional |

### Sample Request

```json
{  
   "request":{  
      "genieAccountNumber":"+94715109942",
      "merchantPgIdentifier":"PG00007632",
      "externalMerchantTransactionId":"25",
      "chargeTotal":7000,
      "counterId":9,
      "invoiceNumber":"sdf",
      "transactionDateTime":"2018-04-12 20:12:34",
      "paymentReference":"reference"
   }
}
```

### Sample Success Response

```json
{
    "response": {
        "status": "success",
        "message": "Successfully initiated OTC payment. Notification sent to customer device",
        "genieTransactionId": "20180000000050698"
    }
}
```

### Sample Failure Response

```json
{
    "response": {
        "status": "failure",
        "message": "ExternalMerchantTransactionId must unique for transactions",
        "code": "5000"
    }
}
```

***

### Initiate Push Transaction

`https://ideabiz.lk/apicall/geniepos/v1/axipay/external/merchant/transaction/initiate/qr`

## Description

Initiate QR based payment transaction request and returns initiated transaction details or relevant error details.

#### Method
```
POST
```

## Parameters

| Parameter | Description | Data Type | Mandatory/ Optional |
|-----------|------|-----|-----------|----------------|
| merchantPgIdentifier | Merchant identification number. | String | Mandatory |
| counterId | Counter id of the merchant | Integer | Mandatory |
| externalMerchantTransactionId | External merchant transaction Id. | String | Mandatory |
| chargeTotal | Transaction amount. | Decimal | Mandatory |
| transactionDateTime | Transaction date time. | DateTime | Mandatory |
| invoiceNumber | Invoice number of the merchant. | String | Optional |
| paymentReference | Payment reference. | String | Optional |

### Sample request

```json
{  
   "request":{  
      "amount":"300",
      "merchantPgIdentifier":"PG00007632",
      "externalMerchantTransactionId":"1002",
      "description":"this is a test Item",
      "invoiceNumber":"1234",
      "counterId":9,
      "transactionDateTime":"2018-04-12 20:12:34"
   }
}
```

### Sample Success Response

```json
{
    "response": {
        "status": "success",
        "message": "success",
        "externalMerchantTransactionId": "1002",
        "genieTransactionId": "20180000000050706",
        "qrEncryptedText": "zFeB9Oqhj2V5FghbDzFjcQ=="
    }
}
```

### Sample Failure Response

```json
{
    "response": {
        "status": "failure",
        "message": "ExternalMerchantTransactionId must unique for transactions",
        "code": "5000"
    }
}
```

***

## Get Transaction Status

`https://ideabiz.lk/apicall/geniepos/v1/axipay/external/merchant/transaction/getstatus`

## Description

Get status of the transaction and returns transaction details or relevant error details.

### Transaction Statuses

| Status  | Description |
|--------|----------|
| Merchant Initiated | Merchant Transaction Initiated |
| Verification Success | Customer QR Verification Success |
| Verification Fail | Customer QR Verification Fail |
| Payment Success | Payment Success |
| Payment Fail | Payment Fail |
| Merchant Cancel | Merchant Canceled the Transaction |
| Customer Approved | Customer Approves the Transaction |
| Customer Reject | Customer Rejects the Transaction |

#### Method
```
POST
```

## Parameters

| Parameter | Description | Data Type | Mandatory/ Optional |
|-----------|------|-----|-----------|----------------|
| merchantPgIdentifier | Merchant identification number. | String | Mandatory |
| externalMerchantTransactionId | External merchant transaction Id. | String | Mandatory |
| genieTransactionId | Genie transaction Id. | String | Mandatory |

> You can use either externalMerchantTransactionId or genieTransactionId to retrieve the transaction status, one of them should be mandatory.


## Sample request

### Request with Genie Transaction Id

```json
{  
   "request":{  
      "merchantPgIdentifier":"PG00007632",
      "genieTransactionId":"4198"
   }
}
```

### Request with Merchant Transaction Id

```json
{  
   "request":{  
      "merchantPgIdentifier":"PG00007632",
      "externalMerchantTransactionId":"1001"
   }
}
```

### Sample Success Response


```json
{
    "response": {
        "status": "success",
        "merchantTxnStatusName": "Verification Success",
        "externalMerchantTransactionId": 5069,
        "genieTransactionId": "20180000000050698",
        "transactionAmount": 7000,
        "invoiceNumber": "sdf",
        "transactionDateTime": "2018-04-12 20:12:34",
        "merchantPgIdentifier": "PG00007632"
    }
}
```

### Sample Failure Response

```json
{
    "response": {
        "status": "failure",
        "message": "There is no transaction record related to provided details",
        "code": "5009"
    }
}
```

***

## Cancel Transaction

`https://ideabiz.lk/apicall/geniepos/v1/axipay/external/merchant/transaction/cancel`

## Description

Cancel the transaction and returns canceled transaction details or relevant error details.

In order to cancel, the transaction should be in the status of `Merchant Initiated`.


#### Method
```
POST
```

## Parameters

| Parameter | Description | Data Type | Mandatory/ Optional |
|-----------|------|-----|-----------|----------------|
| merchantPgIdentifier | Merchant identification number. | String | Mandatory |
| externalMerchantTransactionId | External merchant transaction Id. | String | Conditionally Required |
| genieTransactionId | Genie transaction Id. | String | Conditionally Required |

> You can use either externalMerchantTransactionId or genieTransactionId to cancel the transaction, one of them should be mandatory.


### Sample Request

```json
{  
   "request":{  
      "externalMerchantTransactionId":1002,
      "merchantPgIdentifier":"PG00007632"
   }
}
```

### Sample Success Response

```json
{
    "response": {
        "status": "success",
        "message": "Successfully change the transaction status by merchant",
        "externalMerchantTransactionId": "1002",
        "genieTransactionId": "20180000000050706",
        "transactionAmount": 300,
        "invoiceNumber": "1234",
        "transactionDateTime": "2018-04-12 20:12:34",
        "merchantPgIdentifier": "PG00007632"
    }
}
```

### Sample Failure Response

```json
{
    "response": {
        "status": "failure",
        "message": "External Merchant verification failure.",
        "code": "5001"
    }
}
```

***

## Query Transactions

`https://ideabiz.lk/apicall/geniepos/v1/axipay/external/merchant/transaction/query`

## Description

Query transactions and returns transaction list or relevant error details.


#### Method
```
POST
```

## Parameters

| Parameter | Description | Data Type | Mandatory/ Optional |
|-----------|------|-----|-----------|----------------|
| merchantPgIdentifier | Merchant identification number. | String | Mandatory |
| externalMerchantTransactionId | External merchant transaction Id. | String | Conditionally Required |
| genieTransactionId | Genie transaction Id. | String | Conditionally Required |
| fromDate | From Date to filter. | String | Optional |
| toDate | To Date to filter. | String | Optional |
| outletId | Outlet Id to filter. | String | Optional |
| counterId | Counter Id to filter. | String | Optional |
| pageStart | Page Start to pagination | String | Optional |
| pageEnd | Page End to pagination | String | Optional |

> You can use either externalMerchantTransactionId or genieTransactionId to cancel the transaction, one of them should be mandatory.

### Sample Request

```json
{  
   "request":{  
      "genieTransactionId":"20180000000050340",
      "merchantPgIdentifier":"PG00007632",
      "pageStart":"10",
      "pageEnd":"20",
      "externalMerchantTransactionId": "101",
      "fromDate":"2018-04-12 20:12:34",
      "toDate":"2018-04-18 20:30:34",
      "outletId":181,
      "counterId":181
   }
}
```

### Sample Success Response

```json
{
    "response": {
        "transactionList": [
            {
                "genieTransactionId": "20180000000051003",
                "externalMerchantTransactionId": "101",
                "transactionDateTime": "2018-04-12 20:12:34",
                "genieAccountNumber": "+94715109942",
                "transactionAmount": 7000,
                "transactionStatus": "Customer Reject",
                "statusDescription": "Customer Reject"
            }
        ]
    }
}
```

### Sample Failure Response

```json
{
    "response": {
        "message": "No transactions for searching parameters.",
        "errorCode": "5014",
        "transactionList": []
    }
}
```

## Error codes

The following table lists the error codes.

| Status code | Description |
|--------|----------|
| 5000 | ExternalMerchantTransactionId must unique for transactions |
| 5001 | Lack of necessary details |
| 5002 | Merchant does not exist |
| 5003 | Your Wallet has been blocked. Please go to Menu>Contact Us and lodge a request to unblock |
| 5004 | This user account is inactive, please contact the call centre for more information |
| 5005 | Customer does not exist |
| 5006 | Cannot change status of this transaction, This transaction is already processed |
| 5007 | Cannot change to this status |
| 5008 | You have not permission to change status of this transaction |
| 5009 | Cannot find transaction |
| 5010 | The user account is inactive |
| 5011 | User permissions error, Please log in as a merchant admin user to perform the action |
| 5012 | You have not permission to get this transaction details |
| 5013 | User does not exist |
| 5014 | No transactions for searching parameters |
| 5015 | Required parameter missing - `{Parameter Name}` |

***

