## Ideabiz Request handler .NET C Sharp

Github link [https://github.com/ideabizlk/IdeaBiz-Request-Handler-CSharp](https://github.com/ideabizlk/IdeaBiz-Request-Handler-CSharp)



This library needs NEWTON JSON library. Please install it
```
https://www.nuget.org/packages/Newtonsoft.Json/
```

### How to

*	Import this code to project
*	Install `Newtonsoft.Json`
*	Generate tokens using https://ideabiz.lk/tools/
*	Save token and credentials to Ideabiz.settings (Can use VisualStudio UI)
*	Create Object
*	Send API call

#### Create Object
```
 IdeabizRequestHandler rh =  new IdeabizRequestHandler();
```

#### Send API call 
Can send API call using `sendAPICall` method

 ```
 IdeabizResponse rsponse = rh.sendAPICall(url, IdeabizAPIAuth.REQUEST_METHOD.POST, body, "application/json","application/json");
 ```

#### Parameters

|Parameters		| Description																			|
|---------------|---------------------------------------------------------------------------------------|
|URL			| Request full URL (eg : https://ideabiz.lk/apicall/smsmessaging/v2/94777123456/request	|
|Method			| IdeabizAPIAuth.REQUEST_METHOD GET OR POST												|
|Body 			| Plain text body. JSON string or urlencoded body										|
|ContentType	| Content type, eg: application/json													|
|Accept			| Accept content type : eg application/json												|

#### Return IdeabizResponse class

|Parameters		| Description						|
|---------------|-----------------------------------|
|Status			| SUCCESS OR ERROR					|
|StatusCode		| HTTP status code (Eg : 200,400)	|
|Body			| API call response					|

#### Saving credentials
This class saveS credentials under settings. If required, save the settings on DB or other data source, please re implement `IdabizAuth.readSettings()` and `IdabizAuth.saveSettings()` method