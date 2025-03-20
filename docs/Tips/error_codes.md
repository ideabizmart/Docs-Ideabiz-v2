
### When exceeding throughput limit

```
503
```

```
<?xml version="1.0" encoding="UTF-8"?>
<amt:fault xmlns:amt="http://wso2.org/apimanager/throttling">
   <amt:code>900800</amt:code>
   <amt:message>Message Throttled Out</amt:message>
   <amt:description>You have exceeded your quota</amt:description>
</amt:fault>
```

For more information, Please refer: http://blingtechs.blogspot.com/2017/07/how-you-can-manage-throttle-out-errors.html
 
### When token inactive

```
401
```

```
<?xml version="1.0" encoding="UTF-8"?>
<ams:fault xmlns:ams="http://wso2.org/apimanager/security">
   <ams:code>900904</ams:code>
   <ams:message>Access Token Inactive</ams:message>
   <ams:description>Access failure for API: /testdelay, version: v0.1 with key: 42455834fe7722175561314fbd1f19</ams:description>
</ams:fault>
```
 
### When token expired

```
401
```

```
<?xml version="1.0" encoding="UTF-8"?>
<ams:fault xmlns:ams="http://wso2.org/apimanager/security">
   <ams:code>900903</ams:code>
   <ams:message>Access Token Expired</ams:message>
   <ams:description>Access failure for API: /testdelay, version: v0.1 with key: 16b75a931752aca54348b61d6d7e0</ams:description>
</ams:fault>
``` 

### Other common HTTP Status code
|CODE		| DESCRIPTION																											|
|-----------|-----------------------------------------------------------------------------------------------------------------------|
| 200,201 	| Success! 		 																										|
| 400 		| Bad request; check the error message for details 																		|
| 401 		| Authentication failure, check your authentication details 															|
| 403 		| Forbidden; please provide authentication credentials 																	|
| 404 		| Not found: mistake in the host or path of the service URI 															|
| 405 		| Method not supported: for example you mistakenly used a HTTP GET instead of a POST									|
| 500 		| The server encountered an unexpected condition. This could be wrong authentication details or limited user permission	|
| 503 		| Server busy and service unavailable. Please retry the request.														|
| 502 		| Proxy error. Please retry the request.																				|


**Error Codes related to the SMSmessaging API** (http://docs.ideabiz.lk/APIs/SMS)

<table border="1">
	<thead>
		<tr>
			<th>
			<p><strong>Operator</strong></p>
			</th>
			<th>
			<p><strong>Response Code/DN</strong></p>
			</th>
			<th>
			<p><strong>Description</strong></p>
			</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>
			<p>ALL : SMS SUCCESS but different deliveryStatus</p>
			</td>
			<td>
			<p>200:"DeliveredToNetwork"</br></br>200:"MessageWaiting"</br></br>200:"DeliveryImpossible"</br></br>200:"DeliveryUncertain</br></br>200:"UNSENT""</p>
			</td>
			<td>
			<p>Successful delivery to the network enabler responsible for routing the SMS</br></br>The message is still queued for delivery. This is a temporary state, pending transition to one of the preceeding states</br></br>Unsuccessful delivery;the message could not be delivered before it expired</br></br>Delivery status unknown, eg: because it was handed off to another network</br></br>Errors from Plugin</p>
			</td>
		</tr>
	</tbody>
</table>
</br>
**Error Codes related to the Payment API** (http://docs.ideabiz.lk/APIs/Payment)
</br>
<table border="1">
	<thead>
		<tr>
			<th>
			<p><strong>Operator</strong></p>
			</th>
			<th>
			<p><strong>Response Code/DN</strong></p>
			</th>
			<th>
			<p><strong>Description</strong></p>
			</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>
			<p>ALL: payment SUCCESS but different transactionOperationStatus</p>
			</td>
			<td>
			<p>200:"Charged"</br></br>200:"Refunded"</br></br>200:"insufficient balance"</br></br>200:"System Error"</br></br>200:""</p>
			</td>
			<td>
			<p>Success charged</br></br>Success Refund</br></br></br></br>Invalid MSISDN/Other Business error of OpCo</br></br>Timeout from OpCo</p>
			</td>
		</tr>	
	</tbody>
</table>


