

Contents

Overview        

Method        

Authorization        

QueryActiveRoamingServices        

Request        

Response        

## Overview

This API is used to get customer&#39;s active roaming package details against mobile number.

## Method

The following REST methods are available:

QueryActiveRoamingServices

## Authorization

**Request Header**

Content-Type: application/json
Authorization: Bearer [access token]
Accept: application/json

## QueryActiveRoamingServices

This API is used to get customer&#39;s active roaming package details. The API will return comma separated roaming package list, credit type, connection type and communication language and status against a mobile number.



### Request

Following is a sample request of send service:

#### URL

[https://ideabiz.lk/apicall/query-active-roaming-services/v1.0/rest/{applicationName}/{requestedUser}/Roaming/Lst/GetActiveRoamingList?mobileNo={mobileNo}](https://ideabiz.lk/apicall/query-active-roaming-services/v1.0/rest/%7BapplicationName%7D/%7BrequestedUser%7D/Roaming/Lst/GetActiveRoamingList?mobileNo=%7BmobileNo%7D)

#### URI Parameters

| Parameter Name | Description | Data Type | Possible Value |
| --- | --- | --- | --- |
| applicationName | Name of the MIFE application | String | CamGeneralMS |
| requestedUser | Requested user | String | Test\_User |

#### Method

	GET

#### Body

	Not applicable

#### Request Parameters

| Parameter Name | Description | Data Type | Mandatory/Optional | Possible Value |
| --- | --- | --- | --- | --- |
| mobileNo | The mobile number of the user | String | Mandatory | 773596360 |



### Response

Following is a sample response of send service:

  {

    &quot;requestStatus&quot;: &quot;OK&quot;,

    &quot;creditType&quot;: &quot;COR&quot;,

    &quot;activeRoamingList&quot;: &quot;I\_AIRA\_AIR,DATA\_ROAM&quot;,

    &quot;connectionType&quot;: &quot;POSTPAID&quot;,

    &quot;communicationLanguage&quot;: &quot;ENG&quot;,

    &quot;status&quot;: &quot;OK&quot;

 	 }

Error scenario:

        {

        &quot;requestStatus&quot; : &quot;LOGIC\_FAILURE&quot;,

	&quot;errorMsg&quot; : &quot;The Network Adapter could not establish the connection&quot;

        }

#### Response parameters

| Parameter Name | Description | Data Type | Mandatory/Optional | Example |
| --- | --- | --- | --- | --- |
| requestStatus | Response status of the API request. Returns OK for success, VALIDATION\_ERROR for incorrect request parameter, LOGIC\_FAILURE for internal logic failure and UNHANDLE\_ERROR for system failure | String | Mandatory | OK |
| creditType | Credit type of the user | String | Optional | COR,GSM,VVP |
| activeRoamingList | The comma separated list of active roaming packages | String | Optional | I\_AIRA\_AIR,DATA\_ROAM |
| connectionType | Connection type of the user. This can be PREPAID or POSTPAID | String | Optional | POSTPAID, PREPAID |
| communicationLanguage | Communication language of the user | String | Optional | ENG |
| status | Status of the response | String | Optional | OK, NOT\_OK |
| errorMsg | Detailed description of the request failure | String | Optional | The network adapter could not establish the connection |

#### Response Codes

200, 400, 401, 403, 404, 405, 500, 503

- 200 � Success!
- 400 � Bad request; check the error message for details
- 401 � Authentication failure, check your authentication details
- 403 � Forbidden; please provide authentication credentials
- 404 � Not found: mistake in the host or path of the service URI
- 405 � Method not supported: for example you mistakenly used a HTTP GET instead of a POST
- 500 � The server encountered an unexpected condition. This could be wrong authentication details or limited user permission
- 503 � Server busy and service unavailable. Please retry the request
