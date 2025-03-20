/*
Title: WOM Job (V1)
Sort: 22
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

The WOM Job API allows you to Add, Reschedule, Cancel & Update a job of a work order.

### Method

The following REST methods are available:

   * Adding Jobs
   * Rescheduling Jobs
   * Cancelling Jobs
   * Updating Jobs

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

## Add Job

#### **Request**

Given below is a sample request of the send service.

#### URL
```
https://ideabiz.lk/apicall/WomJob/V1.0/add/{Channel}
```
#### Sample URL
```
https://ideabiz.lk/apicall/WomJob/V1.0/add/WOM
```

#### Method
```
POST
```

#### Body
```
{
	"OrderId": 422,
	"DepotId": 1,
	"TeamId": "TEST_A",
	"StartDateTime": "2017-07-26 17:00:00",
	"EndDateTime": "2017-07-26 17:30:00",
	"JobAddedUser": "WOM_ADMIN",
	"Remark": "Test Job"
}
``` 
<br>
Given below are the Request parameters for WOM Job Addition.
 

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
			<p style="margin-left:6pt;">Longitude</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the Longitude for the job.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Float</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">Latitude</p>
			</td>
			<td>
			<p style="margin-left:6pt;">TThis is the Latitude for the job.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Float</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
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
			<p style="margin-left:6pt;">DepotId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the allocated depot for the job.</p>
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
			<p style="margin-left:6pt;">TeamId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the job allocated team's ID (Uppercase).</p>
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
			<p style="margin-left:6pt;">StartDateTime</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the Start Date and Time for the job.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Date & Time</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Mandatory</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">EndDateTime</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the End Date and Time for the job.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Date & Time</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Mandatory</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">JobAddedUser</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the job added user's ID.</p>
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
			<p style="margin-left:6pt;">Remark</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is to state any remarks for the job.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
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
    "Description": "Successfully Added a job",
    "OrderId": 422,
    "JobId": 814,
    "TeamId": "TEST_A"
}
```

## Job Reschedule

#### **Request**

Given below is a sample request of the send service.

#### URL
```
https://ideabiz.lk/apicall/WomJob/V1.0
```
#### Sample URL
```
https://ideabiz.lk/apicall/WomJob/V1.0/reschedule/WOM
```

#### Method
```
POST
```

#### Body
```
{
	"OrderId": 422,
	"DepotId": 1,
	"JobId": 815,
	"TeamId": "TEST_A",
	"StartDateTime": "2017-07-28 18:00:00",
	"EndDateTime": "2017-07-28 18:30:00",
	"JobAddedUser": "WOM_ADMIN"
}
``` 
<br>
Given below are the Request parameters for Rescheduling a WOM Job.
 

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
			<p style="margin-left:6pt;">Longitude</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the Longitude for the job.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Float</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">Latitude</p>
			</td>
			<td>
			<p style="margin-left:6pt;">TThis is the Latitude for the job.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Float</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
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
			<p style="margin-left:6pt;">DepotId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the allocated depot for the job.</p>
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
			<p style="margin-left:6pt;">JobId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the new Job ID.</p>
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
			<p style="margin-left:6pt;">TeamId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the job allocated team's ID (Uppercase).</p>
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
			<p style="margin-left:6pt;">StartDateTime</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the Start Date and Time for the job.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Date & Time</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Mandatory</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">EndDateTime</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the End Date and Time for the job.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Date & Time</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Mandatory</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">JobAddedUser</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the job added user's ID.</p>
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
			<p style="margin-left:6pt;">Remark</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is to state any remarks for the job.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
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
    "Description": "Successfully Added a job",
    "OrderId": 422,
    "JobId": 816,
    "TeamId": "TEST_A"
}
```

## Job Cancel

#### **Request**

Given below is a sample request of the send service.

#### URL
```
https://ideabiz.lk/apicall/WomJob/V1.0
```
#### Sample URL
```
https://ideabiz.lk/apicall/WomJob/V1.0/cancel/WOM
```

#### Method
```
POST
```

#### Body
```
{
	"OrderId": 422,
	"DepotId": 1,
	"JobId": 816,
	"TeamId": "TEST_A",
	"JobAddedUser": "WOM_ADMIN"
}
``` 
<br>
Given below are the Request parameters for Cancelling a WOM Job.
 

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
			<p style="margin-left:6pt;">Longitude</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the Longitude for the job.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Float</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">Latitude</p>
			</td>
			<td>
			<p style="margin-left:6pt;">TThis is the Latitude for the job.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Float</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
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
			<p style="margin-left:6pt;">DepotId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the allocated depot for the job.</p>
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
			<p style="margin-left:6pt;">JobId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the Job ID.</p>
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
			<p style="margin-left:6pt;">TeamId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the job allocated team's ID (Uppercase).</p>
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
			<p style="margin-left:6pt;">JobAddedUser</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the job added user's ID.</p>
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
			<p style="margin-left:6pt;">Remark</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is to state any remarks for the job.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
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
	"Description": "Successfully Cancelled the job",
	"OrderId": 422,
	"JobId": 1,
	"TeamId": "TEST_A"
}
```

## Job Update

#### **Request**

Given below is a sample request of the send service.

#### URL
```
https://ideabiz.lk/apicall/WomJob/V1.0
```
#### Sample URL
```
https://ideabiz.lk/apicall/WomJob/V1.0/WOM
```

#### Method
```
PUT
```

#### Body
```
{
	"OrderId": 422,
	"DepotId": 1,
	"JobId": 817,
	"TeamId": "TEST_A",
	"JobStatus": "END",
	"JobCompletionStatus": "REVISIT"
}
``` 
<br>
Given below are the Request parameters for Updating a WOM Job.
 

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
			<p style="margin-left:6pt;">Longitude</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the Longitude for the job.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Float</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">Latitude</p>
			</td>
			<td>
			<p style="margin-left:6pt;">TThis is the Latitude for the job.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Float</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
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
			<p style="margin-left:6pt;">DepotId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the allocated depot for the job.</p>
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
			<p style="margin-left:6pt;">JobId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the Job ID.</p>
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
			<p style="margin-left:6pt;">TeamId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the job allocated team's ID (Uppercase).</p>
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
			<p style="margin-left:6pt;">JobStatus</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is to update the job status. The statuses are Dispatch, Start, End.</p>
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
			<p style="margin-left:6pt;">JobCompletionStatus</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is to state the Job Completion Status. The statuses are SUCCESS, DELIVERY FAILED, REVISIT.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Mandatory, if the status is END.</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">Remark</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is to state any remarks for the job.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
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
	"Description": "Successfully updated the job",
	"OrderId": 422,
	"JobId": 816,
	"TeamId": "TEST_A"
}
```


