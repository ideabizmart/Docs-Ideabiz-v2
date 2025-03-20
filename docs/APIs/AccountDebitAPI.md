/*
Title: AccountDebitAPI
Sort: 8
*/


This API will allow pre-registered users to perform debit request from any Dialog GSM, CDMA, LTE or DTV prepaid account.<br><br>
At the time of user registration (Registration is a manual configuration) each user will be given a level of minimum authorization and Type of accounts allowed for debiting (GSM, CDMA, LTE or DTV prepaid) <br><br>
Users are allowed to do a debit request for allowed type of accounts with upper authorization level than assigned level at the registration.<br><br>
For GSM or CDMA account types the same number is used to send SMSs to get customer consent and to notify transaction detail<br><br>
In the case of LTE and DTV prepaid SMSs will be sent o notify number (Only Dialog will be allowed, if notify number is other operator number continue to debit without notify or customer consent??)<br><br>
Customer consent should be given within one minute from SMS dispatch to proceeds debit request, otherwise request is rejected.<br><br>
In the request user can instruct to apply or not to apply taxes on the charge amount. If user haven mentioned it in the request taxes will be added to the amount by default
<br><br>

 ## Authorization API calls
All API call request to ideabiz.lk required Authorization headers. Please refer (http://docs.ideabiz.lk/Getting_Started/Token_Manegment) for Authorization

<br>

|API Name    |DebitFromAnyAccount									|
|------------|------------------------------------------------------|
|**Resource URI**|`https:// ideabiz .lk/apicall/accountdebit/v1/ACCOUNT`|
|**HTTP Method** |**POST**|
<br><br>


**Request Parameters**

|Parameter Name|	Description| Type|
|-----|------|-------|
|**ACCOUNT**|The requested account number**[Mandatory]**<br>**GSM/CDMA/LTE or DTV prepaid account number**|String|
|**BILLERID**|USER ID given to pre registered billers **[Mandatory]**|  |
|**AUTHFLAG**|Level of Authentication requested by user <br>o  1 - no authentication, just charge bill and don’t notify<br>o   2 - no authentication, but notify<br>o   3 - get customer consent via onetime SMS token<br>**[Mandatory]**|Integer|
|**AMOUNT**|Debit amount in Rupees  **[Mandatory]**|Integer|
|**RC(Reason Code)**|Reason Code  **[Mandatory]**|Integer|
|**TAXTYPE**|Whether to charge Tax or NOT **[Optional]**<br>o   1 - TL + Cess<br>o   2 - TL + Cess + VAT + NBT<br>o   3 - VAT + NBT<br>o   4 - Insurance TAX<br>o   5 - TAX % to be sent by the service|Integer|
|**TAX**|Tax percentage to apply on amount.   **[Optional]**<br>**Valid only when TAXTYPE is 5**|number|
|**TRID**|Unique transaction ID.  This will be communicated to customer in the authentication SMS.|String|

<br><br>
**Response Information**

|Response|&nbsp; |
|--------|-------|
|**HTTP status**|**201 Debit  successful**<br>401 Un-authorized<br>409 Error while processing<br>500 Internal Error<br>402 Balance insufficient|
|**Response**|{<br>"resCode":<br>"resDesc": " Un-authorized "<br>}<br><br>ResCode is same as the HTTP Status. When HTTP status is 204 there is no Content|