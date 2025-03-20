/*
Title: WOM Team (V1)
Sort: 23
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

The WOM Team API allows you to create teams in handling the work order jobs and to update and view created teams details. 

### Method

The following REST methods are available:

   * Creating Teams
   * Updating Teams
   * Viewing Team Details

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

## Team Creation

#### **Request**

Given below is a sample request of the send service.

#### URL
```
https://ideabiz.lk/apicall/WomTeam/V1.0
```
#### Sample URL
```
https://ideabiz.lk/apicall/WomTeam/V1.0/{TEAMID}/{Channel}
```

#### Method
```
POST
```

#### Body
```
{
	"TeamId": "TEST_A",
	"RegionCode": "COL",
	"TeamLeader": "CLOSED",
	"Ownership": "Contractor",
	"VehicleType": "VAN",
	"DepotId": 1,
	"ContactNumber": "777123456,773333333",
	"ItouchReference": "123456",
	"AgentNic": "871111111V",
	"PmsId": "25412",
	"VehicletrackingId": "123545",
	"Description": "Test Team",
	"Email": "john@d.lk",
	"VehicleRegistrationNo": "251-8989"
}
``` 
<br>
Given below are the Request parameters for WOM Team Creation.
 

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
			<p style="margin-left:6pt;">TeamId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the unique Team ID (Uppercase).</p>
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
			<p style="margin-left:6pt;">RegionCode</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the tagging region code for the creating team. This will be either COL or OUT.</p>
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
			<p style="margin-left:6pt;">TeamLeader</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the team leader's name.</p>
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
			<p style="margin-left:6pt;">Ownership</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is to state the ownership of the creating team. Ownership will be given to either of the Courier Name or Contractor 
Name or Dialog.</p>
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
			<p style="margin-left:6pt;">VehicleType</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is to state the vehicle type for the creating team. Vehicle types are Van, Bike, 3Wheel, Cab and Lorry.</p>
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
			<p style="margin-left:6pt;">DepotId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the allocated depot ID for the team.</p>
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
			<p style="margin-left:6pt;">ContactNumber</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the contact number/numbers for the team. If multiple numbers are entering, they should be comma separated.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">ItouchReference</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is Touch Reference Number.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">AgentNic</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the NIC number of the agent.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">AgentMsisdn</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the agent MSISDN number.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Numeric</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">PmsId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the Partner Management System ID.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">VehicletrackingId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the vehicle tracking ID.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">Description</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is to state a description for the creating team.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">Email</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is to state a email address/addresses for the team.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">VehicleRegistrationNo</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the vehicle registration number.</p>
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
	"Description": "Successfully created the team",
	"TeamId": "TEST_A"
}
```

## Update Team

#### **Request**

Given below is a sample request of the send service.

#### URL
```
https://ideabiz.lk/apicall/WomTeam/V1.0
```
#### Sample URL
```
https://ideabiz.lk/apicall/WomTeam/V1.0/{TEAMID}/{Channel}
```

#### Method
```
PUT
```

#### Body
```
{
	"TeamId": "TEST_A",
	"DepotId": 1,
	"RegionCode": "COL",
	"TeamLeader": "John Snow",
	"Ownership": "Dialog",
	"VehicleType": "VAN",
	"Email": "john@d.lk"
}
``` 
<br>
Given below are the Request parameters for WOM Team Updation.
 

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
			<p style="margin-left:6pt;">TeamId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the unique Team ID (Uppercase).</p>
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
			<p style="margin-left:6pt;">RegionCode</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the tagging region code for the creating team. This will be either COL or OUT.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">TeamLeader</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the team leader's name.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">Ownership</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is to state the ownership of the creating team. Ownership will be given to either of the Courier Name or Contractor 
Name or Dialog.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">VehicleType</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is to state the vehicle type for the creating team. Vehicle types are Van, Bike, 3Wheel, Cab and Lorry.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>		
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">DepotId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the allocated depot ID for the team.</p>
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
			<p style="margin-left:6pt;">ContactNumber</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the contact number/numbers for the team. If multiple numbers are entering, they should be comma separated.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">ItouchReference</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is Touch Reference Number.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">AgentNic</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the NIC number of the agent.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">AgentMsisdn</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the agent MSISDN number.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Numeric</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">PmsId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the Partner Management System ID.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">VehicletrackingId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the vehicle tracking ID.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">Description</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is to state a description for the creating team.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">Email</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is to state a email address/addresses for the team.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">VehicleRegistrationNo</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the vehicle registration number.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">Status</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is to state the status of the team whether it is in active status or not.</p>
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
	"Description": "Successfully Updated the team",
	"TeamId": "TEST_A"
}
```

## Get Team Details

#### **Request**

Given below is a sample request of the send service.

#### URL
```
https://ideabiz.lk/apicall/WomTeam/V1.0
```
#### Sample URL
```
https://ideabiz.lk/apicall/WomTeam/V1.0?DepotId={DEPOTID}&TeamId={TEAMID}&channel={channel}
```

#### Method
```
GET
```

<br>
Given below are the Request parameters in Getting Team Details.
 

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
			<p style="margin-left:6pt;">DepotId</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the depot ID of the team.</p>
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
			<p style="margin-left:6pt;">This is the unique Team ID.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Optional</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">Channel</p>
			</td>
			<td>
			<p style="margin-left:6pt;">This is the indication of the courier channel.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">String</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Mandatory</p>
			</td>
		<tr>	
	<tbody>		
</table>

#### **Response**
```
Status code  : 200
```
```
{
	"Description": "Query Successful",
	"Records": 
	{
		"count": 1,
		"details": [
  
		{
			"TeamId": "TEST_A",
			"Description": "Test Team",
			"RegionCode": "COL",
			"Status": "Y",
			"TeamLeader": "John Snow",
			"ContactNumber": "777123456",
			"Ownership": "Dialog",
			"Email": "john@d.lk",
			"VehicleType": "VAN",
			"VehicleRegistrationNo": "252-8787",
			"DepotId": 1,
			"VehicletrackingId": "5458",
			"PmsId": "12365",
			"AgentMsisdn": 777123456,
			"AgentNic": "871111114V",
			"ItouchReference": "123456"
}
	],
	}
		}
```


