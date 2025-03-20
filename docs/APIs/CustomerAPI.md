# API Integration

##### Request Header
- Content-Type: application/json
- Authorization: Bearer [access token]
- Accept: application/json

### APIS
1. BillInfoAPI - Get the bill information of the customer by entering the telephone number
2. LOBAPI - Check the Lob by entering the telephone number
3. CcbsAPI - Get ccbs Information by entering the telephone number
4. BalanceCheckAPI - Check the balance by entering the telephone number
5. DataUsageAPI - Get the data usage of a subscriber by entering the telephone numeber and the service name
6. DataPackAPI - Add extend FUP quota to a fixed LTE connection
7. LTECustomerProfileAPI - Get the LTE customer profile information

## BillInfoAPI
- Get the bill information of the customer by entering the telephone number
#### URL
```
https://ideabiz.lk/apicall/cx/V1.0/bill/billInfo/{refaccount}
```
#### Sample URL for V3
```
https://ideabiz.lk/apicall/cx/V1.0/bill/billInfo/94771234567
```

#### Method
```
GET
```
#### Response
```
{
“getAllBillingInfoResponse”: 
{
“last_bill”: “3616.29”,
“last_bill_date”: “2014-12-05 00:00:00”,
“min_payment”: “250.0”,
“outstanding”: “4915.9043”,
“outstanding_date”: “2014-12-30 16:32:33”,
“result”: “0”,
“running_bill”: “2190.2373”,
“running_bill_date”: “2014-12-30 16:32:33”
}
}
```
## LOBAPI
- Check the Lob by entering the telephone number
#### URL
```
https://ideabiz.lk/apicall/cx/V1.0/lob/check/{refaccount}
```
#### Sample URL for V3
```
https://ideabiz.lk/apicall/cx/V1.0/lob/check/94771234567
```
#### Method
```
GET
```
#### Response
```
{
"msisdn":"94771234567",
"lob":"GSM"
}
```
## CcbsAPI
- Get ccbs Information by entering the telephone number
#### URL
```
https://ideabiz.lk/apicall/cx/V1.0/ccbs/basicInfo/{refaccount}
```
#### Sample URL for V3
```
https://ideabiz.lk/apicall/cx/V1.0/ccbs/basicInfo/771234567
```
#### Method
```
GET
```
#### Response
```
{
"statusReasonObj":
{
"IDENTIFICATION_TYPE":"NIC",
"IDENTIFICATION_NUMBER":"xxxxxxxxxxxx",
"OLD_NIC_NO":"xxxxxxxxxV",
"DOB_REGISTRATION_DATE":"xxxx-xx-xx xx:xx:xx.x",
"NAME1":"xxxxxxxx",
"NAME2":"xxxxx",
"NATIONALITY":"LKA",
"ORIGINAL_PACKAGE_START_DATE":"xxxx-xx-xx xx:xx:xx.x",
"SIM":"94xxxxxxxxxxxxx",
"PRE_POST":"PREPAID"
}
}
```
## BalanceCheckAPI
- Check the balance by entering the telephone number
#### URL
```
https://ideabiz.lk/apicall/cx/V1.0/balance/balanceCheck/{msisdn}
```
#### Sample URL for V3
```
https://ideabiz.lk/apicall/cx/V1.0/balance/balanceCheck/94771234567
```
#### Method
```
GET
```
#### Response
```
{
“endUserId”: “94771234567”,
“referenceCode”: “DLG212005-1500564823863”,
“accountInfo”: {
“accountType”: “PREPAID”,
“accountStatus”: “Active”,
“creditLimit”: 66.81,
“balance”: 66.81
}
}
```
## DataUsageAPI
- Get the data usage of a subscriber by entering the telephone numeber and the service name
##### URL
```
https://ideabiz.lk/apicall/cx/V1.0/dpi/usage/{refAccount}/{serviceName}
```
#### Sample URL for V3
```
https://ideabiz.lk/apicall/cx/V1.0/dpi/usage/771234567/dialog
```
#### Method
```
GET
```
#### Response
```
{
    "Response": {
        "DPIUsageInfo": {
            "executionStatus": "00",
            "dpiResult": "srv=I_GPRS_APL_500,qtaName=I_GPRS_APL_500,qtabal=797425,qtausage=1299727,qtaclass=0,expiredate=20171105000000,qtainitval=2097152"
        }
    }
}
```
## DataPackAPI
- Add extend FUP quota to a fixed LTE connection
#### URL
```
https://ideabiz.lk/apicall/cx/V1.0/add/data
```
#### Sample URL for V3
```
https://ideabiz.lk/apicall/cx/V1.0/add/data
```
#### Method
```
PUT
```
#### Body
```
{
“lteNo”:“0214122143”,
“fupExtCode”:“1MORE”,
“count”:“2”,
“requestSource”:“APP”
}
```
#### Response
```
{
    "resultCode": "01",
    "result": "customer in other connection status"
}
```
## LTECustomerProfileAPI
- get the LTE customer profile information.

#### URL
```
https://ideabiz.lk/apicall/cx/V1.0/customer/profile/{LTENo}
```
#### Sample URL for V3
```
https://ideabiz.lk/apicall/cx/V1.0/customer/profile/{LTENo}
```
#### Method
```
GET
```
#### Response
```
{  
   "title":"MR",
   "co":"ABC",
   "name":"TEST-QA-NAME",
   "address1":"TEST-QA-ADDRESS",
   "address2":"TEST-QA-ADDRESS2",
   "postalCode":"11000",
   "idType":"TIN",
   "idNo":"1287",
   "contactNo":"0332222240",
   "email":"abc@gmail.com",
   "startDate":"2007-11-05 00:00:00.0",
   "status":"ACTIVE"
}
```


