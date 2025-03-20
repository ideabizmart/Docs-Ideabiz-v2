

&nbsp;

#### Step 1 : Create a new collection
*  Go to side bar navigation and click on Add new collection icon.
*  Give a name to the new collection 
*  Click the Normal tab

&nbsp;

#### Step 2 : Select the HTTP method 
 Select the method according to the authorization given.
 
 &nbsp;
 
#### Step 3: Fill all requried fields .
 In the Normal tab requested details should be filled as follows.All the needed information is given on [Autharization](#authorization) documnetation.
##### Request url
 ```sh
 API call url
 ```
 <br>
#### Headers
<table border="1">
	<tbody>
		<tr>
			<td style="width:200px;">
			<p align="center"><strong>Header</strong></p>
			</td>
			<td style="width:200px;">
			<p align="center"><strong>Value</strong></p>
			</td>
		</tr>
		<tr>
			<td style="width:200px;">
			<p align="center">Content-Type</p>
			</td>
			<td style="width:200px;">
			<p align="center">application/x-www-form-urlencoded</p>
			</td>
		</tr>
		<tr>
			<td style="width:200px;">
			<p align="center">Accept</p>
			</td>
			<td style="width:200px;">
			<p align="center">application/x-www-form-urlencoded</p>
			</td>
		</tr>
		<tr>
			<td style="width:200px;">
			<p align="center">Authorization</p>
			</td>
			<td style="width:200px;">
			<p align="center">Bearer&lt;access token&gt;</p>
			</td>
		</tr>
	</tbody>
</table>


In the Authorization header the value should contain the word [Bearer](#)
should be given before typing the access token you genarated.
##### Example
```sh
Bearer e3345db83dbd23ernh626e1fe9cd7a
```

&nbsp;

#### Step 4: Send the request
Fill the raw data field with you request details as the follwing example.
```sh
{
  "outboundSMSMessageRequest": {
    "address": [
      "tel:+94777339033"
    ],
    "senderAddress": "94770777555",
    "outboundSMSTextMessage": {
      "message": "Test Message"
    },
    "clientCorrelator": "654321",
    "receiptRequest": {
      "notifyURL": "http://128.145.147.120:1080/sms/report",
      "callbackData": "some-data-useful-to-the-requester"
    },
    "senderName": "87770"
  }
}
```

Click SEND button.



 
 
 