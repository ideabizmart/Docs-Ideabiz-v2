/*
Title: WOM Work Order Status Update (V1)
*/

## Content
* [Overview](#overview)
* [Method](#method)
* [Authorization](#authorization-api-calls)
* [Charging a subscriber](#charging-a-subscriber)
* [Response Codes](#response-codes)
* [Exceptions](#exceptions)
* [Faults](#faults)

 

## Overview

The WOM work order status API allows you to update the work order status at the completion of a work order job. 

### Method

The following REST method is available:

   * Updating the work order status

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

#### **Request**

Given below is a sample request of the send service.

#### URL
```
https://ideabiz.lk/apicall/WomWoStatusUpdate/V1.0/{channel}
```
#### Sample URL
```
https://ideabiz.lk/apicall/WomWoStatusUpdate/V1.0/WOM
```

#### Method
```
PUT
```

#### Body
```
{
	"OrderId": 421,
	"Status": "CLOSED",
	"ClosedUser": "WOM_ADMIN"
}
``` 
<br>
Given below are the Request parameters for the Work Order Status Update.
 

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
			<p style="margin-left:6pt;">OrderId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the unique work order ID.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Numeric</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Mandatory</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">Status</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the status of the work order. The possible work order statuses are Closed, Closed AP, Closed ERF, Failed, Incomplete, Initial, Assigned and On Hold.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Mandatory</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">ClosedUser</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the work order closed user.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Mandatory, if the status is Closed, Closed AP, Closed ERF, Failed.</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">Reason</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the reason that requires to be mentioned when a work order status is updating as Incomplete, On Hold, Initial, Failed.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Mandatory, if the status is Incomplete, On Hold, Initial, Failed.</p>
			</td>		
		<tr>	
	<tbody>		
</table>

#### **Response**
```
Status code  : 201
```
```
{
    "Description": "Successfully updated the workorder status",
    "OrderId": "421"
}
```
			
			
			
			
			
			