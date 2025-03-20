/*
Title: Activate Internet Cards
Sort: 2
*/


1. Summary        3

2. Internet Card        3

	2.1 Activate Internet Cards        3

	2.1.1        Request Information        3

	2.1.2        Request Parameters        3

	2.1.3        Response Information        4

	2.1.4        Response Parameters        4



#### Summary


An API to activate internet cards is described here for both Live and Test OCS platforms.

1. 2.Internet Card
  1. 2.1Activate Internet Cards
    1. 2.1.1Request Information

| API Name | Subscribe to a Product |
| --- | --- |
| Resource URI | Live Number: [https://ideabiz.lk/apicall/activateInternetCard/v1.0/&lt;msisdn&gt;/&lt;Internet\_card\_Value](https://mife.dialog.lk/apicall/activateInternetCard/v1.0/%3cmsisdn%3e/%3cInternet_card_Value)&gt;Test Number: [https://ideabiz.lk/apicall/activateInternetCard/v1.0/771231234/29](https://ideabiz.lk/apicall/activateInternetCard/v1.0/771231234/29) |
| Request header | Content-Type: application/jsonAccept: application/jsonAuthorization: Bearer cf25fd547cc93d4b5ce4d783a469ea7f |
| HTTP Method | GET |

_________



#### Request Parameters

| Parameter Name | Description | Type |
| --- | --- | --- |
| msisdn | The requested mobile number | String |
| Internet Card Value | The Rs. Value of internet card | String |



#### Response Information

| Response |   |
| --- | --- |
| HTTP header | **200** OK **406** Not acceptable (Invalid inputs) **407** Invalid system ID **408** Balance not sufficient **500** Internal Error |
| Response |  {&quot;code&quot;: 7000&quot;description&quot;: &quot;OK&quot;} |


#### Response Parameters

| Parameter Name | Description | Type |
| --- | --- | --- |
| code | Code for the error that occurred | String |
| Description | The description of the particular error that occurred | String |



| code | Description |
| --- | --- |
| 7001 | Product already subscribed |
| 7002 | System error |
| 7003 | Sorry, your request cannot be processed. You have already received the maximum allowed advanced reload amount |
| 7004 | Advance reload error |
| 7005 | Buspos error |
| 7006 | Insufficient balance |
| 7007 | Balance query successful (7007|a008980980) |
| 7008 | Product Removal fail |
| 7010   | Wrong request format |
| 7011 | CX alert Error |
| 7013 | No matching account |
| 70077 | Balance for general bal |