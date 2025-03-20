1. 7Scope

Dialog &quot;securemanagesubscription&quot; API allows you to manage OCS offers (products).This service can be accessed via RESTful web services. Encrypted and non-encrypted MSISDNs are supported.

1. 8References

| Item | Description | Attachments |
| --- | --- | --- |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |

1. 9Abbreviations

For the purposes of the present document, the following abbreviations apply:

| Term | Description |
| --- | --- |
| OCS | Online Charging System |
| CCRM | Converged Customer Relationship Management |
| CRM | Customer Relationship Management |
| SMS | Short Message Service |
| API | Application interface |
|   |   |
|   |   |
|   |   |

1. 1Securemanagesubscription

This API will allows to subscribe/un-subscribe OCS offer (product) for the given MSISDN.

1.
  1.
    1. 9.1.1Request Information

| API Name | Securely activate/deactivate OCS offers subscribers with encrypted/non –encrypted  MSISDNs |
| --- | --- |
| Resource URI | https://ideabiz.lk/apicall/securemanagesubscription/{act,dact} |
| Request header | Content-Type: text/plainAuthorization: Bearer [access token]Content-Type: application/jsonAccept: text/plain |
| HTTP Method | PUT |
| Body | {  &quot;msisdn&quot; : &quot;vl%1D%A3%F5%AB%E2%A7%C2%A8%FA&quot;,  &quot;offer\_id&quot; : &quot;20330&quot;,  &quot;rent&quot; : &quot;10.5&quot;,  &quot;app\_id&quot; : &quot;ID\_WEB\_1&quot;,  &quot;expire\_time&quot; : &quot;20160113150000&quot;,  &quot;channel&quot; : &quot;WEB&quot;} |
| URI Parameters | Command : Values &quot;act&quot; ,&quot;dact&quot; |

1.
  1.
    1. 9.1.2Request Parameters

| Parameter Name | Description | Type | Mandatory/Optional |
| --- | --- | --- | --- |
| msisdn | Subscriber number Ex:- 777123456 or &quot;vl%1D%A3%F5%AB%E2%A7%C2%A8%FA&quot; | String | Mandatory |
| offer\_id | This should specify the offer ID which need to activate or deactivate | String | Mandatory |
| rent | Required rental, this is related to the offer\_id provided | String | Optional |
| app\_id | Related application ID. | String | Optional |
| expire\_time | Expiration date and time. | String | Optional |
| channel | Subscription request triggering channel | String | Mandatory |

1.
  1.
    1. 9.1.3Response Information

| Response | Securely Send SMS – POST a SM with encrypted MSISDN |
| --- | --- |
| HTTP header | 200 – OK, Successfully Activated400 – Bad request409 - Conflict500 – System error |
| Response | {  &quot;desc&quot;: &quot;Operation successfully.&quot;} |

1.
  1.
    1. 9.1.4Response Parameters

| Parameter Name | Description | Type |
| --- | --- | --- |
| desc | Result description | string |
| |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |







1. 10Clarifications/Questions

| Req. Number | Date | By | Description | Status (Open/Close) | Responses |
| --- | --- | --- | --- | --- | --- |
|   |   |   |   |   |   |
|   |   |   |   |   |   |
|   |   |   |   |   |   |
|   |   |   |   |   |   |
|   |   |   |   |   |   |
|   |   |   |   |   |   |
|   |   |   |   |   |   |
|   |   |   |   |   |   |



1. 11Appendix

