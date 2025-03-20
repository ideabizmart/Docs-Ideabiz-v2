# IdeaBiz PHP sample

Github link [https://github.com/ideabizlk/IdeaBiz-Request-Handler---PHP](https://github.com/ideabizlk/IdeaBiz-Request-Handler---PHP)


This will handle the API call and also the token. If required it will refresh the existing token automatically. Therefore you only need to make an API call via this SDK


## Configuration
* Make **config.json** and **lib/data.json** writable
* Change config.json files properties based on your application


To receive a refresh token, you have to use the **token API** with username once [refer Documentation](http://docs.ideabiz.lk/Getting%20Started/Authorization)

## Use
Once config.json is configured, you can include `IdeaBizAPIHandler.php` to your code. Then call `sendAPICall` method 

For example;

```
include 'IdeaBizAPIHandler.php';
$auth = new IdeaBizAPIHandler();
$out = $auth->sendAPICall($url,RequestMethod::POST,$body);
```

## Parameters
### URL
 Complete the URL of ideabiz API. Example for SMS `https://ideabiz.lk/apicall/smsmessaging/v1/outbound/94777123456/requests`
### Method
 its a HTTP method. you can use `RequestMethod` Enum for that. This accepts string as well such as "POST and "GET". RequestMethod enum contains

```
RequestMethod::POST
RequestMethod::GET
RequestMethod::DELETE
RequestMethod::PUT

```

### Body
This is a plain string that contains any payload. If you require to send an object please `json_encode` it.

```
$out = $auth->sendAPICall($url,RequestMethod::POST,json_encode($obj));

```


## Response
Result returns as array. 

### Success

```
 $result['status'] 
 $result['statusCode'] 
 $result['time']
 $result['header']
 $result['body']

```

#### Status 
Contains OK for success

#### Status Code
Contains http status code. eg : 200, 400 

#### Time
Time taken to complete the request

#### Headers
HTTP headers returned by the server

#### Body
body is in plain text. if you have an object, you can use `json_decode` 



### Error
occurs if the connection fails or when Authentication failures


```
 $result['status'] 
 $result['error'] 
```


#### Status 
The string value "ERROR" is given for the Errors

#### Error
Contains error description

 
### Exceptions
This returns two types of exceptions if any authentication error occurs

its
```
AuthenticationException
ConnectionException
```


### Example code
Please refer `test.php`


