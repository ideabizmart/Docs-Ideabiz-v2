/*
Title: NFC
Sort: 8
*/


## API Name: serviceLogin()

Initiated Function: CMS - login ()

#### Requirement

	Prior initiating any transaction, relevant terminals must login to the system by using application credentials.

#### Request

	HTTP Method: POST
    Required URL: https://mife.dialog.lk/apicall/TouchGateway/serviceLogin/1.0
    
    
### Example Request

	{
	"request": {
   				 "applicationCode": "APP_CODE", 
                 "password": "Password1", 
                 "userName": "MIT_user"
				} 
    }
    
 ### Response   
    
|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
| applicationCode|  Application code given by the CMS system | String |
| userName |    User name given by the CMS system   |   String |
| password | Password for the authenticate client. |    String |
 


### Http Status: 200 OK

	{
	serviceLoginResponse: 
    {
			return: 
            {
            applicationCode: "APP_CODE"
			sessionCode: "a34d9086-1820-45b0-a01f-9ee5bc098bd7" 
            statusCode: 0
			statusdesc: "Success"
			} 
    }

|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
| applicationCode|  Application code given by the CMS system. | String |
| sessioncode |    Unique session id for authenticate the client.   |   String |
| statusCode | Status code of the transaction.0 returns if the login is successful. |    String |
| statusdesc | Status of the transaction. “Success” returns if the login is successful. |    String |




## API Name: getProfileByDID()

Initiated Function: CMS - getProfileByDID()

### Requirement

	When DID is given, fetch the relevant profile from CMS.


### Request

	Required URL: https://mife.dialog.lk/apicall/TouchGateway/profileByDID/1.0
	HTTP Method: POST

### Example Request

	{
		"request": {
		"coporateId": "2",
		"sessionCode": "0365d829-1150-4ae1-854d-2dca323ebd28", 
        "dId": "XXXXXXXXXX"
		} 
    }
    
    
 |  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
| dId|  Did of the card | String |
| coporateId |    Unique id of the corporate given by the CMS  |  int |
| sessionCode |    Session id given by the CMS login response.   |   String |


### Response

Http Status: 200 OK

	{
      "getProfileByDIDResponse":{
      "return":{ 
      "card":{
          "cardType":"TEST", 
          "expireDate":"01-12-2030", 
          "msisdn":77XXXXXXX, 			
          "ownershipType":"COP", 
          "uid":"XXXXXXXXXXXXXX", 
          "dId":XXXXXXXXXX
      }, 
      "primaryProfile":{
          "address1":"475,Union Place", 				
          "address2":"Colombo 2", 
          "address3":"Sri Lanka", 
          "contactNo":77XXXXXXX, 
          "dob":"02-01-2013", 
          "email":test@test.com, 
          "firstName":"Name1", 
          "idNumber":"XXXXXXXXXV", 
          "idType":"NIC", 
          "lastName":Name2, 
          "mothersMaidenName":test1, 
          "postalCode":10114, "title":MR
      }, 
      "secondaryProfile":{
          "address1":"test1",
          "address2":"test2",
          "address3":"test3", 
          "contactNo":77XXXXXXX, 
          "dob":"13-08-2014", 
          "email":"test@gmail.com", 
          "firstName":"FirstName", 
          "idNumber":"XXXXXXXXXV", 
          "idType":"NIC",
          "lastName":"LastName", 
          "mothersMaidenName":"MaidenName", 
          "postalCode":10114,
          "title":Ms },
          "statusCode":0,
          "statusDesc":"Success" }
      } 
    }



|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
| primaryProfile|  If the query request returns 0 then it will send the primary profile (CMSProfile) information of the card holder. | |
| secondaryProfile |    If the query request returns 0 then it will send the secondary profile (CMSProfile) information of the card holder. |   |
| card |    If the query request returns 0 then it will send the card information.   |    |
| statusCode |   Returns 0 if it is successful.   |   String |
| statusdesc |   If returns error code then give a description of above error code.  |   String |




## APIName:addProfile()

Initiated Function: CMS - isLogin() , if(true), addProfile()


## Requirement

	This is used to add new profile to CMS and attach it to specific card.

## Request

Required URL: https://mife.dialog.lk/apicall/TouchGateway/addProfile/1.0
    
HTTP Method: POST

	{
		"profileAddRequest": {
		"applicationCode": "APP_CODE", 
        "corporateId": "41",
		"did": "XXXXXXXXXX", 
        	"profileBean": {
				"address1": "add1", 
                "address2": "add2", 
                "address3": "add3", 
                "contactNo": "77XXXXXXX", 
                "dob": "17-09-1983", "email": "test@gmail.com", 
                "firstName": "Name1", 
                "idNumber": "XXXXXXXXXV", 
                "idType": "NIC",
				"lastName": "Name2", 
                "mothersMaidenName": "test", 
                "postalCode": "XXXXX",
				"title": "MR"
			},
		"sessionCode": "3387bc62-4365-4a23-95dd-052d05babfd5", 
  	  	"uid": "XXXXXXXXXXXXXX"
		}
  	 }
    
    
|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
| applicationCode|  Application code given by the CMS system. | String |
| corporateId |    Unique id of the corporate given by the CMS   |   int |
| did | Did of the card |    String |
| profileBean | CMSProfile |    CMSProfile |  
| sessionCode | Session id given by the CMS login response. |    String |
| uid | UID of the card. |    String | 
    
    
### Response 
Http Status: 200 OK

	{
		"addProfileResponse":{
			"return":{ "statusCode":0, "statusDesc":"Success"
			}
        }
	}
    

|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
| statusCode|  Returns 0 if it is successful | String |
| statusdesc |    Status of the transaction. “Success” returns if the request is successful.   |   String |



## API Name: getProfileByIdentification ()

Initiated Function: CMS - isLogin() , if(true), getProfileByIdentification()

### Requirement
	
    This is used to get relevant profile details by providing NIC or PP.

### Request

Required URL: https://mife.dialog.lk/apicall/TouchGateway/profileByIdentification/1.0

HTTP Method: POST

	{
		"request": {
		"applicationCode": "APP_CODE", 
        "password": "Password1", 
        "userName": "MIT_user"
		} 
    }
    
|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
| idType|  Identification type of the profile | String |
| idNumber |    Identification number of the profile.   |   String | 
| sessionCode |    Session id given by the CMS login response.   |   String | 

        
        
### Response

Http Status: 200 OK

	
	{
		"getProfileByIdentificationResponse":{
			"return":{ 	
            	"card":{
					"cardType":1, 
                    "expireDate":08-08-2020, 
                    "msisdn":XXXXXXXXX, 
                    "ownershipType":COP, 
                    "uid":XXXXXXXXXXXXXX, 
                    "dId":XXXXXXXXXX
				}, 
                "primaryProfile":{
					"address1":"add1", 	
                    "address2":"add2", 
                    "address3":"add3", 
                    "contactNo":"77XXXXXXX", 	
                    "dob":"01-01-2000", 
                    "email":"test@gmail.com", 
                    "firstName":"Name1", 
                    "idNumber":"XXXXXXXV", 
                    "idType":"NIC", 
                    "lastName":"Name2", 
                    "mothersMaidenName":"test", 
                    "postalCode":XXXXX, 
                    "title":MR
				}, 
                "secondaryProfile":null,	
                "statusCode":0, 
                "statusDesc":"Success"
				}
        }
	}
    
    
    
    
 |  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
| primaryprofile| If the query request returns 0 then it will send the profile information of the card holder. | CMSProfile |
| secondaryprofile |    null   |   CMSProfile | 
| card |    null   |   CMSCard | 
| statusCode |   Returns 0 if it is successful.  |   String | 
| statusdesc |   If returns error code then give a description of above error code.   |   String | 
    
    


## API Name: getWalletBalance ()

Initiated Function: CMS - isLogin() ,if(true) BM - getBalanceTransaction()
    
### Requirement

	Get the available balance of all compartments of a wallet by providing a DID.

### Request

Required URL: https://mife.dialog.lk/apicall/TouchGateway/walletBalance/1.0

HTTP Method: POST

	{
		"balanceRequest": {
		"sessionCode": "0365d829-1150-4ae1-854d-2dca323ebd28",
		"dId": "XXXXXXXXXX" }
	}
   

    
|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
| sessionCode| Unique session id for authenticate the client. | String |
| dId |    DID of the card   |   String |     
    
    
### Response 

	{ 
    	getWalletBalanceResponse:
		{
			return:
			{
				compartmentsBalance:
				{
               		compartmentCode: 1 
                	compartmentValue: 82
				}
				errorDesc: null 
                status: "TX_SUCCESS"

			}
         }
    }
    
  
 |  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
| Compartments| List of Compartments | Compartments[] |
| errorDesc |    Description of the Error  |  String |   
| status |    Returns 0 if it is successful   |   String |      
    
    
#### Compartment

|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
|compartmentCode| Compartment Code | Compartments[] |
| compartmentValue | Balance of the compartment  |  String |   
    
    

## API Name: Transaction Void

### Requirement

	Rollback already performed top up transaction
    
### Request

Required URL: https://mife.dialog.lk/apicall/TouchGateway/transactionVoid/1.0

HTTP Method: POST

	{
		"transactionRequest": {
		"applicationCode": "APP_CODE",
		"compartmentId": "1",
		"reason": "Test",
		"reqUserId": "XXXXXXXXXX",
		"sessionCode": "0365d829-1150-4ae1-854d-2dca323ebd28", 
        "transactionRefNo": "XXXXXXXXXXXXXXX"
		} 	
    }

|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
|applicationCode| Application code given by the CMS sys | String |
| compartmentId | Compartment to which the top up has been put into  |  Number |   
|expiryDate| Transaction expiration date | Date |
| noOfUnits | Transaction Amount  |  Number |   
|posCode| Terminal ID | String |
| sessionCode | Session id given by the CMS login response.  |  String |   
|topUpTxRefNo|Reference number of the original topup | String |
| dId | DID of the card  |  String |   



### Response

Http Status 200 Ok

	{
		transactionVoidResponse: {
			return: {
				batchNo: null
				errorCode: null
				errorDesc: "null"
				status: "TX_SUCCESS" 
                txReference: XXXXXXXXXXXXXX
            }
        }
    }


|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
|batchNo| Redeemed batch number |  |
|errorCode| Standard Error code |  |
|errorDesc| Error Desc |  |
|status| Request status (success/failure) |  |
|topupId| Topup ID of ez cash |  |
|txReference| Transaction reference of Ez cash |  |


## API Name: redeemWallet ()

### Requirement

	Initiate transaction redemption
    
### Request

Required URL: https://mife.dialog.lk/apicall/TouchGateway/redeemWallet/1.0

HTTP Method: POST
    
 	{
		"redeemRequest": {
		"amount": "50",
		"applicationCode": "APP_CODE",
		"compartmentId": "1",
		"posId": "XXXXXXXXXXXXXXX",
		"posTransactionTime": "08/04/2015 10:43:12", "postBalance": "100",
		"preBalance": "50",
		"sessionCode": "0365d829-1150-4ae1-854d-2dca323ebd28", 
        "transactionRefNo": "XXXXXXXXXXXXXXX",
        "dId": "XXXXXXXXXX" 
        }
	}
    
 
|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
|amount| Redeem Amount | double |
|applicationCode| Application code given by the CMS system. | String  |
|compartmentId| Redeem Compartment given by Dialog | long |
|posId| Terminal ID | String |
|posTransactionTime| Transaction Time of the POS | String |
|postBalance| Post Balance of the card | double |
|preBalance| Previous Balance of the card | double |
|sessionCode| Unique session id for authenticate the client. | String |
|transactionRefNo| Unique transaction reference | String |
|dId|DID Of the card |  String|


### Response 

Http Status: 200 OK

	{
		"redeemWalletResponse":{
 		  "return":{
    		 "batchNo":XXX,
      		 "status":"TX_SUCCESS",
     		 "txReference":"XXXXXX"
           }  
		}
	}

|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
|batchNo| Unique Batch number for Balance Module | String |
|bmReferance| Reference id of the balance module | String  |
|statusCode| Returns ‘TX_SUCCESS’ if it is successful | String |
|statusDesc|  | String |
|transactionReferance| Unique transaction reference send by request | String |




## API Name: walletTopup ()

### Requirement

	Initiate wallet top up

### Request

Required URL: https://mife.dialog.lk/apicall/TouchGateway/walletTopup/1.0

HTTP Method: POST

	{
		"topUpRequest": {
			"applicationCode": "APP_CODE", "compartmentId": "1",
			"expiryDate": "12/12/2015 12:12:12", "noOfUnits": "50",
			"posCode": "XXXXXXXXXXXXXXX",
			"sessionCode": "0365d829-1150-4ae1-854d-2dca323ebd28", 
            "topUpTxRefNo": "XXXXXXXXXXXXXXX ",
			"dId": "XXXXXXXXXX"
		} 
    }
    
 
|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
|applicationCode| Application code given by the CMS system. | String |
|compartmentId| Wallet compartment | String  |
|expiryDate| Top up expiration date | String |
|noOfUnits| Amount | double |
|posCode| POS id from which top up is initiated | String |
|sessionCode| Transaction Session | String |
|topUpTxRefNo| Transaction reference | String |
|dId| Card ID | String |


### Response

Http Status 200 OK

	Response {
		walletTopupResponse: {
			return: {
				statusCode: 0
				statusdesc: "TX_SUCCESS" 
                topUpId: XXXXX 
                topUpTxRefNo: XXXXXXXXXX
             }   
         }    
	}


|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
|statusCode| Transaction Status | String |
|statusDesc| Success/failure | String  |




## API Name: pendingTopups ()

### Requirement

Get the pending topups values for given card

### Request
Required URL: https://mife.dialog.lk/apicall/TouchGateway/pendingTopups/1.0

HTTP Method: POST

	{
		"pendingTopUp": {
		"applicationCode": "APP_CODE", 
        "compartmentId": "1",
		"did": "XXXXXXXXXX", 
        "lastTopUpId": "0",
		"txRefNo": "XXXXXXXXXX",
		"updatedPosCode": "XXXXXXXX",
		"sessionCode": "607af6be-8258-48af-9c21-f8dc10e56b5b"
		}
    }
    
|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
|applicationCode|Application code given by the CMS system | String |
|compartmentId| Wallet compartment | String  |
|did| Did of the top up card | String  |
|lastTopUpId| Topup id of the last topup did for given card | int  |
|txRefNo| Transaction reference number of the transaction | String  |
|updatedPosCode| POS id from which top up is initiated | String  |
|sessionCode| Transaction Session | String  |


### Response

Http Status 200 OK


	{
		"getPendingTopUpResponse": {
			"return": {
				"did": XXXXXXXXXX, "noOfUnits": 125,
				"status": "TX_SUCCESS", 
                "topUpDate": "YYYY-MM-DD", 
                "topUpId": XXXXX, 					
                "topUpTxRefNo": XXXXXXXXXX
			} 
         }
	}

|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
|errorCode|Error Status code | String |
|errorDesc| Description of the error | String  |
|did| Did of the card | String  |
|noOfUnits| Total value of the pending topups | double  |
|status| Success/failure | String  |
|topUpDate| Last topup Date | String  |
|topUpId| Last Topup update for last topup update | int  |
|topUpTxRefNo| Reference of the transactions | String  |




## API Name: updateWalletTopUp ()

### Requirement

	Update topup value after writes to the card.

### Request

Required URL: https://mife.dialog.lk/apicall/TouchGateway/updateWalletTopUp/1.0

HTTP Method: POST	

	{
		"updateBalance": {
			"applicationCode": "APP_CODE", 
            "cmsMessage": "test", 
            "compartmentCode": "1",
			"did": "XXXXXXXXXX",
			"posId": "XXXXXXXX",
			"posTransactionTime": "12/12/2015 12:12:12", 
            "postBalance": "100",
			"preBalance": "50",
			"subApplicationCode": "2",
			"topUpId": "XXXXX",
			"transactionCode": "WALLETTOP", 
            "transactionReferenceNo": "XXXXXXXXXX",
			"sessionCode": "0365d829-1150-4ae1-854d-2dca323ebd28"
		} 
    }


|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
|applicationCode|Application code given by the CMS system | String |
|cmsMessage| Message of the card validation system given * This is optional | String  |
|compartmentCode| Compartment Id | String  |
|did| Did of the card | int  |
|posId| POS id from which top up is initiated | String  |
|posTransactionTime| Transaction time of the POS machine MM/DD/YYYY HH:MM:SS | String  |
|postBalance| Last Topup update for last topup update | int  |
|preBalance| Post Balance of the card | double  |
|preBalance| Pre balance of the card | double  |
|subApplicationCode| Sub Application cord given by Dialog * Optionals | String  |
|topUpId| Last top up id give from pending Topup function | Integer  |
|transactionCode| Transaction code “WALLETTOP” | String  |
|transactionReferenceNo| Reference number of the transaction | String  |
|sessionCode| Transaction Session | String  |


### Response
Http Status 200 OK

	{
		"updateWalletTopUpResponse": {
			"return": {
				"did": XXXXXXXXXX,
				"status": "TX_SUCCESS", 
                "topUpDate": "06/13/2015 12:12:12", 
                "topUpId": XXXXX,
				"topUpTxRefNo": XXXXXXXXXX
			} 
         }
	}



|  Parameter Name  |    Description      |  Data Type|
|----------|:-------------:|------:|
|errorCode|Error Status code | String |
|errorDesc| Description of the error | String  |
|did| Did of the card | String  |
|topUpDate| Date of the topup updated | String  |
|status| Success/failure | String  |
|topUpDate| Last topup Date | String  |
|topUpId| Last Topup update for last topup update | int  |
|topUpTxRefNo| Reference of the transactions | String  |












