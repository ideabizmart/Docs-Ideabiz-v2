** TOUCH-FUEL – (v 1.0 ) **

**Content**

- Overview
- Method
- Authorization
- Service Login (Get Token)
- Query Transactions Card Details
- Card Status Update
- Exceptions
- Response Codes

**Overview**

Dialog Touch - FQMS API allows an application to query Transactions Card Details and Card Status Update. These services are accessible via RESTful web services.

**Methods**

The following REST methods are available:

- Get transactions details to your Web Application
- Update card status from your Web Application

**Authorization API calls**

All API call request to ideabiz.lk required Authorization headers. Please refer ( [http://docs.ideabiz.lk/Getting\_Started/Token\_Manegment](http://docs.ideabiz.lk/Getting_Started/Token_Manegment)) for the Authorization.

**Request header**

Content-Type: application/json

Authorization: Bearer [access token]

Accept: application/json

**Service Login (Get Token)**

This allows you to get token to call other FQMS API&#39;s from your Web application

**Request**

Following is a sample request of the service (Service Login),

**URL**

https://ideabiz.lk/apicall/touch-fuel/v1.0/serviceLogin

**Method**

POST

**Header**

{

Content-Type: application/json

Accept: application/json

Authorization: Bearer [access token]

}

**Body**

{

"usr": "XXXXX",

"pwd": "xxxxxx"

}

**Following are the Request parameters of the service (Service Login).**

| Name | Type | Description | Data Type | Availability | Comment |
| --- | --- | --- | --- | --- | --- |
| usr | Body | Username to login to the API | String | Required |   |
| pwd | Body | Password  of the user | String | Required |   |

**Response**

**Following is a sample response of the service (Service Login).**

{

   "status" : true,

   "message"; : "success",

   "token" : "ladjaldkfj79874987498574dc792489891"

}

**Following are the Response parameters of the service (****Service Login****).**

| Name | Type | Description | Data Type | Availability | Comment |
| --- | --- | --- | --- | --- | --- |
| status | Body | Operation status | &lt;Boolean&gt; | Required | See operation status |
| message | Body | Service login status message | &lt;String&gt; | Required |   |
| Token | Body | System generated token which needs to use in other operations | &lt;String&gt; | Required |   |

**Query Transaction Card Details**

This allows you to get transaction report from the Touch – FQMS

**Request**

Following is a sample request of the service (Query Transaction Card Details).

**URL**

https://ideabiz.lk/apicall/touch-fuel/v1.0/trnCardDetail

**Method**

POST

**Header**

Accept: application/json

Content-Type: application/json

Authorization: Bearer [access token]

**Body**

{

        "did": "7700003206",
        
        "start_time" : "2016-01-01",
        
        "end_time" : "2016-12-01",
        
        "trn_status" : "2",
        
        "token" : "Od5kQcpflxtHgZsh93BFT0nqA4GD8C",
        
        "brn" : "0011223355"

}



**Following are the Request parameters of the service (****Query Transaction Card Details****).**

| Name | Type | Description | Data Type | Availability | Comment |
| --- | --- | --- | --- | --- | --- |
| did | Body | Card DID number | &lt;String&gt; | Required |   |
| start\_time | Body | Transaction report start time | &lt;String&gt; | Required | yyyy-mm-dd |
| end\_time | Body | Transaction report end time | &lt;String&gt; | Required | yyyy-mm-dd |
| trn\_status | Body | Filtering criteria transaction status | &lt;String&gt; | Required |   |
| token | Body | Above ServiceLogin RESPONSE included token | &lt;String&gt; | Required |   |
| brn | Body | Particular corporate account related Business Registration Number | &lt;String&gt; | Required |   |

**Response**

**Following is a sample response of the service (Query Transaction Card Details).**

{

  			“status ": true,
  			
        "message": “operation Success” ,
        
        "data": [{
        
				"tr_id":"10000002405",
				
				"amount":"119.50",
				
				"liters":"1.000",
				
				"time":"2016-01-06 15:12:35",
				
				"fuel_type":"A DIESEL",
				
				"station":"Luckyplaza",
				
				"terminal":"0000000081004381",
				
				"status":"commit",
				
				"vehicle":"KH-9999",
				
				"super_station_name":"Orange"
				
},
            	
{

				"tr_id":"10000002409",
				
				"amount":"119.50",
				
				"liters":"1.000",
				
				"time":"2016-01-08 10:39:35",
				
				"fuel_type":"A DIESEL",
				
				"station":"Luckyplaza",
				
				"terminal":"0000000081004381",
				
				"status":"commit",
				
				"vehicle":"KH-9999",
				
				"super_station_name":"Orange"

},

},

**Following are the Response parameters of the service**

| Name | Type | Description | Data Type | Availability | Comment |
| --- | --- | --- | --- | --- | --- |
| state | Body | Operation status | &lt;Boolean&gt; | Required |   |
| message | Body | queryTrnCardDetail status message | &lt;String&gt; | Required |   |
| Data | Body | Result set which encoded in Json string. detail of the result set will append following | &lt;Json formatted string&gt; | Required |   |

**Card Status Update**

This allows you to update card status from your Web application.

**Request**

Following is a sample request of the service (Card Status Update).

**URL**

https://ideabiz.lk/apicall/touch-fuel/v1.0/CardStatusChange

**Method**

POST

**Header**

Accept: application/json

Content-Type: application/json

Authorization: Bearer [access token]

**Body**

{

        "did": "7700003206",
        
        "token" : "Od5kQcpflxtHgZsh93BFT0nqA4GD8C",
        
        "status" : 1,
        
        "brn" : "PV123"

}

**Following are the Request parameters of the service (Card Status Update).**

| Name | Type | Description | Data Type | Availability | Comment |
| --- | --- | --- | --- | --- | --- |
| did | Body | Card DID number | &lt;String&gt; | Required |   |
| status | Body | card status to be updated | &lt;int&gt; | Required | Active=1/inactive=0 |
| token | Body | Above ServiceLogin RESPONSE included token | &lt;String&gt; | Required |   |
| brn | Body | Particular corporate account related Business Registration Number | &lt;String&gt; | Required |   |

**Response**

**Following is a sample response of the service (Card Status Update).**

    {

	    "status": true,
	    
	    "message": "operation Success",
	    
	    "data": " "Operation status"

    }

**Following are the Response parameters of the service**

| Name | Type | Description | Data Type | Availability | Comment |
| --- | --- | --- | --- | --- | --- |
| status | Body | Operation status | &lt;Boolean&gt; | Required |   |
| message | Body | CardStatusChange status message | &lt;String&gt; | Required |   |
| data | Body | Json formatted string | &lt;Json formatted string&gt; | Required |   |

**Exception Handling**

Details about HTTP Error codes and the exception handling scenarios

**Response Code**

| statusCode | status |
| --- | --- |
| 200 | Ok |
| 401 | Unauthorized Request |
| 400 | Validation Failure |
| 500 | Unknown error |