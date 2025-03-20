/*
Title: Roaming
Sort: 8
*/

## Authorization

Authorization API calls
All API call request to ideabiz.lk required Authorization headers. Please refer (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) for Authorization 

## Query Roaming rates.

API will returns the current roaming rates from the submitted Local Location to Destination Location.

**Method**
```
GET
```
**URL**
```
https://ideabiz.lk/apicall/roaminginfor/v0.1/<MCC>/<MNC>/<MSISDN>/<Home Location ID>/<Local Location ID>/<Destination Location ID>
```


**Parameter Description**

| Parameter        | Description          |
| ------------- |-----------------------| 
| MCC 			| Mobile country code  	|
| MNC 			| Mobile network code  |
| MSISDN 		| MSISDN of the mobile customer  |
| Home Location ID | Actual location of the MSISDN |
| Local Location ID | Currently available location | 
| Destination Location ID | Destination to be dialed (Optional) |

** Note **

- If the `Destination Location ID` is set to `null`
  <br>The API will return   the Roaming rates for transactions from Local Location to Home Location.
- If the  `Destination Location ID` is set accordingly 
  <br>The API will return   the Roaming rates for transactions from Local Location to Destination Location.
- If the `Home Location` is same as the `Local location`
  <br>The API will return the domestic rates of the home location.


** Response body **

```
{
  "VISITED COUNTRY": "China",
  "VISITED OPERATOR": "All",
  "CALLED COUNTRY": {
    "DEFAULT": {
      "WITHOUTTAX": {
        "MOIDD": "250",
        "UCB": "51"
      },
      "WITHTAX": {
        "MOIDD": "250",
        "UCB": "51"
      }
    },
    "OCOR": {
      "WITHOUTTAX": {
        "MOIDD": "250",
        "UCB": "51"
      },
      "WITHTAX": {
        "MOIDD": "250",
        "UCB": "51"
      }
    }
  }
}
```


**Response Parameters**

| Parameter        | Description          |
| -----------------|-----------------------| 
| DEFAULT		   | Package Name 		|
| WITHOUTTAX		| Rate without tax	|
| MOIDD, UCB		| Service types array |
| WITHTAX		| Rate with tax	|


## Query Roaming Usage

API will return the roaming usage details of the given time period of the submitted msisdn.
Return data will categorize as `Voice` / `SMS`/ `Data` and the Total usage in rupees and Usage count will be displayed for each category. Response will be in json format.  


**Method**
```
POST
```
**URL**
```
https://ideabiz.lk/apicall/roaminginfor/v0.1/<MCC>/<MNC>/<MSISDN>
```

**POST Body parameters**
``` 
{
	"startdate":"20151023",
	"enddate":"20151123"
}
```

**Parameter Description**

| Parameter        | Description          |
| ------------- |-----------------------| 
| MCC 			| Mobile country code  	|
| MNC 			| Mobile network code  |
| MSISDN 		| MSISDN of the mobile customer  |
| startdate 	| Starting timestamp for the information query  |
| enddate		| Ending timestamp for the information query.|



**Response Body Format**  
```
[
  "777123456",
  {
    "data": "3.5624",
    "voice": "6536.774",
    "sms": "344.385"
  }
]
```


## Query Account Information##


API will return the below information relates to the submitted msisdn.
For both account information and the Roaming product information can be queried via above API. . Response will be in json format (key->value).


**Method**
```
GET
```
**URL**
```
https://ideabiz.lk/apicall/roamingservice/v0.1/<MCC>/<MNC>/<msisdn>
```

**Parameter Description**

| Parameter        | Description        |
| -------------|-----------------------| 
| MCC 			| Mobile country code  	|
| MNC 			| Mobile network code  |
| MSISDN 		| MSISDN of the mobile customer  |



** Response body format **
```
{
  "paidMode": "POS",
  "creditbalance": 0,
  "unbilledValue": 12675.9002,
  "accountvalidity": "0",
  "mainpackage": "STAFF-STAFF PACKAGE",
  "raompackage": [
    "I_AIRA_AIR",
    "I_OCOR",
    "I_ROAM_ALERT"
  ],
  "allroamingpkg": [
    "I_AIR_NO_DEP",
    "I_DF_CORP_BB_UN",
    "I_DF_BB_DAILY",
    "I_OCOR",
    "I_DF_SMRT_RM_D",
    "I_DF_SMRT_RM_M",
    "I_ROAM_ALERT"
  ]
}
```

**Response Parameters**

| Parameter        | Description          |
| -----------------|-----------------------| 
| paidMode 			| `PPP` for pre-paid connection <br> `POS` for Post Paid connection |
| creditbalance 	|  For the prepaid Users only |
| accountvalidity	| For prepaid users only |
| unbilledValue		| For postpaid users only |
| mainpackage		| Current main Package |
| roampackage		| Subscribed Roaming package |



## Roaming service Activation ##

Above API will activate the submitter Roaming service product ID to the submitted msisdn.

**Method**
```
POST
```
**URL**
```
https://ideabiz.lk/apicall/roamingservice/v0.1/<MCC>/<MNC>/<msisdn>/< PackageID >/< Operation>

```

**Parameter Description**

| Parameter        | Description          |
| ------------- |-----------------------| 
| MCC 			| Mobile country code  	|
| MNC 			| Mobile network code  |
| MSISDN 		| MSISDN of the mobile customer  |
| PacakgeID		| Roaming Package ID   |
| Operation		| `C` – Add Roaming service <br> `T` – Delete Roaming service |


** Response Body Format **  
```
{
  "status": "200",
  "desc": "successful"
}
```

## Response Codes 

Following standard response codes will be used in the REST response header.  

| Code  			| Description |
|---------			| ------ |
| 200	          |'OK' |
| 201	          |'Created' |
| 202	          |'Accepted' |
| 203	          |'Non-Authoritative Information' |
| 204	          |'No Content' |
| 205	          |'Reset Content' |
| 206	          |'Partial Content' |
| 207	          |'Already Have' |
| 400	          |'Bad Request' |
| 401	          |'Unauthorized' |
| 403	          |'Forbidden' |
| 404	          |'Not Found' |
| 405	          |'Method Not Allowed' |
| 406     	  	  |'Not Acceptable' |
| 407	          |'Proxy Authentication Required' |
| 408	          |'Request Timeout' |
| 409	          |'Conflict' |
| 410	          |'Gone' |
| 500	          |'Internal Server Error' |
| 501	          |'Not Implemented' |
| 503	          |'Server Busy'