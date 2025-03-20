**TOUCH � TnA CLOUD (GET Transactions)**

**Content**

- Overview
- Method
- Authorization
- Get Transactions
- Response Codes
- Exceptions
- Faults

**Overview**

Dialog Touch � T&amp;A Cloud API allows an application to get all transactions. These services are accessible via RESTful web services.

**Methods**

The following REST methods are available:

- Get all required transactions from T&amp;A Cloud application to your Application

**Authorization API calls**

All API call request to ideabiz.lk required Authorization headers. Please refer ( [http://docs.ideabiz.lk/Getting%20Started/Sever%20Authentication](http://docs.ideabiz.lk/Getting%20Started/Sever%20Authentication)) for the Authorization.

**Request header**

Content-Type: application/json

Authorization: Bearer [access token]

Accept: application/json

**Add Employee**

This allows you to Get all required transactions from T&amp;A Cloud application to your Application

**Request**

Following is a sample request of the service (Get Transactions).

**URL**

https://ideabiz.lk/apicall/touch-tna/v1.0/GetTransactions?pin=&lt;UserID&gt;&amp;pass=&lt;Password&gt;&amp;type=&lt;Transacton\_Types&gt;&amp;id=&lt;Last\_Received\_Transaction \_Id&gt;

**Method**

GET

**Parameters**

**Pin: API User user ID**

**Pass: API User Password**

**Type: Transaction Types**

**{**

                    RDCOK = 0x00,

                    RDCER = 0x01,

                    TRYERR\_R1 = 0x02,

                    DOORSTATE = 0x03,

                    NA\_0x04 = 0x04,

                    BUTTON = 0x05,

                    TOUCH = 0x06,

                    MENACE = 0x07,

                    APB = 0x08,

                    RDC2OK = 0x09,

                    RDC2ER = 0x0A,

                    USERTS = 0x0B,

                    USERERR = 0x0C,

                    AUX1\_ACTIVE = 0x0D,

                    AUX2\_ACTIVE = 0x0E,

                    NA\_0x0F = 0x0F,

                    MENACE\_R1 = 0x10,

                    APB\_R1 = 0x11,

                    USERTS\_R1 = 0x12,

                    USERERR\_R1 = 0x13,

                    MENACE\_R2 = 0x14,

                    APB\_R2 = 0x15,

                    USERTS\_R2 = 0x16,

                    USERERR\_R2 = 0x17,

                    RDC1OK = 0x20,

                    RDC1ER = 0x21,

                    RDC\_ERROR\_DOUBLE = 0x22,

                    RDC\_NOACCESS = 0x24,

                    RDC\_ADD = 0x25,

                    RDC\_DELETE = 0x26,

                    RDCOK\_OUT = 0x27,

                    RDC\_LOCKED = 0x28,

                    RDC1\_LOCKED = 0x29,

                    RDC2\_LOCKED = 0x2A,

                    FPM\_OK = 0x40,

                    FPM\_ERROR = 0x41,

                    FPM\_ERROR\_DOUBLE = 0x42,

                    FPM\_CANCEL = 0x43,

                    FPM\_NOACCESS = 0x44,

                    FPM\_ADD = 0x45,

                    FPM\_DELETE = 0x46,

                    FPM\_ERROR\_TS = 0x48,

                    FPM\_APB = 0x49,

                    FPM\_OK\_OUT = 0x4A,

                    FPM\_LOCKED = 0x4B,

                    PIN\_OK = 0x60,

                    PIN\_ERROR = 0x61,

                    PIN\_ERROR\_DOUBLE = 0x62,

                    PIN\_NOACCESS = 0x64,

                    PIN\_ADD = 0x65,

                    PIN\_DELETE = 0x66,

                    PIN\_CHANGE = 0x67,

                    PIN\_ERROR\_TS = 0x68,

                    PIN\_APB = 0x69,

                    PIN\_OK\_OUT = 0x6A,

                    PIN\_LOCKED = 0x6B,

                    VSN\_RELAY1 = 0x80,

                    VSN\_RELAY2 = 0x81,

                    VSN\_RELAY3 = 0x82,

                    VSN\_OUTPUT1 = 0x83,

                    VSN\_OUTPUT2 = 0x84,

                    VSN\_OUTPUT3 = 0x85,

                    VSN\_OUTPUT4 = 0x86,

                    VSN\_RELAY1\_LOCKED = 0x87,

                    VSN\_RELAY2\_LOCKED = 0x88,

                    VSN\_RELAY3\_LOCKED = 0x89,

                    VSN\_OUTPUT1\_LOCKED = 0x8A,

                    VSN\_OUTPUT2\_LOCKED = 0x8B,

                    VSN\_OUTPUT3\_LOCKED = 0x8C,

                    VSN\_OUTPUT4\_LOCKED = 0x8D,

                    SYS\_BOOT = 0xA0,

                    SYS\_ENTRY = 0xA1,

                    SYS\_CHANGE\_PARAM = 0xA2,

                    SYS\_CHANGE\_TIME = 0xA3,

                    SYS\_RESET\_TRANSACT = 0xA4,

                    SYS\_REBOOT\_START = 0xA5,

                    SYS\_ENTRY\_FAIL = 0xA6,

                    SYS\_RESTART = 0xA7,

                    SYS\_TTL1 = 0xA8,

                    SYS\_TTL2 = 0xA9,

                    SYS\_TTL3 = 0xAA,

                    SYS\_TTL4 = 0xAB,

                    SYS\_INITALIZE = 0xAC,

                    SYS\_DOOR\_OPEN = 0xAD,

                    SYS\_DOOR\_FORCED = 0xAE,

                    SYS\_RELAY1 = 0xB1,

                    SYS\_RELAY2 = 0xB2,

                    SYS\_RELAY3 = 0xB3,

                    SYS\_OUTPUT1 = 0xB4,

                    SYS\_OUTPUT2 = 0xB5,

                    SYS\_OUTPUT3 = 0xB6,

                    SYS\_OUTPUT4 = 0xB7,

                    SYS\_ALARM1 = 0xB8,

                    SYS\_ALARM2 = 0xB9,

                    SYS\_ALARM3 = 0xBA,

                    SYS\_ALARM4 = 0xBB,

                    NULL = 0xFF,

                    JOB\_F1 = 0x01F1, // JOB codes are defined from 0x101, 0xF1 to 0xF4 are function keys

                    JOB\_F2 = 0x01F2,

                    JOB\_F3 = 0x01F3,

                    JOB\_F4 = 0x01F4,

                    JOB\_F5 = 0x01F5,

                    JOB\_F6 = 0x01F6,

                    JOB\_F7 = 0x01F7,

                    JOB\_F8 = 0x01F8

**}**

**ID: Last Received Transaction ID**

**Class structure of received data:**

       publicclassTransaction

       {

             publiclong Id { get; set; }

             publicstring Date { get; set; }

             publicTimeSpan Time { get; set; }

             publicstring NodeId { get; set; }

             publicstring EmployeeNumber { get; set; }

             publicstring Nic { get; set; }

             publicint? PlantCode { get; set; }

             publiculong? CardUid { get; set; }

             publiculong? CardDid { get; set; }

             publicstring FirstName { get; set; }

             publicstring MiddleName { get; set; }

             publicstring LastName { get; set; }

             publicbyte Type { get; set; }

             publicstring TypeName { get; set; }

       }

**Response Codes**

| statusCode | status |
| --- | --- |
| 200 | Ok |
| 401 | Unauthorized Request |
| 400 | Bad Request |
| 500 | Internal Server Error |