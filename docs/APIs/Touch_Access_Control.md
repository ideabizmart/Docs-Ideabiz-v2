/*
Title: Touch Access Control
Sort: 8
*/

## Authorization

VmsToBoneId Connector uses standard implementation of basic authentication.

### Authorization API calls
All API call request to ideabiz.lk required Authorization headers. Please refer (http://docs.ideabiz.lk/Getting_Started/Token_Manegment)for Authorization


### Access Groups

#### URL
```
  https://ideabiz.lk/apicall/nfc/1.0/accessgroup/getall/{regionId}
```
#### Method
```
GET
```
#### Request
```
  Authorization:Bearer [Access Token]
  Content-Type:application/json
  Accept:application/json
```  
#### Response
```
{
  "plantCode":"A01",
  "accessGroupId":7,
  "accessGroupName":"Red zone",
  "terminalId":27,
  "terminalName":"Main entrance"
}

```
### Visitor Check-In
#### URL
```
  https://ideabiz.lk/apicall/nfc/1.0/visitor/in
```
#### Method
```
  POST
```

#### Headers
```
  Authorization:Bearer [Access Token]
  Content-Type:application/json
  Accept:application/json
```
#### Request
```
{
  "regionId":1,
  "accessgpId":[6,7],
  "cardDid":12937162739,
  "cardUid":819236012632,
  "firstName":"John",
  "middleName":"Dickens",
  "lastName":"Smith",
  "companyId":"AB12987",
  "phone1":"095 3899 898",
  "created":"2016-09-09T10:15:00",
  "activeFrom":"2016-09-09T10:20:00",
  "activeTo":"2016-09-09T16:30:00"
}
```

  |    Field         |    Type                           |    Value                     |
  |------------------|-----------------------------------|------------------------------|
  |    regionId      |    int                            |    Plant code                |
  |    accessgpId    |    int                            |    Access group ID           |
  |    cardDid       |    uint                           |    Card DID                  |
  |    cardUid       |    uint                           |    Card UID                  |
  |    firstName     |    string                         |    Visitor first name        |
  |    middleName    |    string                         |    Visitor middle name       |
  |    lastName      |    string                         |    Visitor last name         |
  |    companyId     |    string                         |    Visitor NIC               |
  |    phone1        |    string                         |    Visit requester mobile    |
  |    created       |    Date and time ISO formatted    |    Visit Date and Time       |
  |    activeFrom    |    Date and time ISO formatted    |    Visit start               |
  |    activeTo      |    Date and time ISO formatted    |    Visit end time            |


#### Response
```
{
  "cardDid":12937162739
  "cardUid":819236012632,
  "enabled":1
}

```
### Visitor Check-Out

#### URL
```
https://ideabiz.lk/apicall/nfc/1.0//visitor/out
```
#### Method
```
  POST
```
#### Headers
```
Authorization:Bearer [Access Token]
Content-Type:application/json
Accept:application/json

```
#### Request
```
{
  "cardDid":3274892738,
  "cardUid":974282323,
  "enabled":0
}

```

#### Response
```
{
  "code":"200",
  "description":"OK"
}

```

