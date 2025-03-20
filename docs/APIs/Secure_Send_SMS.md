/*
Title: SecureSendSMS
Sort: 8
*/

This API will Deduct the mentioned charge from referenced encrypted Jason encoded mobile number .

# Request Information

|API Name|	Securely Send SMs to subscribers with encrypted MSISDNs|
|-------|--------|
|Resource URI|	`https://ideabiz.lk/apicall/securesendsms/<port>/`|
|Request header|	Content-Type: text/plain <br>Authorization: Bearer [access token] <br>Content-Type: application/json<br> Accept: text/plain|
|HTTP Method|	POST|
|Body|	{<br>"msisdn":"vl%1D%A3%F5%AB%E2%A7%C2%A8%FA",<br> " msg":"test SMS",<br> " app_id":"dialog_cc" <br>}|
|URI Parameters|	Port : Ex:- “Dialog”, “5555” , “89867”|

Note: when selecting a port, the API users must register the requesting PORT on ECN (OCS - GITCC) and SMSC (VAS) and get the relevant app_id. 
If not registered, this API won’t deliver the SMS through requested port.<br>

# Request Parameters
|Parameter Name|	Description	Type|	Mandatory/Optional|
|-------|-----|-------|
|msisdn|	This must be encrypted MSISDN with URL encoded with 94 (Ex –“vl%1D%A3%F5%AB%E2%A7%C2%A8%FA”)	|String|	Mandatory|
|msg|	Message Content|	String	|Mandatory|

# Response Information 
|Response|	Securely Send SMS – POST a SM with encrypted MSISDN|
|-----|------|
|HTTP header|	201 – OK, SMS Send Successful <br>400 – Bad request<br> 500 – System error|
|Response|	null|

# Response Parameters 
|Parameter Name|	Description|	Type|
|------|------|-------|
| | | | 
| | | | 
| | | | 
| | | | 

## Appendix






