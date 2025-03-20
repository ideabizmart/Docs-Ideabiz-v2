

## PIN Charge API

This api allow user to verify MSISDN ondemand without subscribing the applicaiton. 

Flow is `Send Verify Request` -> `Submit PIN` 

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

### Verification Request
#### URL
```
https://ideabiz.lk/apicall/pin/verify/v1/verify
```
### Method
```
POST
```


#### Request
```
{
    "method":"ANC",
    "msisdn":"94777339033"
}
```


#### Response
```
{
  "statusCode": "SUCCESS",
  "message": null,
  "data": {
    "status": "PENDING_AUTH",
    "serverRef": "f07927b4030048be99dc051314b5f40e",
    "msisdn": "tel:+94777339033",
    "method": "ANC"
  }
}

```

### Submit PIN
#### URL
```
https://ideabiz.lk/apicall/pin/verify/v1/submitPin
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
#### Sample Error Response
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
  "message": "Verification Status",
  "data": {
    "status": "SUCCESS",
    "serverRef": "8f27e17b51de4b64baa6cd5525506516",
    "msisdn": "tel:+94777339033",
    "method": "ANC"
  }
}

```



