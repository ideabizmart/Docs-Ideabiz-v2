/*
ProxyNumberMapping
Sort: 8
*/

## Content
* [Overview](#overview)
* [Method](#method)
* [Subscribing New Users](#subscribing-new-users)
* [Removing the added Users](#removing-the-added-users)
* [Response Codes](#response-codes)
* [Faults](#faults)

 

## Overview

The anonymous call API provides the user the ability to map two mobile numbers together to a proxy number, whereby either of the numbers mapped can initiate calls to each other by dialing the proxy number.
This will provide the end user to make a call in a secure way by protecting their identity


### Method

The following REST methods are available:

   * Subscribing New Users.
   * Removing the added Users.

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



## Subscribing New Users

This allows you to add two new users for a service provided by your application. All charges are Taxable, and Standard Taxes will be charged from the subscriber  on top of the charging amount.
</br></br>


#### **Request**

Given below is a sample request of the API call.

#### URL
```
https://ideabiz.lk/apicall/pnm/{version}
```
#### Sample URL
```
https://ideabiz.lk/apicall/pnm/v1.0
```

#### Method
```
POST
```

#### Body
```
  {
    "a_party": "94xxxxxxxxx",
    "a_party_ref_id": "12345",
    "a_party_type": "D",
    "b_party": "94xxxxxxxxx",
    "b_party_ref_id": "98765",
    "b_party_type": "C",
    "short_code": "43098",
    "time_out_period": "600",
    "reference_id": "1234_bb"
  }
``` 

### Response
```
Status code  : 200
```
```
{
    "response": "Both users entered Successfully"
}
```
<br>
## Removing the added Users

This allows you to remove the existing user pair.
</br></br>


#### **Request**

Given below is a sample request of the API call.

#### URL
```
https://ideabiz.lk/apicall/pnm/{version}/drop-user
```
#### Sample URL
```
https://ideabiz.lk/apicall/pnm/v1.0/drop-user
```

#### Method
```
POST
```

#### Body
```
{
  "reference_id": "1234_bb"
}
``` 

### Response
```
Status code  : 200
```
```
{
    "response": "users successfully removed"
}
```
<br>
Given below are the Request parameters of the service.
 

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
			<p style="margin-left:6pt;">a_party/b_party</p>
			</td>
			<td>
			<p style="margin-left:6pt;">In this case the a_party/b_party are the userss MSISDNs including the country code. Eg:94766691500.</p>
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
			<p style="margin-left:6pt;">a_party_ref_id</p>
			</td>
			<td>
			<p style="margin-left:6pt;">An unique reference for the User that identifies the User.</p>
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
			<p style="margin-left:6pt;">b_party_ref_id</p>
			</td>
			<td>
			<p style="margin-left:6pt;">An unique reference for the User that identifies the User.</p>
			<td>
			<p style="margin-left:6pt;">string</p>
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
			<p style="margin-left:6pt;">a/b_party_type</p>
			</td>
			<td>
			<p style="margin-left:6pt;">D for Driver / C for Customer</p>
			<td>
			<p style="margin-left:6pt;">string</p>
            </td>
			<td>
			<p style="margin-left:6pt;">Mandatory</p>
			</td>
		</tr>
        <tr>
			<td>
			<p style="margin-left:6pt;">Short Code</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Short code allocated for the app</p>
			<td>
			<p style="margin-left:6pt;">string</p>
            </td>
			<td>
			<p style="margin-left:6pt;">Mandatory</p>
			</td>
		</tr>
        <tr>
			<td>
			<p style="margin-left:6pt;">time_out_period</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Time duration in seconds [ 60 - for time out in 1 minute ]</p>
			<td>
			<p style="margin-left:6pt;">string</p>
            </td>
			<td>
			<p style="margin-left:6pt;">Mandatory</p>
			</td>
		</tr>
        <tr>
			<td>
			<p style="margin-left:6pt;">reference_id</p>
			</td>
			<td>
			<p style="margin-left:6pt;">An unique reference id for scheduled job</p>
			<td>
			<p style="margin-left:6pt;">string</p>
            </td>
			<td>
			<p style="margin-left:6pt;">Mandatory</p>
			</td>
		</tr>
	</tbody>
</table>




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
