/*
Title: BillininfoAPI
Sort: 
*/

### Overview
    Dialog BillInfoAPI provides end users to extract all the billing related details and the service can be accessed via a RESTful web service. 

### Method
    The following REST methods are available:
    Get All Billing information


### Authorization API calls
    All API call request to ideabiz.lk required Authorization headers. Please refer (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) for Authorization


### Request Header
```sh
Authorization: Bearer [access token]
Accept: application/json
```


## Get All Billing information
This method allows you to get all the billing information of the customer.

### Request


#### URL
```
https://ideabiz.lk/apicall/crm/api/billing/v1.0.0/connections/{refaccount}/bill/info?lob={lob}
```

#### URI Parameters 
|Parameter  | Description	|
|-----------|---------------|
|refaccount	| Mobile Number |

#### Method
```
GET
```
 

Response
```
{
    "getAllBillingInfoResponse": {
      "last_bill": "3616.29",
      "last_bill_date": "2014-12-05 00:00:00",
      "min_payment": "250.0",
      "outstanding": "4915.9043",
      "outstanding_date": "2014-12-30 16:32:33",
      "result": "0",
      "running_bill": "2190.2373",
      "running_bill_date": "2014-12-30 16:32:33"
    }
}
```

