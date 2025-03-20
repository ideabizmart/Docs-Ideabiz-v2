


<p>
Ideabiz platform can execute the payment API calls on behalf of the Service provider.

In this case the Service provider doesn't need to subscribe to Payment API or balance check APIs.

Service provider doesn't have to manage the charging schedule in their system/application either

Both these functions will be carried out automatically by ideabiz platform for prepaid or postpaid users.

Service provider needs to inform below pre-configurations to ideabiz team.
</p>

```
1. Charging recurrence
2. Amount to be charged
3. Revenue Share agreed
```
<br>


<p>
Based on these configurations whenever a new subscriber consents to activating a service, ideabiz platform will trigger balance check, charging, and specified schedule. Service provider will get a notification via <a href="http://docs.ideabiz.lk/Go_Live/Admin_API_v2">Admin API</a> whenever there is a status change.
</P>


<b>Note that fall back charging is not possible in this scenario.</b>
<br>
<h3>
Sample parameters
</h3>
<br>


| Rental Status 	|	 Subscription Status 	| Scenario 		|  		|
| --------- | ----------- | --------- | :--------- |
| SUCCESS | SUBSCRIBED | Initial Subscription | User was charged successfully on subscription. Ideabiz will retry charging as per charging recurrence configured | 
| SUCCESS | SUBSCRIBED | On Renewal cycles | User was charged successfully after a period of failed payment/s. If the user was kept on blocked status, Service provider can allow access now. |
| FAILED | FAILED | Initial Subscription | User was not charged on subscription. Ideabiz will NOT retry to charge this user again. |
| FAILED | SUBSCRIBED | On Renewal cycles | User was not charged on renewal attempts - Ideabiz will retry charging as per charging recurrence. Service provider can decide whether to wait for a payment SUCCESS in the next charging cycle/s or block service for the user |
<br>
<br>

Please see the Admin API documentation for implementation of <a href='http://docs.ideabiz.lk/Go_Live/Admin_API_v2'>Admin API</a> and more info on capturing these responses.
