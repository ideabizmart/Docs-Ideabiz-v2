# IdeaBiz-Request-Handler-JAVA

Github link [https://github.com/DialogIdeaBiz/IdeaBiz-Request-Handler-JAVA](https://github.com/DialogIdeaBiz/IdeaBiz-Request-Handler-JAVA)


This will handle the API cal and also the token. If required,it will refresh the existing token automatically. therefore only need to make an API call via this SDK

## Configuration
* Make **resources/config.properties** writable
* Change resources/config.properties files properties based on your application

Application token Details[ refer Documentation](https://ideabiz.lk/tools/token.html)

To receive a refresh token, you have to use the **token API** with username once [refer Documentation](http://docs.ideabiz.lk/Getting%20Started/Authorization)

## Use
Once config.json is configured, you can import `lk.dialog.ideabiz.javaauth.model.RequestMethod` to your code.Then create `IdeaBizAPIHandler` object using `IdeaBizAPIHandler.java` class then call `sendAPICall` method with correct parameters. 

For example

```
import lk.dialog.ideabiz.javaauth.model.RequestMethod;
```
Creating `IdeaBizAPIHandler` object.
````
        IdeaBizAPIHandler ideaBizAPIHandler = new IdeaBizAPIHandler();
```

## Parameters
### URL
 Complete URL of ideabiz API. Example for SMS `https://ideabiz.lk/apicall/subscription/v1/subscribe`
### Method
 It is a HTTP method. You can use `RequestMethod` Enum for that. This accepts string as well such as "POST and "GET". RequestMethod enum contains

```
RequestMethod::POST
RequestMethod::GET
RequestMethod::DELETE
RequestMethod::PUT

```

### Example code
Please refer `Main.java`
````
 public static void main(String[] args) {
        String url = "https://ideabiz.lk/apicall/subscription/v1/subscribe";
        
        RequestMethod requestMethod = RequestMethod.POST;
        
        IdeaBizAPIHandler ideaBizAPIHandler = new IdeaBizAPIHandler();
        
        String body = "{ "msisdn": ["94777123455] }";

         String urlPra = "";

        try {
          ideaBizAPIHandler.sendAPICall(url,requestMethod,body,urlPra);
        } catch (Exception e) {
            e.printStackTrace();
        }
        
    }
`````



