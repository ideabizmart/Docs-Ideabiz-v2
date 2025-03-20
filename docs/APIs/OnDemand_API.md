## Content
* [Overview](#overview)
* [Response](#response)
* [Notification Request](#request)

## Overview
The OnDemand API acts as a middleware handling inbound SMS and USSD requests from Dialog customer's to send their location to third-party service providers.

## Response
Following is a sample response from the OnDemand API once the user's location is retrieved.
```sh
{
  "msisdn":"tel:+94771234567",
  "port":"7752",
  "keyword":"149",
  "latitude":"0",
  "longitude":"0",
  "accessorMethod":"ussd",
  "accuracy":"10"
}
```
<br>
Following are the Response parameters of the OnDemand service.


<table border>
	<tbody>
		<tr>
			<td>
			<p align="center"><strong>Parameter name</strong></p>
			</td>
			<td>
			<p align="center"><strong>Description</strong></p>
			</td>
			<td>
			<p align="center"><strong>Type</strong></p>
			</td>
			<td>
			<p align="center"><strong>Mandatory/Optional</strong></p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">msisdn</p>
			</td>
			<td>
			<p style="margin-left:6pt;">MSISDN of the mobile terminal to locate. in this case the recipients MSISDN including the &lsquo;tel:&rsquo; protocol identifier and the country code preceded by &lsquo;+&rsquo;.</p>

			<p style="margin-left:6pt;">i.e. tel:+ 94771234567.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">&nbsp;string</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Mandatory</p>
			</td>
		</tr>
		<tr>
        			<td>
        			<p style="margin-left:6pt;">port</p>
        			</td>
        			<td>
        			<p style="margin-left:6pt;">The port assigned to an application. End users dialling this short code should be routed to the corresponding application.
                                                
                                                 
                                                
                                                NOTE: The short code can be unique to an application or can be shared my multiple applications</p>
        			</td>
        			<td>
        			<p style="margin-left:6pt;">&nbsp;string</p>
        			</td>
        			<td>
        			<p style="margin-left:6pt;">Mandatory</p>
        			</td>
        		</tr>
        		<tr>
                			<td>
                			<p style="margin-left:6pt;">keyword</p>
                			</td>
                			<td>
                			<p style="margin-left:6pt;">When a short code is shared among multiple applications, the keyword should be used to uniquely identify the application.
                                                        
                                                         
                      NOTE: This parameter can be empty if the short code is unique for an application</p>
                			</td>
                			<td>
                			<p style="margin-left:6pt;">&nbsp;string</p>
                			</td>
                			<td>
                			<p style="margin-left:6pt;">Mandatory</p>
                			</td>
                		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">latitude</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Decimal Degrees, ISO 6709.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">&nbsp;string</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Mandatory</p>
			</td>
		</tr>
		<tr>
			<td>
			<p style="margin-left:6pt;">longitude</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Decimal Degrees, ISO 6709.</p>
			</td>
			<td>
			<p style="margin-left:6pt;">&nbsp;string</p>
			</td>
			<td>
			<p style="margin-left:6pt;">Mandatory</p>
			</td>
		</tr>
		<tr>
        			<td>
        			<p style="margin-left:6pt;">accessorMethod</p>
        			</td>
        			<td>
        			<p style="margin-left:6pt;">The method through which the API was accessed.(Either USSD or SMS)</p>
        			</td>
        			<td>
        			<p style="margin-left:6pt;">&nbsp;string</p>
        			</td>
        			<td>
        			<p style="margin-left:6pt;">Mandatory</p>
        			</td>
        		</tr>
                <tr>
                <td>
                <p style="margin-left:6pt;">accuracy</p>
                </td>
                <td>
                <p style="margin-left:6pt;">Result accuracy in metres.</p>
                </td>
                <td>
                <p style="margin-left:6pt;">&nbsp;string</p>
                </td>
                <td>
                <p style="margin-left:6pt;">Mandatory</p>
                </td>
                </tr>
	</tbody>
</table>

 <br>
 
## Request (Optional)
Once the OnDemand API has notified the service provider, a notification can be sent back to the API to either send a SMS to the user or continue the USSD session(Diverting it to the service provider's end).

A sample request is given below:
```sh
{
"ussd": {
"message" :"Received the location",
"callbackURL": "https://apps.ideabiz.lk/ussd/"
},
"sms":{
"message" : "Nearby restaurants: KFC, McDonald's",
"callbackURL": "https://apps.ideabiz.lk/sms/",
"port": "87756"
}
}
```
If the service provider intends to send an USSD along with a SMS it is possible if the request above is sent. However if the request is not sent, a pre-defined message will be sent by the API.
If the service provider decides to continue the USSD session, the response will be similar to that of the USSD API.(http://docs.ideabiz.lk/APIs/USSD)

### States of Parameters</br>
States of the Request parameters of send service.
<br>
<table border="1">
	<thead>
		<tr>
			<th>
			<p><strong>Parameter Name</strong></p>
			</th>
			<th>
			<p><strong>Description</strong></p>
			</th>
			<th>
			<p><strong>Type</strong></p>
			</th>
			<th>
			<p><strong>Mandatory /Optional</strong></p>
			</th>
		</tr>
	</thead>
	<tbody>
	<tr>
    			<td>
    			<p>ussd: message</p>
    			</td>
    			<td>
    			<p>The message to be displayed on the end user device.</p>
    			</td>
    			<td>
    			<p>string</p>
    			</td>
    			<td>
    			<p>Mandatory</p>
    			</td>
    		</tr>
    		<tr>
    			<td>
    			<p>ussd: callbackURL</p>
    			</td>
    			<td>
    			<p>If the user responds to the USSD menu, the response should be forwarded to this URL.&nbsp;&nbsp;</p>
    			</td>
    			<td>
    			<p>string</p>
    			</td>
    			<td>
    			<p>Optional.Keep empty or null if you don't want delivery notifications.</p>
    			</td>
    		</tr>
		<tr>
			<td>
			<p>sms: message</p>
			</td>
			<td>
			<p>Must be provided within the message element. Messages over 160 characters may end up being sent as two or more messages by the operator.</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Mandatory</p>
			</td>
		</tr>
		<tr>
			<td>
			<p>sms: callbackURL</p>
			</td>
			<td>
			<p>The URL to which you would like to receive a notification of delivery of SMS.</br></br>The format of this notification is shown below.</p>
			</td>
			<td>
			<p>string</p>
			</td>
			<td>
			<p>Optional.Keep empty or null if you don't want delivery notifications.</p>
			</td>
		</tr>
	     <tr>
    			<td>
    			<p>sms: port</p>
    			</td>
    			<td>
    			<p>This is the Port number configured for the API.If a valid port is not sent, the OnDemand API will not dispatch the message to the recipient.</br></p>
    			</td>
    			<td>
    			<p>string</p>
    			</td>
    			<td>
    			<p>Mandatory</p>
    			</td>
    		</tr>
		
	</tbody>
</table>

<br>
