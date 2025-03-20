### 1. Create Token

There are two methods of creating tokens. This is a one time process in normal scenarios.


**A. Using an API call (via POSTMAN or your Application)**

**B. Using the Store and Refresh token tool provided.** 



### A. Via API call


#### Via API call using your Application 

Do this only if you don't already have a valid refresh token. This method will require you to provide your Ideabiz Username and password.

But best practice is, to generate a token using refresh token. 


* URL

		https://ideabiz.lk/apicall/token

* Headers

      Content-Type: application/x-www-form-urlencoded
      Authorization: Basic <Authorization code>

##### Authorization Code

The Authorization Code is the base64 encoded string of the following string.
```
    consumer key:consumer secret
```

The consumer key and the consumer secret can be found on the **My Subscriptions** page once you log into ideabiz.lk 

* Sample Headers
```
    Content-Type: application/x-www-form-urlencoded
    Authorization: Basic UWNzRmt6X1hdsfghe4b1RRZlBFRYUMmJTQUZVYTpWWXlkV0VIMzRfTHh2VEV3NUFvUTJsN0FobG9h
```

* Method

	POST

* URL parameter and value

      grant_type : password 
      username : [User Name] 
      password : [Password] 
      scope : PRODUCTION 


Eg:

    https://ideabiz.lk/apicall/token?grant_type=password&username=<USER>&password=<PASSWORD>&scope=PRODUCTION



* Response
```
{
	"scope": "PRODUCTION",
	"token_type": "bearer",
	"expires_in": 3600,
	"refresh_token": "",
	"access_token": ""
}
```


__________


#### Via API call using POSTMAN

In this method Sever Authentication is done by using REST Client such as [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=en)  

Postman can be used to check the connectivity of the sever and responses to the Requests(API calls) you are making.

*Download [Postman](https://www.getpostman.com) 

Input relavent data to fields in the following sample requests.

* URL 

      https://ideabiz.lk/apicall/token?grant_type=password&username=<USER>&password=<PASSWORD>&scope=PRODUCTION

* USER -> Username
* PASSWORD -> Login password


* Headers

      Content-Type: application/x-www-form-urlencoded
      Authorization: Basic <Authorization code> 

Refer [Authorization Code Generation](#authorization-code)

* Method         

		POST

* After you send the request (empty body) you will recive a response as below
* Below success response will confirm that connectivity to the server and sever side functions are working.
```
      {
      "scope": "PRODUCTION",
      "token_type": "bearer",
      "expires_in": 3600,
      "refresh_token": "",
      "access_token": ""
      }
```
       

* Below error respose will appear if there is a issue with username, password, consumer key or consumer secret.
```
      {
        "error": "invalid_client",
        "error_description": "Client Authentication failed."
      }
```
______________











### B. Via Ideabiz Store 

1.You can create Access tokens via Ideabiz Store.

Go to [My Subscriptions](https://www.ideabiz.lk/store/site/pages/subscriptions.jag)

Once you have done that, you will recive the **Access Token** which will expire in 1 hour.



2.Create Refresh token using the ideabiz tool.

Click **Create Refresh Token** or Visit [Ideabiz Tools](https://ideabiz.lk/tools) 






____________________
_________________________










### 2. Refreshing Tokens 

Once the access token expires you will get an error response.

Eg:

       900903
       Access Token Expired
       Access Token has expired. Renew the access token.

When this happens, you must make the following API call to refresh the access token. For this you will require the refresh token, since both the refresh token and the access token are coupled. The currently active refresh token that you received in step 1 ) is used to create a new access token.
</br></br>
##### **NOTE: Please note that the token should be refreshed ONLY when the existing token expires.**
**This process of token renewing can be automated, please refer the below link for sample PHP source code.**<br>
https://github.com/ideabizlk/IdeaBiz-Request-Handler---PHP

This is a continuous process

* URL

		https://ideabiz.lk/apicall/token

* Method

		POST

* URL parameter and value

      grant_type : refresh_token
      refresh_token : <Refresh token generated in Step 1>
      scope : PRODUCTION

Eg :

      https://ideabiz.lk/apicall/token?grant_type=refresh_token&refresh_token=<Refresh 		Token>&scope=PRODUCTION

* Headers

      Content-Type: application/x-www-form-urlencoded
      Authorization: Basic <Authorization code>

Refer [Authorization Code Generation](#authorization-code)

* Response
```
      {
      "scope": "PRODUCTION",
      "token_type": "bearer",
      "expires_in": 3600,
      "refresh_token": "",
      "access_token": ""
      }
```




### Responses

#### Token expired


HTTP Status 
```
401
```

```
<?xml version="1.0" encoding="UTF-8"?>
<ams:fault xmlns:ams="http://wso2.org/apimanager/security">
   <ams:code>900903</ams:code>
   <ams:message>Access Token Expired</ams:message>
   <ams:description>Access failure for API: /payment, version: v2</ams:description>
</ams:fault>
```


#### Token Inactive


HTTP Status 
```
401
```

```
<?xml version="1.0" encoding="UTF-8"?>
<ams:fault xmlns:ams="http://wso2.org/apimanager/security">
   <ams:code>900904</ams:code>
   <ams:message>Access Token Inactive</ams:message>
   <ams:description>Access failure for API: /balancecheck, version: v2</ams:description>
</ams:fault>
```
_____________________







