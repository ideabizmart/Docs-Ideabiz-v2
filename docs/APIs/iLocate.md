/*
Title: iLocate APIs
Sort: 8
*/ 

## Overview

iLocate APIs let designated iLocate users to make use of the REST APIs available here and build their own applications without depending on the apps offered with the iLocate packages.

## Methods

Following REST methods are available.<br>
•	getDeviceList <br>
•	getCurrentLocation <br>
•	getLocationHistory <br>
•	getEventLog <br>
•	getDistanceReport <br>
•	getProximityLocations <br>
•	addProximityLocation <br>
•	changeProximityLocation <br>
•	deleteProximityLocation <br>
•	getSharees <br>
•	removeSharee <br>
•	addSharee <br>
•	getDeviceSharees <br>
•	unshareDevice <br>
•	shareDevice <br>
•	updateDevice <br>
•	getNoUpdateAlertSetting <br>
•	setNoUpdateAlert <br>
•	getDeviceChargingMode <br>
•	setDeviceChargingMode<br>
•	getAccountChargingNumber <br>
•	setAccountChargingNumber <br>
•	reactivateDevice <br>
•	subscribeForUpdates <br>
•	unsubscribeFromUpdates <br>
•	subscribeProximityAlerts <br>
•	unsubscribeProximityAlerts <br>



 ## Authorization API calls
All API call request to ideabiz.lk required Authorization headers. Please refer (http://docs.ideabiz.lk/Getting%20Started/Sever%20Authentication) for Authorization


## getDeviceList
Get the list of devices under the customer account

**Request Method**
```
GET
```

**Sample Request**
```
https://ideabiz.lk/apicall/iLocate/v1/getDeviceList/777123456
```
**Sample Response**
```
[{
	"number": "76XXXXXX",
	"name": "Device 1",
	"icon": "5",
	"enabled": true,
	"deviceType": "GPS",
	"paymentStatus": "Active",
	"sharedDevice": false,
	"package": "IL650"
},
{
	"number": "76XXXXXY",
	"name": "Device 2",
	"icon": "3",
	"enabled": true,
	"deviceType": "GPS",
	"paymentStatus": "Active",
	"sharedDevice": false,
	"package": "IL650"
}
}]
```
** Parameters **

|Type|	Name| Description|
|-----|:-------------:|-----|
|OUTPUT|number| MSISDN of the device|
|OUTPUT|name| Short name of the device  |
|OUTPUT|icon| Type of icon that is shown in the map. Client should decide what to show as the icon.  |
|OUTPUT|enabled	|  Whether or not show the device location in the map. Current location will not be available if this is “false”. |
|OUTPUT|deviceType	|  GPS or LBS  |
|OUTPUT|paymentStatus	| Status of the device in terms of rental/query charge payments. Possible values – “Active”, “Grace Period”, “Insufficient Credit”, “CG Failure”|
|OUTPUT|sharedDevice|  “true” if the device is not owned by the customer and shared from another customer’s device list. “false” otherwise. |
|OUTPUT|package	| Name of the rental package  |

## getCurrentLocation
Get the current location of a single device or all the devices under the account.
<br>

** Request Method **
```
GET
```

** Sample Request **
```
https://ideabiz.lk/apicall/iLocate/v1/getCurrentLocation/777123456
```
** Sample Response **
```
[{
	"number": "76XXXXXXX",
	"name": "Device 1",
	"lon": "80.2736608",
	"lat": "6.8873353",
	"icon": "5",
	"state": "on",
	"timestamp": "2015-10-01 11:21:51",
	"paymentStatus": "Active",
	"deviceType": "GPS",
	"restrictedLocation": false
}]

```
** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|number| MSISDN of the device to get the current location. Set this to “all” to get the current location of all devices under the account.|
|OUTPUT|number| MSISDN of the device  |
|OUTPUT|name| Short name of the device  |
|OUTPUT|icon| Type of icon that is shown in the map. Client should decide what to show as the icon. |
|OUTPUT|lat	|  Latitude |
|OUTPUT|lon	| Longitude|
|OUTPUT|state| If the device is in “on” or “off” state. Depending on the Network Signal (LBS) or age of latest location update (GPS). |
|OUTPUT|timestamp| Date and time of the last location update |
|OUTPUT|restrictedLocation|If the device is inside a high-security zone, this will be “true” and lat/lon will be “null”. |
|OUTPUT|deviceType	| GPS or LBS |
|OUTPUT|paymentStatus	|Status of the device in terms of rental/query charge payments. Possible values – “Active”, “Grace Period”, “Insufficient Credit”, “CG Failure”|

##getLocationHistory
Get the location history of a device on a specific date within specified time period.
<br>

** Request Method **

```
GET
```

**Sample Request**
```
https://ideabiz.lk/apicall/iLocate/v1/getLocationHistory/777123456
```
**Sample Response**
```
[
{
	"number": "76XXXXXXX",
	"name": "Device 1",
	"lon": "80.2737212",
	"lat": "6.8872407",
	"state": "on",
	"time": "October 5, 2015, 12:01 AM",
	"timestamp": 1443983495,
	"errorPossible": false
},
]


```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|number|MSISDN of the device to get the location history.|
|INPUT|date| Date of the location data needed. Ex: “2015-10-05”. |
|INPUT|startTime| (Optional) start time of the period. Ex: “09:00:00” |
|INPUT|endTime| (Optional) end time of the period. Ex: “14:00:00” |
|OUTPUT|number|  MSISDN of the device |
|OUTPUT|name| Short name of the device|
|OUTPUT|lat|Latitude|
|OUTPUT|lon| Longitude |
|OUTPUT|state|If the device is in “on” or “off” state. Depending on the Network Signal (LBS) or age of latest location update (GPS).|
|OUTPUT|time|Date and time of the last location update|
|OUTPUT|timestamp|Unix timestamp of the location data|
|OUTPUT|errorPossible|“true” if the location data maybe erroneous based on the validations done by iLocate platform. “false” otherwise.|


## getEventLog

Get the geo-fencing log for a given time period.
<br>

** Request Method **
```
GET
```

** Sample Request **
```
https://ideabiz.lk/apicall/iLocate/v1/getEventLog/777123456
```
** Sample Response **
```
[
{
	"date": "2015-09-23 12:49:54 pm",
	"device": "76XXXXXXX",
	"type": "out",
	"locationId": "304",
	"locationName": "Wellawatta"
},
]

```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|device|(Optional) MSISDN of the device to get the location history. If not set or set to “all” log for all devices will be returned.|
|INPUT|startDate| Starting date of the period. Ex: “2015-10-05”. |
|INPUT|endDate| Ending date of the period. Ex: “2015-10-05”.|
|INPUT|type| (Optional) type of the logs to return. “all”, “in”, or “out”. |
|INPUT|locationId|(Optional) return logs for specified location ID |
|OUTPUT|device|MSISDN of the device|
|OUTPUT|type|“in” or “out”|
|OUTPUT|date| Timestamp of the event|
|OUTPUT|locationId|ID of the proximity location|
|OUTPUT|locationName|Name of the location as defined by the user|


## getDistanceReport
Get the report of approximate distance travelled for a given time period.
<br>

** Request Method **
```
GET
```

** Sample Request **
```
https://ideabiz.lk/apicall/iLocate/v1/getDistanceReport/777123456
```
** Sample Response **
```
[
{
	"date": "2015-09-28",
	"distance": "72.3031"
},
]


```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|device|MSISDN of the device.|
|INPUT|startDate|Starting date of the period. Ex: “2015-10-05”.|
|INPUT|endDate| Ending date of the period. Ex: “2015-10-05”.|
|INPUT|startTime|Start time of the period. Ex: “09:00:00”|
|INPUT|endTime|End time of the period. Ex: “19:00:00”|
|OUTPUT|date|Date|
|OUTPUT|Distance|Distance (approx.) travelled in kilometers.|

## getProximityLocations
Get the list of proximity locations defined under the account.
<br>

** Request Method **
```
GET
```

**Sample Request**
```
https://ideabiz.lk/apicall/iLocate/v1/getProximityLocations/777123456
```
**Sample Response**
```
[
{
	"date": "2015-09-28",
	"distance": "72.3031"
},
]


```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|device|MSISDN of the device.|
|INPUT|startDate|Starting date of the period. Ex: “2015-10-05”.|
|INPUT|endDate| Ending date of the period. Ex: “2015-10-05”.|
|INPUT|startTime|Start time of the period. Ex: “09:00:00”|
|INPUT|endTime|End time of the period. Ex: “19:00:00”|
|OUTPUT|date|Date|
|OUTPUT|Distance|Distance (approx.) travelled in kilometers.|

## addProximityLocation
Add a new proximity location.
<br>

**Request Method**
```
PUT
```

**Sample Request**
```
https://ideabiz.lk/apicall/iLocate/v1/addProximityLocation/777123456
```
**Sample Response**
```
{
	"locationId": "318"
}

```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|locationName|Name of the location.|
|INPUT|lat|Latitude of the location. Ex: “6.2342343”|
|INPUT|lon|Longitude of the location. Ex: “72.3553221”|
|INPUT|radius|Radius for the location in meters. Ex: “100.0”|
|OUTPUT|locationId|ID of the new location|

## changeProximityLocation
Modify existing proximity location.
<br>

**Request Method**
```
POST
```

**Sample Request**
```
https://ideabiz.lk/apicall/iLocate/v1/changeProximityLocation/777123456
```
**Sample Response**
```
{
	"msg": "Successfully updated."
}


```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|locationName|Name of the location.|
|INPUT|lat|Latitude of the location. Ex: “6.2342343”|
|INPUT|lon|Longitude of the location. Ex: “72.3553221”|
|INPUT|radius|Radius for the location in meters. Ex: “100.0”|
|OUTPUT|locationId|ID of the new location|


## deleteProximityLocation
Delete proximity location.
<br>

**Request Method**
```
DELETE
```

**Sample Request**
```
https://ideabiz.lk/apicall/iLocate/v1/deleteProximityLocation/777123456
```
**Sample Response**
```
{
	"msg": "Successfully deleted."
}


```
 
** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|locationId|ID of the location.|
|OUTPUT|msg|“Successfully deleted.” on success. Standard error object otherwise.|

##getSharees
Get the list of designated sharees.
<br>

**Request Method**
```
GET
```

**Sample Request**
```
https://ideabiz.lk/apicall/iLocate/v1/getSharees/777123456
```
**Sample Response**
```
[
{
	"name": "XYZ Corporation",
	"contact": "777678678"
},
]

```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|OUTPUT|name|Name of the customer / sharee|
|OUTPUT|msg|Contact number / username of the sharee|

## removeSharee
Remove customer from designated sharee list
<br>

**Request Method**
```
DELETE
```

**Sample Request**
```
https://ideabiz.lk/apicall/iLocate/v1/removeSharee/777123456
```
**Sample Response**
```
{
	"msg": " Successfully removed."
}
```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|shareeNumber|Contact number / username of the sharee|
|OUTPUT|Msg|“Successfully removed.” on success. Standard error object otherwise.|

## addSharee
Designate a customer as a sharee.
<br>

**Request Method**
```
PUT
```

**Sample Request**
```
https://ideabiz.lk/apicall/iLocate/v1/addSharee/777123456
```
**Sample Response**
```
{
	"name": "XYZ Corporation",
	"contact": "777678678"
}

```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|shareeNumber|Contact number / username of the sharee|
|OUTPUT|name|Name of the new sharee.|
|OUTPUT|contact|Contact number of the new sharee.|

## getDeviceSharees
Get the current sharees of the device.
<br>

**Request Method**
```
GET
```

**Sample Request**
```
https://ideabiz.lk/apicall/iLocate/v1/getDeviceSharees/777123456
```
**Sample Response**
```
{
	"name": "XYZ Corporation",
	"contact": "777678678"
}
```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|device|MSISDN of the device|
|OUTPUT|name|Name of the sharee.|
|OUTPUT|contact|Contact number of the sharee.|

## unshareDevice
Unshare device from a specific sharee.
<br>

** Request Method **
```
DELETE
```

** Sample Request **
```
https://ideabiz.lk/apicall/iLocate/v1/unshareDevice/777123456
```
** Sample Response **
```
{
	"msg": "Successfully unshared"
}

```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|device|MSISDN of the device|
|INPUT|name|Contact / username of the sharee|
|OUTPUT|contact|Contact number of the sharee.|

## shareDevice
Share device from a specific sharee.
<br>

** Request Method **
```
POST
```

** Sample Request **
```
https://ideabiz.lk/apicall/iLocate/v1/shareDevice/777123456
```
** Sample Response  **
```
{
	"name": "XYZ Corporation",
	"contact": "777678678"
}
```
** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|device|MSISDN of the device|
|INPUT|shareeNumber|Contact / username of the sharee|
|OUTPUT|name|Contact Name of the sharee.|
|OUTPUT|contact|Contact number of the sharee.|
 
## updateDevice
Update name, icon, and status of the device.
<br>

** Request Method **
```
POST
```

** Sample Request **
```
https://ideabiz.lk/apicall/iLocate/v1/updateDevice/777123456
```
** Sample Response **
```
{
	"name": "Taxi",
	"icon": "2",
	"enabled": true
}

```
**Parameters**

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|device|MSISDN of the device|
|INPUT|name|New name of the device|
|INPUT|icon|Number of the icon (1 to 15)|
|INPUT|enabled|Whether or not to show the location of the device on the map. true or false|
|OUTPUT|name|Name of the device after the operation|
|OUTPUT|icon|Icon of the device after the operation|
|OUTPUT|enabled|Status of the device after the operation|

## getNoUpdateAlertSetting
Get the setting for No Update Alerts.
<br>

** Request Method **
```
GET
```

** Sample Request **
```
https://ideabiz.lk/apicall/iLocate/v1/getNoUpdateAlertSetting/777123456
```
** Sample Response **
```
{
	"enabled": true,
	"alertInHours": "48"
}


```
**Parameters**

|Type|	Name| Description|
|-----|:------:|-------|
|OUTPUT|enabled|true or false|
|OUTPUT|alertInHours|(Optional) number of hours to wait before sending notification. Only returned if enabled is true.|

 
## SetNoUpdateAlert
Set No Update Alert configuration.
<br>

** Request Method **
```
POST
```

** Sample Request **
```
https://ideabiz.lk/apicall/iLocate/v1/setNoUpdateAlert/777123456
```
** Sample Response **
```
{
	"msg": "Updated successfully"
}

```

** Parameters ** 

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|enabled|true or false|
|INPUT|alertInHours|(Optional) number of hours to wait before sending notification. Only returned if enabled is true.|
|OUTPUT|msg|“Updated successfully” on success. Standard error object otherwise.|

## getDeviceChargingMode
Get the charging mode of the device.
<br>

** Request Method **
```
GET
```

**Sample Request**
```
https://ideabiz.lk/apicall/iLocate/v1/getDeviceChargingMode/777123456
```
**Sample Response**
```
{
	"chargingMode": "THIS_TRACKER",
	"chargingNumber": "76XXXXXXX"
}

```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|device|MSISDN of the device|
|OUTPUT|chargingMode|“OWNER_CHARGE_NUMBER” – charge to owner’s charging number“THIS_TRACKER” – charge to this device’s number<br>“PRIMARY_TRACKER” – charge to the primary device of the account|
|OUTPUT|chargingNumber|The actual number which will be charged as a result of the configured charging mode.|

## setDeviceChargingMode
Get the charging mode of the device.*
<br>

**Request Method**
```
POST
```

**Sample Request**
```
https://ideabiz.lk/apicall/iLocate/v1/setDeviceChargingMode/777123456
```
**Sample Response**
```
{
	"chargingMode": "THIS_TRACKER",
	"chargingNumber": "76XXXXXXX"
}

```
** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|device|MSISDN of the device|
|INPUT|chargingMode|“OWNER_CHARGE_NUMBER” – charge to owner’s charging number “THIS_TRACKER” – charge to this device’s number<br>“PRIMARY_TRACKER” – charge to the primary device of the account|
|OUTPUT|chargingMode|“OWNER_CHARGE_NUMBER” – charge to owner’s charging number “THIS_TRACKER” – charge to this device’s number<br>“PRIMARY_TRACKER” – charge to the primary device of the account|
|OUTPUT|chargingNumber|The actual number which will be charged as a result of the configured charging mode.|

## getAccountChargingNumber
Get the charge number of the account.
<br>

**Request Method**
```
GET
```

**Sample Request**
```
https://ideabiz.lk/apicall/iLocate/v1/getAccountChargingNumber/777123456
```
**Sample Response**
```
{
	"chargingNumber": "777123456"
}

```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|OUTPUT|chargingNumber|Account’s charging number. This is the number gets charged if the charging mode is set to “OWNER_CHARGE_NUMBER”|

## setAccountChargingNumber
Get the charge number of the account.
<br>

** Request Method **
```
POST
```

** Sample Reques t**
```
https://ideabiz.lk/apicall/iLocate/v1/setAccountChargingNumber/777123456
```
** Sample Response **
```
{
	"chargingNumber": "777123456"
}

```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|chargingNumber|Account’s charging number. This is the number gets charged if the charging mode is set to “OWNER_CHARGE_NUMBER”|
|OUTPUT|chargingNumber|Charging number after the operation.|

## reactivateDevice
Reactivate a disabled device after reloading the charging number.
<br>

** Request Method **
```
POST
```

** Sample Request **
```
https://ideabiz.lk/apicall/iLocate/v1/reactivateDevice/777123456
```
**Sample Response**
```
{
	"msg": "Successfully reactivated"
}

```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|number|MSISDN of the device.|
|OUTPUT|msg|"Successfully reactivated" on success. Standard error object otherwise.|

## subscribeForUpdates
Subscribe for location updates.
<br>
 
** Request Method **
```
POST
```

** Sample Request **
```
https://ideabiz.lk/apicall/iLocate/v1/subscribeForUpdates/777123456
```
** Sample Response **
```
{
	"interval": "30",
	"deviceList": "76XXXXXXX,76YYYYYYY",
	"callbackUrl": "http:\/\/locate.dialog.lk\/dummyApiCallback.php"
}


```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|interval|How frequently update should be sent (in minutes)|
|INPUT|callbackUrl|A valid URL to be send the location updates. This is the location of the client’s web service|
|INPUT|deviceList|List of devices to enable location updates. MSISDNs as a comma separated list.|
|OUTPUT|interval|Value after the operation.|
|OUTPUT|callbackUrl|Value after the operation.|
|OUTPUT|deviceList|Value after the operation.|

## unsubscribeForUpdates
Unsubscribe for location updates.
<br>

** Request Method **
```
POST
```

** Sample Request **
```
https://ideabiz.lk/apicall/iLocate/v1/unsubscribeFromUpdates/777123456
```
** Sample Response **
```
{
	"msg": "Successfully unsubscribed"
}

```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|OUTPUT|msg|"Successfully unsubscribed" on success. Standard error object otherwise.|

## SubscribeProximityAlerts
Subscribe for proximity updates.
<br>

** Request Method **
```
POST
```

** Sample Request **
```
https://ideabiz.lk/apicall/iLocate/v1/subscribeProximityAlerts/777123456
```
** Sample Response **
```
{
	"callbackUrl": "http:\/\/locate.dialog.lk\/dummyApiCallback.php"
}

```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|INPUT|callbackUrl|A valid URL to be send the proximity updates. This is the location of the client’s web service.|
|OUTPUT|callbackUrl|Value after the operation.|

##UnsubscribeProximityAlerts
Unsubscribe for proximity updates.
<br>

**Request Method**
```
POST
```

**Sample Request**
```
https://ideabiz.lk/apicall/iLocate/v1/unsubscribeProximityAlerts/777123456
```
**Sample Response**
```
{
	"msg": "Successfully unsubscribed"
}
```

** Parameters **

|Type|	Name| Description|
|-----|:------:|-------|
|OUTPUT|msg|"Successfully unsubscribed" on success. Standard error object otherwise.|

## HTTP Codes
HTTP codes 200 and 201 should be considered as success. Other codes mean failure.

## Error Codes
|Type|	Name|
|-----|------|
| 001  |Device Not Found|
| 002  |Invalid radius|
| 003  |Invalid Lat/Lon value|
| 004  |Error saving proximity location|
| 005  |Proximity location with same name exists|
| 006  |Proximity location does not exist|
| 007  |Error deleting proximity location|
| 008  |Error removing sharee|
| 009  |Sharee entry not found|
| 010  |Error adding sharee|
| 011  |Sharee entry already exists|
| 012  |Sharee / device not found|
| 013  |Error unsharing device|
| 014  |Device not shared with this customer|
| 015  |Device already shared with the customer|
| 016  |Error sharing device|
| 017  |Error updating device|
| 018  |Error updating alert setting|
| 019  |alertInHours should be a positive integer|
| 020  |Error updating account charging number|
| 021  |Charging number should be a Dialog number|
| 022  |Device already active|
| 023  |Unable to reactivate device|
| 024  |One or more devices in the list are not found|
| 025  |Invalid callback URL|
| 026  |Interval should be a positive integer|
| 027  |Interval should be between XX and YY|
| 028  |Error subscribing to location updates|
| 029  |Error unsubscribing from location updates|
| 030  |Error subscribing to proximity alerts|
| 031  |Error unsubscribing to proximity alerts|
| 032  |Subscription blocked by admin|
| 400  |Bad Request|
| 401  |Unauthorized|
| 402  |Payment Required|
| 403  |Forbidden|
| 404  |Not Found|
| 409  |Conflict|
| 500  |Internal Server Error|
| 501  |Not Implemented|