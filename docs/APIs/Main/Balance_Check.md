
## Content

* [Overview](#overview)
* [Method](#method)
* [Requirements](#authorization-api-calls)
* [Balance Checking](#balance-checking)
* [Response Codes](#response-codes)
* [Exceptions](#exceptions)
* [Faults](#faults)

## Overview
The Dialog Balance Check API allows you to obtain the account balance information of a user. Balance check services are accessible via RESTful web services.

### Method

The following REST methods are available: 
+  Obtaining the account balance information of a user.

## Requirements</br>

**Authorization API Calls**</br>
All API call requests to ideabiz.lk require Authorization headers. Please refer the Token Management (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) documentation for Authorization. 

**Request Header**
```
Authorization: Bearer [access token] 
Accept: application/json
Content-Type: application/json
```
 
**Sample Request Header**
```
Authorization: Bearer a92ba8hjgjhgjh3fa1609cabcd79
Accept: application/json
Content-Type: application/json
```
### Encrypted MSISDN
Please refer Header Enrichment (http://docs.ideabiz.lk/APIs/Header_Enrichment) document for encrypted MSISDNs.

## Balance Checking

This allows you to send an SMS from your Web application to one or more addresses (MSISDNs).

#### URL
```
https://ideabiz.lk/apicall/balancecheck/v3/{MSISDN}/transactions/amount/balance
```
```
https://ideabiz.lk/apicall/balancecheck/v4/{MSISDN}/transactions/amount/balance
```
Please note that when you add an **Encrypted MSISDN**, you have to URL encode it again. 

#### Sample URL *(With URL encoded Encrypted Number)*
```
https://ideabiz.lk/apicall/balancecheck/v3/etel:9476-vl%251D%25A3%25F7%25AC%25E1%25AE%25C0%25AD%25FF/transactions/amount/balance
```

```
https://ideabiz.lk/apicall/balancecheck/v4/etel:9476-vl%251D%25A3%25F7%25AC%25E1%25AE%25C0%25AD%25FF/transactions/amount/balance
```

*(Refer the Header Enrichment (http://docs.ideabiz.lk/APIs/Header_Enrichment) document) *

Before adding a **Plain Number** to the URL please omit "tel:+"

#### Sample URL *(With Plain Number)*

```
 https://ideabiz.lk/apicall/balancecheck/v3/94766691500/transactions/amount/balance
```

##### Method
```
GET
```

#### Response
```
{
    "endUserId": "94766691500",
    "referenceCode": "DLG212005-1500564823863",
    "accountInfo": {
        "accountType": "PREPAID",
        "accountStatus": "Active",
        "creditLimit": 66.81,
        "balance": 66.81
    }
}
```



<br>
### Response Codes
 <br>
200 – Success!<br>
400 – Bad request; check the error message for details<br>
401 – Authentication failure, check your authentication details<br>
403 – Forbidden; please provide authentication credentials<br>
404 – Not found: mistake in the host or path of the service URI<br>
405 – Method not supported: for example you mistakenly used a HTTP GET instead of a POST<br>
500 – The server encountered an unexpected condition. This could be wrong authentication details or limited user permission<br>
503 – Server busy and service unavailable. Please retry the request.<br>
 


### Exceptions

#### Types

##### Policy Exception

Message Id start with<code>PL</code>

##### Server Exception

Message Id start with <code>SV</code>

 

#### Exception Body
```
{
    "requestError": {
        "serviceException": {
            "messageId": "SVC0002",
            "text": " Invalid input value for message part %1",
            "variables": " clientCorrelator Value 12345"
        }
    }
}
```

## Faults

HTTP Respose code <code>503</code>
 
#### Fault Response Body
```
{
    "fault": {
        "code": "900800",
        "message": "Message Throttled Out",
        "description": "You have exceeded your quota"
    }
}
```

