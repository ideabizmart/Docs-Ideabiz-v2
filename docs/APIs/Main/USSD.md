
# ussd-v1/v3

## Content

 * [Initiating mobile terminated USSD](#initiating-mobile-terminated-ussd)
 * [Receive mobile originated USSD](#receive-moblie-originated-ussd)
 * [Response Codes & Exceptions](#response-codes-&-exceptions)
 * [Exceptions](#exceptions)
 * [Service Exceptions](#service-exceptions)
 * [Policy Exceptions](#policy-exceptions)
 

### Method

The following REST methods are available <br>
* Intiate mobile terminated USSD.
* Receive mobile originated USSD.
* Stop subscription to notifications of MO USSD.

## Authorization API calls
All API call request to ideabiz.lk required Authorization headers. Please refer (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) for Authorization

##### Request Header
<pre>
Content-Type: application/json
Authorization: Bearer [access token]
Accept: application/json
</pre>

### Initiating Mobile Terminated USSD (NI USSD/MT)

Initiates a USSD session with the intended end user. This request would pop up a USSD menu on the end user's device.

#### Request

Following is a sample request of initiating mobile terminated USSD.

##### URL
```sh
https://ideabiz.lk/apicall/ussd/{version}/outbound/{Subscriber Number}
```

##### Method
```sh
POST
```
##### Body
```sh
{
    "outboundUSSDMessageRequest": {
    "address": "tel:+94777123456",
    "shortCode": "tel:1234",
    "keyword": "123",
    "outboundUSSDMessage": "Login to service?\n1. Ok\n2. Cancel",
    "clientCorrelator": "123456",
    "responseRequest": {
      "notifyURL": "http://ussd.response.receive.url ",
      "callbackData": "some-data-useful-to-the-requester"
    },
    "ussdAction": "mtinit"
  }
}
```
Following are the request parameters of initiating mobile terminated USSD. 

<table border="2">
	<tbody>
		<tr>
			<td style="width:213px;">
			<p><strong>Parameter name</strong></p>
			</td>
			<td style="width:213px;">
			<p><strong>Description</strong></p>
			</td>
			<td style="width:213px;">
			<p><strong>Usage</strong></p>
			</td>
		</tr>
		<tr>
			<td style="width:213px;">
			<p>address</p>
			</td>
			<td style="width:213px;">
			<p>The recipients MSISDN including the &lsquo;tel:&rsquo; protocol identifier and the country code preceded by &lsquo;+&rsquo;. i.e., tel:+94770000000.</p>
			</td>
			<td style="width:213px;">
			<p>Mandatory</p>
			</td>
		</tr>
		<tr>
			<td style="width:213px;">
			<p>shortCode</p>
			</td>
			<td style="width:213px;">
			<p>The short assigned to an application. End users dialling this short code should be routed to the corresponding application.</p>

			<p>&nbsp;</p>

			<p>NOTE: The short code can be unique to an application or can be shared my multiple applications</p>
			</td>
			<td style="width:213px;">
			<p>Mandatory</p>
			</td>
		</tr>
		<tr>
			<td style="width:213px;">
			<p>keyword</p>
			</td>
			<td style="width:213px;">
			<p>When a short code is shared among multiple applications, the keyword should be used to uniquely identify the application.</p>

			<p>&nbsp;</p>

			<p>NOTE: This parameter can be empty if the short code is unique for an application</p>
			</td>
			<td style="width:213px;">
			<p>Optional</p>
			</td>
		</tr>
		<tr>
			<td style="width:213px;">
			<p>outboundUSSDMessage</p>
			</td>
			<td style="width:213px;">
			<p>(string) The message to be displayed on the end user device.</p>
			</td>
			<td style="width:213px;">
			<p>Mandatory</p>
			</td>
		</tr>
		<tr>
			<td style="width:213px;">
			<p>clientCorrelator</p>
			</td>
			<td style="width:213px;">
			<p>(string) uniquely identifies this request. If there is a communication failure during the request, using the same clientCorrelator when retrying the request allows the operator to avoid sending duplicate messages to the end user.</p>
			</td>
			<td style="width:213px;">
			<p>Mandatory</p>
			</td>
		</tr>
		<tr>
			<td style="width:213px;">
			<p>ussdAction</p>
			</td>
			<td style="width:213px;">
			<p>The desired USSD action, valid actions are;</p>

			<p>&nbsp;</p>

			<p><strong>mtinit:</strong>&nbsp;Initiate MT USSD session</p>

			<p><strong>mtcont:</strong>&nbsp;Continue a MT USSD session</p>

			<p><strong>moinit:</strong>&nbsp;An USSD session initiated by an end user.</p>

			<p><strong>mocont:</strong>&nbsp;Continue a MO USSD session</p>

			<p><strong>mtfin:</strong>&nbsp;End a USSD Session</p>
			</td>
			<td style="width:213px;">
			<p>Mandatory</p>
			</td>
		</tr>
		<tr>
			<td style="width:213px;">
			<p>notifyURL</p>
			</td>
			<td style="width:213px;">
			<p>(string) If the user responds to the USSD menu, the response should be forwarded to this URL.&nbsp;&nbsp;</p>

			<p>&nbsp;</p>

			<p>NOTE: If this parameter is not set, a subscription to receive MO USSD should be created. If either notifyURL or subscription to receive MO is created, the application has no way of receiving end user input.</p>
			</td>
			<td style="width:213px;">
			<p>Optional</p>
			</td>
		</tr>
		<tr>
			<td style="width:213px;">
			<p>callbackData</p>
			</td>
			<td style="width:213px;">
			<p>(string) will be passed back in this notification. It&rsquo;s used to identify the message the receipt relates to (or any other useful data, such as a function name). This is only valid if notifications are required &ndash; sent with the notifyURL parameter within the responseRequest element.</p>
			</td>
			<td style="width:213px;">
			<p>Optional</p>
			</td>
		</tr>
	</tbody>
</table>

<br>


 

#### Response

Following is a sample response of initiating mobile terminated USSD.
```sh
{
    "outboundUSSDMessageRequest": {
    "address": " tel:+94777123456",
    "keyword": "123",
    "shortCode": "tel:1721",
    "outboundUSSDMessage": " Login to service?\n1. Ok\n2. Cancel ",
    "clientCorrelator": "123456",
    "responseRequest": {
      "notifyURL": "http://ussd.response.receive.url ",
      "callbackData": "some-data-useful-to-the-requester"
    },
    "ussdAction": "mtinit",
    "deliveryStatus": "SENT"
  }
}
```
#### Response Parameters

All the parameters of the original request should be relayed back with the addition of the deliveryStatus parameter.


<table border="" >
	<tbody>
		<tr>
			<td style="width:151px;">
			<p><strong>Parameter Name</strong></p>
			</td>
			<td style="width:354px;">
			<p><strong>Description</strong></p>
			</td>
			<td style="width:133px;">
			<p><strong>Usage</strong></p>
			</td>
		</tr>
		<tr>
			<td style="width:151px;">
			<p>deliveryStatus</p>
			</td>
			<td style="width:354px;">
			<p>The status of the NI USSD command should be sent back in this field. If the request was accepted by the underlying USSD platform, then the value of the parameter will be set to &ldquo;SENT&rdquo;.</p>
			</td>
			<td style="width:133px;">
			<p>Mandatory</p>
			</td>
		</tr>
	</tbody>
	</table>

<br>

#### Receive Response

If the notification URL was set in the responseRequest object in outboundUSSDMessageRequest (Refer Initiating Mobile Terminated USSD (NI USSD/MT)) or a subscription to receive MO USSD was created, then the end users response should be posted to the notification URL.
```sh
 {

    "inboundUSSDMessageRequest": {
    "address": " tel:+94777123456",
    "shortCode": "tel:1721",
    "keyword": "123",
    "inboundUSSDMessage": "2",
    "clientCorrelator": "123456",
    "sessionID": "XXXXXX",
    "responseRequest": {
      "notifyURL": "http://ussd.response.receive.url ",
      "callbackData": "some-data-useful-to-the-requester"
    },
    "ussdAction": "mtcont"
  }
}
```
#### Receive Response Parameters

<table border="1">
	<tbody>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;"><strong>Parameter name</strong></p>
			</td>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-left: none; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><strong><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Description</span></strong></p>
			</td>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-left: none; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><strong><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Usage</span></strong></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">address</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">The recipients MSISDN including the &lsquo;tel:&rsquo; protocol identifier and the country code preceded by &lsquo;+&rsquo;. i.e., tel:+94770000000.</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">sessionID</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">A unique identifier for the session should be returned. Any subsequent communication between MIFE and the application, should contain this session ID.</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">shortCode</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">The short assigned to an application. End users dialling this short code should be routed to the corresponding application.</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;">&nbsp;</p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">NOTE: The short code can be unique to an application or can be shared my multiple applications</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">keyword</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">When a short code is shared among multiple applications, the keyword should be used to uniquely identify the application.</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;">&nbsp;</p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">NOTE: This parameter can be empty if the short code is unique for an application</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Optional</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">inboundUSSDMessage</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">(string) The response message provided by the end user.</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">clientCorrelator</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin: 7.5pt 0in;">(string) The clientCorrelator of the original request should be set here.</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr data-mce-style="mso-yfti-irow: 7; height: 241.6pt;">
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 241.6pt;" style="width:213px;height:322px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">ussdAction</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 241.6pt;" style="width:213px;height:322px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">The desired USSD action, valid actions are;</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;">&nbsp;</p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;"><strong>mtinit:</strong>&nbsp;Initiate MT USSD session</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;"><strong>mtcont:</strong>&nbsp;Continue a MT USSD session</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;"><strong>moinit:</strong>&nbsp;An USSD session initiated by an&nbsp;end user</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;"><strong>mocont:</strong>&nbsp;Continue a MO USSD session</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;"><strong>mtfin:</strong>&nbsp;End a USSD Session</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 241.6pt;" style="width:213px;height:322px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr data-mce-style="mso-yfti-irow: 8; height: 21.55pt;">
			<td colspan="3" data-mce-style="width: 6.65in; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 21.55pt;" style="width:638px;height:29px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">responseRequest</span></p>
			</td>
		</tr>
		<tr data-mce-style="mso-yfti-irow: 9; height: 79.15pt;">
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 79.15pt;" style="width:213px;height:106px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">notifyURL</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 79.15pt;" style="width:213px;height:106px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">(string) If the user responds to the USSD menu, the response should be forwarded to this URL.&nbsp;&nbsp;</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;">&nbsp;</p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;">&nbsp;</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 79.15pt;" style="width:213px;height:106px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Optional</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">callbackData</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">(string) Will be passed back in this notification. It&rsquo;s used to identify the message the receipt relates to (or any other useful data, such as a function name). This is only valid if notifications are required &ndash; sent with the notifyURL parameter within the responseRequest element.</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Optional</span></p>
			</td>
		</tr>
	</tbody>
</table>

<p>&nbsp;</p>

<br>


### Continue USSD Session

Continue an USSD session with the intended end user.

##### Method
```sh
POST
```
##### Request
```sh
{
    "outboundUSSDMessageRequest": {
    "address": "tel:+123456789",
    "shortCode": "tel:1234",
    "keyword": "123",
    "outboundUSSDMessage": "Login to service?\n1. Ok\n2. Cancel",
    "clientCorrelator": "123456",
    "responseRequest": {
      "notifyURL": "http://ussd.response.receive.url ",
      "callbackData": "some-data-useful-to-the-requester"
    },
    "ussdAction": "mtcont"
  }
}

```

<table border="1">
	<tbody>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;"><strong>Parameter name</strong></p>
			</td>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-left: none; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><strong><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Description</span></strong></p>
			</td>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-left: none; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><strong><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Usage</span></strong></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">address</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">The recipients MSISDN including the &lsquo;tel:&rsquo; protocol identifier and the country code preceded by &lsquo;+&rsquo;. i.e., tel:+94770000000.</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">shortCode</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">The short assigned to an application. End users dialling this short code should be routed to the corresponding application.</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;">&nbsp;</p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">NOTE: The short code can be unique to an application or can be shared my multiple applications</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">keyword</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">When a short code is shared among multiple applications, the keyword should be used to uniquely identify the application.</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;">&nbsp;</p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">NOTE: This parameter can be empty if the short code is unique for an application</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Optional</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">outboundUSSDMessage</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">(string) The message to be displayed on the end user device.</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">clientCorrelator</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin: 7.5pt 0in;">(string) uniquely identifies this request. If there is a communication failure during the request, using the same clientCorrelator when retrying the request allows the operator to avoid sending duplicate messages to the end user.</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">ussdAction</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">The desired USSD action, valid actions are;</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;">&nbsp;</p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">mtinit: Initiate MT USSD session</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">mtcont: Continue a MT USSD session</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">moinit: An USSD session initiated by an&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; end user</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">mocont: Continue a MO USSD session</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">mtfin: End a USSD Session</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr>
			<td colspan="3" data-mce-style="width: 6.65in; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:638px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">responseRequest</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">notifyURL</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">(string) If the user responds to the USSD menu, the response should be forwarded to this URL.&nbsp;&nbsp;</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;">&nbsp;</p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">NOTE: If this parameter is not set, a subscription to receive MO USSD should be created. If either notifyURL or subscription to receive MO is created, the application has no way of receiving end user input.</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Optional</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">callbackData</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">(string) will be passed back in this notification. It&rsquo;s used to identify the message the receipt relates to (or any other useful data, such as a function name). This is only valid if notifications are required &ndash; sent with the notifyURL parameter within the responseRequest element.</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Optional</span></p>
			</td>
		</tr>
	</tbody>
</table>

<p>&nbsp;</p>


#### Response

Refer the Receive Response Message for the response body and description.

 

### Receive Mobile Originated USSD

When an end user dials USSD code, if a subscription to receive MO USSD was created for that code, then the request will be posted to the notification URL.

 
```sh
{
    "inboundUSSDMessageRequest": {
    "address": " tel:+94777123456",
    "shortCode": "tel:1721",
    "keyword": "123",
    "inboundUSSDMessage": "2",
    "clientCorrelator": "123456",
    "responseRequest": {
      "notifyURL": "http://ussd.response.receive.url ",
      "callbackData": "some-data-useful-to-the-requester"
    },
    "ussdAction": "mtcont"
  }
}
```
 #### Receive Response Parameters

<table border="1">
	<tbody>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;"><strong>Parameter name</strong></p>
			</td>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-left: none; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><strong><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Description</span></strong></p>
			</td>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-left: none; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><strong><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Usage</span></strong></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">address</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">The recipients MSISDN including the &lsquo;tel:&rsquo; protocol identifier and the country code preceded by &lsquo;+&rsquo;. i.e., tel:+94770000000.</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">sessionID</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">A unique identifier for the session should be returned. Any subsequent communication between MIFE and the application, should contain this session ID.</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">shortCode</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">The short assigned to an application. End users dialling this short code should be routed to the corresponding application.</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;">&nbsp;</p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">NOTE: The short code can be unique to an application or can be shared my multiple applications</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">keyword</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">When a short code is shared among multiple applications, the keyword should be used to uniquely identify the application.</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;">&nbsp;</p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">NOTE: This parameter can be empty if the short code is unique for an application</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Optional</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">inboundUSSDMessage</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">(string) The response message provided by the end user.</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">clientCorrelator</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin: 7.5pt 0in;">(string) The clientCorrelator of the original request should be set here.</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr data-mce-style="mso-yfti-irow: 7; height: 241.6pt;">
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 241.6pt;" style="width:213px;height:322px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">ussdAction</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 241.6pt;" style="width:213px;height:322px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">The desired USSD action, valid actions are;</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;">&nbsp;</p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;"><strong>mtinit:</strong>&nbsp;Initiate MT USSD session</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;"><strong>mtcont:</strong>&nbsp;Continue a MT USSD session</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;"><strong>moinit:</strong>&nbsp;An USSD session initiated by an&nbsp;end user</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;"><strong>mocont:</strong>&nbsp;Continue a MO USSD session</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;"><strong>mtfin:</strong>&nbsp;End a USSD Session</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 241.6pt;" style="width:213px;height:322px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Mandatory</span></p>
			</td>
		</tr>
		<tr data-mce-style="mso-yfti-irow: 8; height: 21.55pt;">
			<td colspan="3" data-mce-style="width: 6.65in; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 21.55pt;" style="width:638px;height:29px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">responseRequest</span></p>
			</td>
		</tr>
		<tr data-mce-style="mso-yfti-irow: 9; height: 79.15pt;">
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 79.15pt;" style="width:213px;height:106px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">notifyURL</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 79.15pt;" style="width:213px;height:106px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">(string) If the user responds to the USSD menu, the response should be forwarded to this URL.&nbsp;&nbsp;</span></p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;">&nbsp;</p>

			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;">&nbsp;</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 79.15pt;" style="width:213px;height:106px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Optional</span></p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 159.6pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">callbackData</p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">(string) Will be passed back in this notification. It&rsquo;s used to identify the message the receipt relates to (or any other useful data, such as a function name). This is only valid if notifications are required &ndash; sent with the notifyURL parameter within the responseRequest element.</span></p>
			</td>
			<td data-mce-style="width: 159.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:213px;">
			<p data-mce-style="margin-top: 7.5pt; margin-right: 0in; margin-bottom: 7.5pt; margin-left: 0in; text-align: justify; line-height: 15.0pt;"><span data-mce-style="mso-fareast-font-family: 'Times New Roman'; mso-bidi-font-family: Helvetica; color: #333333;">Optional</span></p>
			</td>
		</tr>
	</tbody>
</table>

<p>&nbsp;</p>




### Response Codes & Exceptions

##### Response Codes

HTTP response codes should be used to indicate:

200 – Success! <br>
400 – Bad request; check the error message for details <br>
401 – Authentication failure, check your authentication details<br>
403 – Forbidden; please provide authentication credentials<br>
404 – Not found: mistake in the host or path of the service URI<br>
405 – Method not supported: for example you mistakenly used a HTTP GET instead of a POST <br>
500 – The server encountered an unexpected condition. This could be wrong authentication details or limited user permission <br>
503 – Server busy and service unavailable. Please retry the request.
Exceptions<br>

If the transaction is immediately confirmed, the response is displayed as follows:

This section lists the available error codes, the possible reasons why the exception may have occured, and possible solutions.

 

#### Service Exceptions

If the transaction is immediately confirmed, the response is displayed as follows,
```sh
HTTP/1.1404BadRequestAccept: application/json{
  "requestError": {
    "serviceException": {
      "messageId": "SVC0002",
      "text": " Invalid input value for message part %1",
      "variables": " clientCorrelator Value 12345"
    }
  }
}
```

##### Common Service Exceptions


<table border="1">
	<tbody>
		<tr>
		</tr>
		<tr data-mce-style="mso-yfti-irow: 0; mso-yfti-firstrow: yes; height: 26.5pt;">
			<td data-mce-style="width: 3.45in; border: solid windowtext 1.0pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 26.5pt;" style="width:331px;height:35px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">Exception</p>
			</td>
			<td data-mce-style="width: 207.0pt; border: solid windowtext 1.0pt; border-left: none; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 26.5pt;" style="width:276px;height:35px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">Variables</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 3.45in; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:331px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">SVC0001: A service error occurred. Error code is %1</p>
			</td>
			<td data-mce-style="width: 207.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:276px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; explanation of the error</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 3.45in; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:331px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">SVC0002: Invalid input value for message part %1</p>
			</td>
			<td data-mce-style="width: 207.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:276px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; the part of the request that is invalid</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 3.45in; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:331px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">SVC0003: Invalid input value for message part %1, valid values are %2</p>
			</td>
			<td data-mce-style="width: 207.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:276px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; message part</p>

			<p data-mce-style="margin-bottom: 0.0001pt;">%2 &ndash; list of valid values</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 3.45in; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:331px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">SVC0004: No valid addresses provided in message part %1</p>
			</td>
			<td data-mce-style="width: 207.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:276px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; message part</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 3.45in; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:331px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">SVC0005: Correlator %1 specified in message part %2 is a duplicate</p>
			</td>
			<td data-mce-style="width: 207.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:276px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; Correlator</p>

			<p data-mce-style="margin-bottom: 0.0001pt;">%2 &ndash; message part</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 3.45in; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:331px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">SVC0006: Group %1 in message part %2 is not a valid group</p>
			</td>
			<td data-mce-style="width: 207.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:276px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; identifier for the invalid group</p>

			<p data-mce-style="margin-bottom: 0.0001pt;">%2 &ndash; message part</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 3.45in; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:331px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">SVC0007: Invalid charging information</p>
			</td>
			<td data-mce-style="width: 207.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:276px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">none</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 3.45in; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:331px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">SVC0008: Overlapped criteria %1</p>
			</td>
			<td data-mce-style="width: 207.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:276px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 Message Part with the overlapped criteria</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 3.45in; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:331px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">SVC1000: No resources</p>
			</td>
			<td data-mce-style="width: 207.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:276px;">&nbsp;
				<p data-mce-style="margin-bottom: 0.0001pt;">none</p>
			</td>
		</tr>
	</tbody>
</table>


Following service exceptions may be thrown for SMS specifically:


<table border="1" >
	<tbody>
		<tr data-mce-style="mso-yfti-irow: 0; mso-yfti-firstrow: yes; height: 25.15pt;">
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 25.15pt;" style="width:337px;height:34px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">Exception</p>
			</td>
			<td data-mce-style="width: 238.5pt; border: solid windowtext 1.0pt; border-left: none; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 25.15pt;" style="width:318px;height:34px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">Variables</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">SVC0280: Message too long. Maximum length is %1 characters</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; number of characters allowed in a message</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">SCV0283: Delivery receipt notification not supported</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">none</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">SVC0284: Address format is invalid. Expected format is %1</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 - &quot;tel:+94771211212&quot;</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">SVC0285: Message Not Delivered %1</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; Errors occurred while sending the request for all the destinations.</p>
			</td>
		</tr>
	</tbody>
</table>

<p>&nbsp;</p>


Following service exceptions may be thrown for Payment specifically:


<table align="left" border="1">
	<tbody>
		<tr data-mce-style="mso-yfti-irow: 0; mso-yfti-firstrow: yes; height: 25.15pt;">
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 25.15pt;" style="width:337px;height:34px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">Exception</p>
			</td>
			<td data-mce-style="width: 238.5pt; border: solid windowtext 1.0pt; border-left: none; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 25.15pt;" style="width:318px;height:34px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">Variables</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">SVC0270: Charging operation failed, the charge was not applied</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">none</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">SVC0271: endUserId format invalid. Expected format is %1</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 - &quot;tel:+94771211212&quot;</p>
			</td>
		</tr>
	</tbody>
</table>

<p>&nbsp;</p>

<p>&nbsp;</p>

<p>&nbsp;</p>

<div>&nbsp;</div>

 

#### Policy Exceptions

If a request is invalid, a policy exception is thrown.

POL0001 - Policy error occured.

The above exception may be thrown to indicate a fault relating to a policy associated with the service.

 
```sh
HTTP 403Forbidden{
  "requestError": {
    "policyException": {
      "messageId": "POL0001",
      "text": "A policy error occurred. Error code is maxBatchSize exceeded. The maximum allowed maxBatchSize is %1.",
      "variables": "20"
    }
  }
}
```
Following exceptions may be thrown:


<table border="2">
	<tbody>
		<tr data-mce-style="mso-yfti-irow: 0; mso-yfti-firstrow: yes; height: 29.2pt;">
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 29.2pt;" style="width:337px;height:39px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">Exception</p>
			</td>
			<td data-mce-style="width: 238.5pt; border: solid windowtext 1.0pt; border-left: none; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt; height: 29.2pt;" style="width:318px;height:39px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">Variables</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0001: A policy error occurred. Error code is %1</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; explanation of the error</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0002: Privacy verification failed for address %1, request is refused</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; address privacy verification failed for</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0003: Too many addresses specified in message part %1</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; message part</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0004: Unlimited notification request not supported</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">none</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0005: Too many notifications requested</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">none</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0006: Group specified in message part %1 not allowed</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; message part. Note: group means an address which refers to more than one end user</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0007: Nested groups specified in message part %1 not allowed</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; message part. Note: group means an address which refers to more than one end user. Groups cannot contain addresses which are themselves groups.</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0008: Charging is not supported</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">none</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0009: Invalid frequency requested</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">none</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0010: Requested information unavailable as the retention time interval has expired</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">none</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0011: Media type not supported</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">none</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0012: Too many description entries specified in message part %1</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; message part</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0013: Duplicated addresses %1</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; duplicated addresses</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0253: Payment operation refused by user</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">none</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0254: The amount exceeds the operator limit for a single charge</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">none</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0255: Address format invalid. Expected format is %1</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 - &quot;tel:+94771211212&quot;</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0256: Invalid currency specified. %1</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1-Check your SLA for valid currency types</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0257: Message not delivered %1</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1- Request failed. Errors occurred while sending the request for all the destinations</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL0299: Unexpected Errors</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">none</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL1000: User has insufficient credit for transaction</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">none</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL1001: The %1 operator charging limit for this user has been exceeded</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; the time period for which the charging limit has been reached</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL1007: Refunds not supported</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">none</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL1009: User has not been provisioned for %1</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; name of the service</p>
			</td>
		</tr>
		<tr>
			<td data-mce-style="width: 252.9pt; border: solid windowtext 1.0pt; border-top: none; mso-border-top-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:337px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">POL1010: User has been suspended from %1</p>
			</td>
			<td data-mce-style="width: 238.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; mso-border-top-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-alt: solid windowtext .5pt; padding: 0in 5.4pt 0in 5.4pt;" style="width:318px;">
			<p data-mce-style="margin-bottom: 0.0001pt;">%1 &ndash; the name of the service</p>
			</td>
		</tr>
	</tbody>
</table>

<p>&nbsp;</p>


 

 

 