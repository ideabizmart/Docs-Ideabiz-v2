# PHP Classes

Github link [https://github.com/ideabizlk/Classes-PHP](https://github.com/ideabizlk/Classes-PHP)


This contain PHP classes for IdeaBiz API
* SMS Messaging

More will be added soon


# SMS Classes
## Send SMS Request Class
This contain PHP classes for IdeaBiz API
* SMS Messaging

More will be added soon

## Send SMS Request Class
```
include_once 'SMS/SMSMessagingRequest.php';

$s = new  SMSMessagingRequest();
$s->buildRequest("077", "755", "MSG");

echo $s->toJSON();

```

### Build Request method
This simply builds a sms request in one line. If required, you can set properties one by one.

### ToJSON
This return json text of object

## Server SMS response class

```
include_once 'SMS/SMSMessagingResponse.php';
```
