/*
Title: Touch CEPM
Sort: 
*/

### Content

- Overview
- Method
- Authorization
- Add Employee
- Update Employee Details
- Delete Employee
- Validate Card
- Retrieve Card Info
- Response Codes

### Overview

Dialog Touch - CEPM API allows an application to add, update, delete employee and validate card, retrieve card info. These services are accessible via RESTful web services.

### Methods

The following REST methods are available:

- Send an employee from your Web Application
- Update an employee from your Web Application
- Delete an employee from your Web Application
- Validate card to your Web Application
- Retrieve card info to your Web Application

______________

### Authorization API calls

All API call request to ideabiz.lk required Authorization headers. Please refer ( [http://docs.ideabiz.lk/Getting_Started/Token_Manegment](http://docs.ideabiz.lk/Getting%20Started/Sever%20Authentication)) for the Authorization.

#### Request header

    Content-Type: application/json

    Authorization: Bearer [access token]

    Accept: application/json
_______________

### Add Employee

  This allows you to send an employee from your Web application to Touch – CEPM

#### Request

  Following is a sample request of the service (Add Employee).

#### URL

    https://ideabiz.lk/apicall/CEPM/1.0/employee

#### Method

    PUT

#### Header

    Username: <Username>

    Password: <Password>

    UnitCode: <UnitCode>

**Body**

      {

       "title":"Mr",

       "firstName":"Shashika",

       "lastName";:"Chathumadusha",

       "mobile":"07xxxxxxx",

       "DOB":"17/06/1992",

       "Address1":"No.475",

       "Address2":"Union Place",

       "Address3":"Colombo 02",

       "employeeNo":"07806",

       "IDType":"NIC",

       "IDNo":"999999999V",

       "EPFNo":"07799",

       "key":"EMP001",

       "SBU":"SBU1",

       "addedBy":"user1",

       "status":"1",

       "Branch":"B4"

      }

**Following are the Request parameters of the service (Add Employee).**

| Name | Type | Description | Data Type | Availability | Comment |
| --- | --- | --- | --- | --- | --- |
| Username | Header | Username to login to API | String | Required |   |
| Password | Header | Password to login to API | String | Required |   |
| UnitCode | Header | Unit Code to identify corporate | String | Required |   |
| title | Body | Title of employee | &lt;String&gt; | Required |   |
| firstName | Body | First name | &lt;String&gt; | Required |   |
| lastName | Body | Last Name | &lt;String&gt; | Optional |   |
| mobile | Body | Mobile number | &lt;String&gt; | Optional |   |
| DOB | Body | Date of birth | &lt;String&gt; | Optional | Format: dd/MM/yyyy |
| Address1 | Body | Address line1 | &lt;String&gt; | Optional |   |
| Address2 | Body | Address line2 | &lt;String&gt; | Optional |   |
| Address3 | Body | Address line3 | &lt;String&gt; | Optional |   |
| employeeNo | Body | Employee Number | &lt;String&gt; | Required |   |
| IDType | Body | Type of ID number | &lt;String&gt; | Required | NIC,PP |
| IDNo | Body | ID number | &lt;String&gt; | Required |   |
| EPFNo | Body | EPF number | &lt;String&gt; | Optional |   |
| key | Body | Unique key to identify the employee | &lt;String&gt; | Required |   |
| SBU | Body | SBU | &lt;String&gt; | Required |   |
| addedBy | Body | User who calls this API | &lt;String&gt; | Required |   |
| status | Body | Active/inactive status of employee | &lt;int&gt; | Required | 1-Active0-Inactive |

#### Response

**Following is a sample response of the service (Add Employee).**

    {

       "state" : 1,

       "msg" : "Success",

       "key" : "EMP001"

    }

**Following are the Request parameters of the service (Add Employee).**

| Name | Type | Description | Data Type | Availability | Comment |
| --- | --- | --- | --- | --- | --- |
| state | Body | Status of the Transaction | &lt;int&gt; | Required | See status codes table |
| msg | Body | Response message | &lt;String&gt; | Optional |   |
| key | Body | Unique key to identify the employee | &lt;String&gt; | Required | The key sent when calling API |

______________________

### Update Employee Details

This allows you to update an employee details from your Web application to Touch – CEPM

#### Request

Following is a sample request of the service (Update Employee Details).

#### URL

    https://ideabiz.lk/apicall/CEPM/1.0/employee/{employeeKey}

#### Method

    POST

#### Header

    Username: <Username>

    Password: <Password>

    UnitCode: <UnitCode>

#### Body

        {

         "title":"Mr",

         "firstName":"Shashika",

         "lastName";:"Chathumadusha",

         "mobile":"07xxxxxxx",

         "DOB":"17/06/1992",

         "Address1":"No.475",

         "Address2":"Union Place",

         "Address3":"Colombo 02",

         "employeeNo":"07806",

         "IDType":"NIC",

         "IDNo":"999999999V",

         "EPFNo":"07799",

         "key":"EMP001",

         "SBU":"SBU1",

         "modifiedBy":"user1",

         "status":"1",

         "Branch":"B4"

        }

**Following are the Request parameters of The service (Update Employee Details).**

| Name | Type | Description | Data Type | Availability | Comment |
| --- | --- | --- | --- | --- | --- |
| Username | Header | Username to login to API | String | Required |   |
| Password | Header | Password to login to API | String | Required |   |
| UnitCode | Header | Unit Code to identify corporate | String | Required |   |
| title | Body | Title of employee | &lt;String&gt; | Required |   |
| firstName | Body | First name | &lt;String&gt; | Required |   |
| lastName | Body | Last Name | &lt;String&gt; | Optional |   |
| mobile | Body | Mobile number | &lt;String&gt; | Optional |   |
| DOB | Body | Date of birth | &lt;String&gt; | Optional |   |
| Address1 | Body | Address line1 | &lt;String&gt; | Optional |   |
| Address2 | Body | Address line2 | &lt;String&gt; | Optional |   |
| Address3 | Body | Address line3 | &lt;String&gt; | Optional |   |
| employeeNo | Body | Employee number | &lt;String&gt; | Required |   |
| IDType | Body | Type of ID number | &lt;String&gt; | Required | NIC,PP |
| IDNo | Body | ID number | &lt;String&gt; | Required |   |
| EPFNo | Body | EPF number | &lt;String&gt; | Optional |   |
| key | Body | Unique key to identify the employee | &lt;String&gt; | Required |   |
| SBU | Body | SBU | &lt;String&gt; | Required |   |
| modifiedBy | Body | User who calls this API | &lt;String&gt; | Required |   |
| status | Body | Active/inactive status of employee | &lt;int&gt; | Required | 1-Active0-Inactive |

#### Response

**Following is a sample response of the service (Update Employee Details).**

      {

         "state" : 1,

         "msg" : "Success",

         "key" : "EMP001"

      }

**Following are the Request parameters of the service**

| Name | Type | Description | Data Type | Availability | Comment |
| --- | --- | --- | --- | --- | --- |
| state | Body | Status of the Transaction | &lt;int&gt; | Required | See status codes table |
| msg | Body | Response message | &lt;String&gt; | Optional |   |
| key | Body | Unique key to identify the employee | &lt;String&gt; | Required | The key sent when calling API |

_____________

### Delete Employee

This allows you to delete an employee from your Web application in Touch – CEPM.

#### Request

Following is a sample request of the service (Delete Employee).

#### URL

    https://ideabiz.lk/apicall/CEPM/1.0/employee/{employeeKey}?un={modifiedByUsername}

#### Method

    DELETE

#### Header

      Username: <Username>

      Password: <Password>

      UnitCode: <UnitCode>

______________

### Validate Card

This allows you to validate card information from Touch – CEPM to your Web application.

#### Request

Following is a sample request of the service (Validate Card).

#### URL

    https://ideabiz.lk/apicall/CEPM/1.0/card/validate

#### Method

    POST

#### Header

    Username: <Username>

    Password: <Password>

    UnitCode: <UnitCode>

#### Body

    {

     "cardUID":"777000000006",

     "cardDID":"777000000086",

     "unitCode":"UT00421",

     "serviceCode":"Service001"

    }

**Following are the Request parameters of the service (Validate Card).**

| Name | Type | Description | Data Type | Availability |
| --- | --- | --- | --- | --- |
| Username | Header | Username to login to API | String | Required |
| Password | Header | Password to login to API | String | Required |
| cardUID | Body | Card UID | &lt;String&gt; | Required |
| cardDID | Body | Card DID | &lt;String&gt; | Required |
| unitCode | Body | Unit Code | &lt;String&gt; | Required |
| serviceCode | Body | Service Code | &lt;String&gt; | Required |

#### Response

**Following is a sample response of the service (Validate Card****).**

    {

      "status": "OK",

      "statusCode": "200",

      "statusMsg": [

        "Success"

      ],

      "cardUID": 1,

      "cardDID": 1,

      "unitCode": 1,

      "serviceCode": 1

    }

**Following are the response parameters of the service (Validate Card****).**

| Name | Type | Description | Data Type | Availability | Comment |
| --- | --- | --- | --- | --- | --- |
| status | Body | Status of the transaction | &lt;String&gt; | Required |   |
| statusCode | Body | Status Code of the transaction | &lt;String&gt; | Required |   |
| statusMsg | Body | Status Messages of the transaction | List&lt;String&gt; | Required |   |
| cardUID | Body | Valid Card UID or not | &lt;int&gt; | Required | Valid - 1Invalid - 0 |
| cardDID  | Body | Valid Card DID or not | &lt;int&gt; | Required | Valid - 1Invalid - 0 |
| unitCode | Body | Valid Unit code or not | &lt;int&gt; | Required | Valid - 1Invalid - 0 |
| serviceCode | Body | Valid Service code or not | &lt;int&gt; | Required | Valid - 1Invalid - 0 |

_______________

### Retrieve Card Info

This allows you to retrieve card information from Touch – CEPM to your Web application.

#### Request

Following is a sample request of the service (Retrieve Card Info).

#### URL

    https://ideabiz.lk/apicall/CEPM/1.0/card/retrieve/did

#### Method

    POST

#### Header

    Username: <Username>

    Password: <Password>

    UnitCode: <UnitCode>

#### Body

    {

     "unitCode":"UT02182",

     "cardUID":"77404929478000",

     "serviceCode":"service11";

    }

**Following are the Request parameters of the service (Retrieve Card Info).**

| Name | Type | Description | Data Type | Availability |
| --- | --- | --- | --- | --- |
| Username | Header | Username to login to API | String | Required |
| Password | Header | Password to login to API | String | Required |
| unitCode | Body | Unit Code | &lt;String&gt; | Required |
| cardUID | Body | Card UID | &lt;String&gt; | Required |
| serviceCode | Body | Service Code | &lt;String&gt; | Required |

**Response**

**Following is a sample response of send service (Retrieve Card Info).**

    {

      "status": "OK",

      "statusCode": "200",

      "statusMsg": [

        "Success"

      ],

      "uid": "77404929478000",

      "did": "77404929478",

      "unitCode": 1,

      "cardUID": 1,

      "serviceCode": 1

    }

**Following are the response parameters of the service (Retrieve Card Info).**

| Name | Type | Description | Data Type | Availability | Comment |
| --- | --- | --- | --- | --- | --- |
| status | Body | Status of the transaction | &lt;String&gt; | Required |   |
| statusCode | Body | Status Code of the transaction | &lt;String&gt; | Required |   |
| statusMsg | Body | Status Messages of the transaction | List&lt;String&gt; | Required |   |
| uid | Body | Card UID | &lt;String&gt; | Required |   |
| did | Body | Card DID | &lt;Sting&gt; | Required |   |
| cardUID | Body | Valid Card UID or not | &lt;int&gt; | Required | Valid - 1Invalid - 0 |
| unitCode | Body | Valid Unit code or not | &lt;int&gt; | Required | Valid - 1Invalid - 0 |
| serviceCode | Body | Valid Service code or not | &lt;int&gt; | Required | Valid - 1Invalid - 0 |

________

### Response Codes

**Employee API**

| Code | Msg |
| --- | --- |
| 0 | Success |
| 1 | Unauthorized |
| 2 | Validation Failed |
| 3 | Unknown error |

**Card API**

| statusCode | status |
| --- | --- |
| 200 | Ok |
| 401 | Unauthorized Request |
| 400 | Validation Failure |
| 500 | Unknown error |