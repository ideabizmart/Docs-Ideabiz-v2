
## Ideamart
Ideamart is platform that will charge the end user and developer is provided a revenue share of 70%. Application developer do not have to bear cost to start,upload or maintain. 

## Ideabiz
Ideabiz is a platform for corporates. Any application consuming API's in this platform, consumers of the applications will not be charged. Company who use the API's will bear the cost Except payment API. 

As mentioned above there are two different business cases in ideamart and ideabiz. If your company want to move your exisitng application to ideabiz, you have to bear the cost. but you can charge from end user via PaymentAPI if required. 

When you starting ideabiz application you have to sign a paper agreement. But ideamart only need to accept/reject the online agreement. Commitment for Ideabiz from the companies end will be higher. 

In ideamart, platform dont expose any customer numbers (Except special situations agreed and provided a written agreement). In IdeaBiz the customer numbers are exposted to the company and company can decided whether to generate an Anonymous Customer Reference (ACR). 

If you use the actual number of the customer through ideabiz, it means you have that users consent or legal permission to commit operations to that number (ie. Send and receive messages from employees, track locations of your sales force). If you dont have users consent to do such operations you are not authorized to commit such and sole responsibility will be of the company. 

## Programming differences
Ideabiz and ideamart both are use RestFul API's. Request bodies are bit different. Ideabiz most API's compliant with OneAPI standard and also there is totally different mechanism used ideabiz for authentication. In Ideamart you can use the App ID and password inside the json body. but Ideabiz using OAuth2 for authenitcation.

## In ideabiz Authentication
You can generate token via API or dashboard. When you call an API, you have to send that access token in header. It will authenticate and authorize for APIs. This token will expire in a specific time. Please dont create token with very long life time. Because of security issues. Once expire the token, you need to refresh your exisiting token using refresh token.

You can refer Authorization Documentation on docs.ideabiz.lk for how to do that and also some sample SDK is available. You can use that to handle Token.

## How to recieve SMS
In ideamart simply you can config your application via dashboard. But Ideabiz need to verify all serverlets. First you need to request your SMS port and keyword with URL. We will map it after validation and configuring firewalls. 

## How to select SMS port
If you using shared SMS port, you need can request free available SMS port range.Once we provice SMS port list, you can select SMS port and keyword as you like but if you need Unique SMS port, you have to get by paying port charges to Dialog.

You can use long number if you want to send SMS to other networks. 

## How to receive USSD Callback
This also same as above. you can mention callback notify url in a request. But that URL need to inform earlier to us for config firewall and verify

## Sending SMS 
In ideabiz simply you can send SMS to any network. Prices depend on your rate card.
You can send it with long number, SMS port or any alias. But since this was ideabiz, you need to request to us prior configurations. We will go through internal approval and config it

## How to start development
If your company need to consume IdeaBiz api's, simply call us send us an email. We will discuss and provide you a developer account with API you want with limited access. Once you dont it, you can go production with papper agreement


## How to bill
You can select rate card depend on your requirement. End of the month we will send usage bill to you. you can make payment to any dialog outlet or arcade. If you have amount to get from PaymentAPI, we will debit it to you account.

If you want, we can deduct API cosumsion charges (SMS, USSD ...) and can debit rest to your account.