/*
Title: Governance API
Sort: 8
*/

This API allows to filter inappropriate content through out your application

### Authorization API calls
All API call request to ideabiz.lk required Authorization headers. Please refer [http://docs.ideabiz.lk/Getting_Started/Token_Manegment](http://docs.ideabiz.lk/Getting_Started/API_config) for Authorization

### Request header
```
Content-Type: application/json 
Authorization: Bearer [access token] 
Accept: application/json
```

## Governance Check

#### URL
```
https://ideabiz.lk/apicall/governance/v1/check
```

#### METHOD
```
POST
```

#### Request
```
{
  "governanceRequest": {
    "phase": "word1 word. sentense"
  }
}
```

#### Response
```
{
  "governanceResponse": {
    "phase": "word1 word. sentense",
    "status": "BLOCKED"
  }
}
```

### governanceResponse > status

| Status        		| Description     						|
| ----------------------|---------------------------------------|
| OK 					| No Governance word found				|
| BLOCKED  				| Found Governance word					|