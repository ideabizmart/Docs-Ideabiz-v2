#### Getting error response?

Please share below details with ideabiz team via email (support@ideabiz.lk). They will help you to troubleshoot your issue

|Information          	| Value  	|
|-----------------------|-----------|
|Application Name 	  	|        	|
|Working Access token 	|		 	|
|Request Method		  	|        	|
|Ideabiz URL		  	|		   	|
|Request body		  	|		   	|
|Request Headers	  	|		   	|
|Response Code   	  	|		   	|
|Response body		  	|		   	|
|HTTP Request [Optional]|	   		|


#### Sample Query


|Information          	| Value  			|
|-----------------------|-------------------|
|Application Name 	  	| TEST_APP      	|
|Working Access token 	|	8h73hd7dhd7dhdd |
|Request Method		  	| POST	        	|
|Ideabiz URL		  	|	https://ideabiz.lk/apicall/smsmessaging/v2/outbound/87721/requests	   |
|Request body		  	|					|

```
	{
	  "outboundSMSMessageRequest": {
		"address": [
		  "tel:+94777339033"
		],
		"senderAddress": "tel:87721",
		"outboundSMSTextMessage": {
		  "message": "Test Message"
		},
		"clientCorrelator": "123456",
		"senderName": "87721"
	  }
	}
```



*Request Headers*	  						

```
Content-Type: application/json;charset=UTF-8
Accept: application/json
Authorization: Bearer 8h73hd7dhd7dhdd
```

|Response Code   	  	|	401	   			|
|-----------------------|-------------------|
|Response body		  	|		   			|
```
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
    <soapenv:Body>
        <ams:fault xmlns:ams="http://wso2.org/apimanager/security">
            <ams:code>900901</ams:code>
            <ams:message>Invalid Credentials</ams:message>
            <ams:description>Access failure for API: /smsmessaging, version: v2 with key: 8h73hd7dhd7dhdd</ams:description>
        </ams:fault>
    </soapenv:Body>
</soapenv:Envelope>
```


|HTTP Request [Optional]|					|
|-----------------------|-------------------|
```
POST /smsmessaging/v2/outbound/87721/requests HTTP/1.1
Host: {{API}}
Content-Type: application/json;charset=UTF-8
Accept: application/json
Authorization: Bearer 8h73hd7dhd7dhdd
Cache-Control: no-cache
Postman-Token: 077c3451-2614-22fc-66c6-39eb8fa4d539

{
  "outboundSMSMessageRequest": {
	"address": [
	  "tel:+94777339033"
	],
	"senderAddress": "tel:87721",
	"outboundSMSTextMessage": {
	  "message": "Test Message"
	},
	"clientCorrelator": "123456",
	"senderName": "87721"
  }
}
```

