

## Content
* [Overview](#overview)
* [Method](#method)
* [Authorization](#authorization-api-calls)
* [Charging a subscriber](#charging-a-subscriber)
* [Response Codes](#response-codes)
* [Exceptions](#exceptions)
* [Faults](#faults)

 

## Overview

The Dialog Payment API allows you to charge mobile subscribers for the use of your application or content. It also allows you to charge a user directly based on their consent. Payment services are accessible via RESTful web services.

### Method

The following REST method is available:

   * Charging a subscriber

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
https://ideabiz.lk/apicall/payment/{version}/{msisdn}/transactions/amount
```
#### Sample URL for V4
```
https://ideabiz.lk/apicall/payment/v4/tel:+94766691500/transactions/amount
```
OR

```
https://ideabiz.lk/apicall/payment/v4/94766691500/transactions/amount
```

#### Method
```
POST
```

#### Body
```
{
    "amountTransaction": {
        "clientCorrelator": "54321",
        "endUserId": "tel:+94766691500",
        "paymentAmount": {
            "chargingInformation": {
                "amount": 1,
                "currency": "LKR",
                "description": "Test Charge"
            },
            "chargingMetaData": {
                "onBehalfOf": "IdeaBiz Test",
                "purchaseCategoryCode": "Service",
                "channel": "WAP",
                "taxAmount": "0",
				"serviceID": "null"
            }  
        },
        "referenceCode": "REF-12345",
        "transactionOperationStatus": "Charged"
    }
}
``` 
<br>
Given below are the Request parameters of the Payment service.
 

<table border>
	<tbody>
		<tr>
			<td>
			<p align="center"><strong>Parameter Name</strong></p>
			</td>
			<td>
			<p align="center"><strong>Description</strong></p>
			</td>
			<td>
			<p align="center"><strong>Type</strong></p>
			</td>
			<td>
			<p align="center"><strong>Mandatory/Optional</strong></p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">clientCorrelator</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This uniquely identifies the Charging request. If there is a communication failure during the charging request, using the same clientCorrelator, when retrying the request, the operator can avoid applying the same charge twice.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">string</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">endUserId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">In this case the endUserId is the user's MSISDN including the 'tel:' protocol identifier and the country code preceded by '+'.</br>Eg: tel:+94766691500.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">string</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Mandatory</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">chargingInformation</p>
			</td>
			<td>
			<p style="margin-left:6pt;">description -This is the text to include, to easily identify what the user has been charged for. This description should be limited to 190 characters.</p>

			<p style="margin-left:6pt;">currency - This is the 3 figure code as per ISO 4217.</p>

			<p style="margin-left:6pt;">amount - This is the actual amount being charged. It can be a whole number or a decimal.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">string</p>

			<p style="margin-left:6pt;">string</p>

			<p style="margin-left:6pt;">decimal</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Mandatory</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">referenceCode</p>
			</td>
			<td>
			<p style="margin-left:6pt;">You can append a reference code for each transaction, to identify or cross check the transactions initiated by your application.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">string</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>

		<tr>
			<td>
			<p style="margin-left:6pt;">chargingMetaData type</p>
			</td>
			<td>
			<p style="margin-left:6pt;">onBehalfOf - Allows aggregators/partners to specify the actual payee.</p>

			<p style="margin-left:6pt;">purchaseCategoryCode - A category defining the type of service, product or media being purchased.(Optional - if you don't have a Category code, please keep this empty or remove this property)</p>

			<p style="margin-left:6pt;">channel - Indicates the source of user interaction.(Optional - to identify in case several channels are available e.g. Web/ Mobile App/ SMS) </p>

			<p style="margin-left:6pt;">taxAmount - Specify as '0' as Tax is already preconfigured in the platform</p>


			<p style="margin-left:6pt;">serviceID - Charging reason code. This will override the default application reason code. (Optional - if you don't have a separate Reason code that prints your service name in the end users bill remove this property)</p>
			</td>
			<td>
			<p style="margin-left:6pt;">string</p>

			<p style="margin-left:6pt;">string</p>
<br>
			<p style="margin-left:6pt;">string</p>

			<p style="margin-left:6pt;">decimal</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
	</tbody>
</table>

### Response
```
Status code  : 201
```
```
{
    "amountTransaction": {
        "clientCorrelator": "54321",
        "endUserId": "tel:+94766691500",
        "paymentAmount": {
            "chargingInformation": {
                "amount": "1",
                "currency": "LKR",
                "description": "Test Charge"
            },
            "totalAmountCharged": "1.0",
            "chargingMetaData": {
                "onBehalfOf": "IdeaBiz Test",
                "purchaseCategoryCode": "Service",
                "channel": "WAP",
                "taxAmount": "0"
            }
        },
        "referenceCode": "REF-12345",
        "transactionOperationStatus": "Charged",
        "serverReferenceCode": "DLG212005-1500564823863",
        "resourceURL": "https://ideabiz.lk/apicall/payment/v2/94766691500/transactions/amount/DLG212005-1500564823863"
    }
}
```
## Check Balance

Please use the balance check API for this purpose. (http://docs.ideabiz.lk/APIs/Balance_Check)


## Retry Payment

When the application needs to retry the same payment, send the same charging request with the same `cleintCorrelator`. The System will then return the last charging status.
</br></br>
Eg : When your application receives an error, you can send the same request. The status of the previous request will be returned.


#### Support
```
SPECIAL CHARGING METHOD (NONETAX)
DEBIT REQUEST 
```

#### Validation
The following will be validated. 
```
endUserId
ClientCorrelator
Method
Amount
```

### Success Response
Success Response same as API success response.

```
Status code  : 201
```

### Error Retry Response
```
{
  "requestError": {
    "serviceException": {
      "messageId": "SVC0002",
      "text": " Invalid input value for message part %1",
      "variables": "Charging operation failed, ClientCorrelator exist and charging data not matched"
    }
  }
}
```

### Enable Charging Retry
Please contact `support@ideabiz.lk` to enable the Charging Retry feature.

### Response Codes
<br>
201 – Success!<br>
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

##### User has insufficient credits for the transaction



#### Exception Body
```
{  
   "requestError":{  
      "policyException":{  
         "messageId": "POL1000",
         "variables":"User has insufficient credit for transaction"
      }
   }
}
```

##### Number is inactive



#### Exception Body
```
{
	"requestError":{
		"serviceException":{
			"messageId":"SVC0270",
			"variables":"Charging operation failed, the charge was not applied"
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
