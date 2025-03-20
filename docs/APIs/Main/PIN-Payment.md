
This API allows to Charge a MSISDN on demand, upon verification of a PIN sent via SMS. The user will not be charged if the PIN is not verified .

API flow is as below
1.`Send Charge Request`  and capture the server reference `serverRef` 
2.`Submit PIN` - use the PIN entered by user and the `serverRef` to get authorisation & charge status
3.`Status Check` - use the `serverRef` to check the status of any charge request 

## Requirements


**Authorization API calls** </br>
All API call request to ideabiz.lk required Authorization headers. Please refer the *Token Management* (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) document for Authorization.

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


### Charge Request
#### URL
```
https://ideabiz.lk/apicall/pin/payment/v1/charge
```
### Method
```
POST
```


#### Request
```
{
  "msisdn": "tel:+94777123456",
  "description": "test pay",
  "taxable": true,
  "callbackURL": null,
  "txnRef": "ABC-123",
  "amount": 1.00
}
```


#### Response
```
{
  "statusCode": "SUCCESS",
  "message": null,
  "data": {
    "status": "PENDING_AUTH",
    "serverRef": "dbb8e4d2e3af4ff5aaaf98310300f80b",
    "msisdn": "tel:+94777123456",
    "description": "test pay",
    "taxable": true,
    "callbackURL": null,
    "txnRef": "ABC-123",
    "amount": 1
  }
}
```

### Submit PIN
#### URL
```
https://ideabiz.lk/apicall/pin/payment/v1/submitPin
```
### Method
```
POST
```

#### Request
```
{
  "pin": "520972",
  "serverRef": "958e39b574a042c290e6019f0590e7f5"
}
```
#### Sample Error Responses
```
{
  "statusCode": "ERROR",
  "message": "Wrong PIN",
  "data": null
}
```
```
{
  "statusCode": "ERROR",
  "message": "Max attempt exceeded",
  "data": null
}
```

#### Success Response

```
{
  "statusCode": "SUCCESS",
  "message": "Transaction Success",
  "data": {
    "status": "SUCCESS",
    "serverRef": "dbb8e4d2e3af4ff5aaaf98310300f80b",
    "msisdn": "tel:+94777339033",
    "description": "test pay",
    "taxable": true,
    "callbackURL": null,
    "txnRef": "ABC-123",
    "amount": 1
  }
}
```

#### Insufficient Credit error
```
{
  "statusCode": "SUCCESS",
  "message": "User has insufficient credit for transaction",
  "data": {
    "status": "FAILED",
    "serverRef": "dbb8e4d2e3af4ff5aaaf98310300f80b",
    "msisdn": "tel:+94777339033",
    "description": "test pay",
    "taxable": true,
    "callbackURL": null,
    "txnRef": "ABC-123",
    "amount": 1
  }
}
```

### Status Check
#### URL
```
https://ideabiz.lk/apicall/pin/payment/v1/status/{Server Ref}
e.g.
https://ideabiz.lk/apicall/pin/payment/v1/status/9d4607ad1d454c1e9e0261cdcaaaecab
```
### Method
```
GET
```

```
{
  "statusCode": "SUCCESS",
  "message": "User has insufficient credit for transaction",
  "data": {
    "status": "FAILED",
    "serverRef": "dbb8e4d2e3af4ff5aaaf98310300f80b",
    "msisdn": "tel:+94777339033",
    "description": "test pay",
    "taxable": true,
    "callbackURL": null,
    "txnRef": "ABC-123",
    "amount": 1
  }
}
```


