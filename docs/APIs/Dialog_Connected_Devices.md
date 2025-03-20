/*
Title: Dialog Connected Devices
Sort: 8
*/

### Authorization

Dialog Connected Devices Connector uses standard implementation of basic authentication.

### Authorization API calls
All API call request to ideabiz.lk required Authorization headers. Please refer (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) for Authorization


### For developer to provision the callback URL and event

#### URL
```
  https://ideabiz.lk/apicall/Dialog_Connected_Devices/v1.0/rules
```
#### Method
```
POST
```
#### Request
```
Authorization:Bearer [Access Token]
Content-Type:application/json
Accept:text/plain
```  
#### Response
```
{
  "desc": "Successfully processed"
}
```
## Device Controlling
#### URL
```
  https://ideabiz.lk/apicall/Dialog_Connected_Devices/v1.0/process
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
  "serial": "<mac_address>",
  "auth_code": "<auth_code>",
  "action": "<action>"
}
```
#### Response
```
{
  "code":"200",
  "description":"OK"
}

```

##  Device Status

#### URL
```
https://ideabiz.lk/apicall/Dialog_Connected_Devices/v1.0/status
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
  "serial": "<RPI_serial>_<Port_ID>",
  "auth_code": "<auth_code>"
}
```

#### Response
```
{
  "code":"200",
  "description":"OK"
}
```
## Valid Actions

| Action   |      Description      |
|----------|:-------------:|
| ON/OFF |  PRI Digital port control |
| QUERY_INPUT |   RPI query input port status|  
| QUERY |   RPI query logical port |  






