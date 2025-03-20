/*
Title: wOw Agent API
Sort: 8
*/

## Web Service APIs for Cash On Delivery order checkout

A sequence of service APIs are exposed to automatically checkout an order for a given user, using cash on delivery payment method.

Consider the following order of execution for the purpose.

## Authorization API calls
All API call request to ideabiz.lk required Authorization headers. Please refer (http://docs.ideabiz.lk/Getting%20Started/Sever%20Authentication) for Authorization


### 1.	Get Session Confirmation Number
```
https://ideabiz.lk/apicall/wow/agent/SessionConfirmationActor/getSessionConfirmationNumber
```

For all the APIs below, add the following parameter when getting called.
_dynSessConf="Number get from above API (Get Session Confirmation Number)"

### 2.	Logout Users
```
https://ideabiz.lk/apicall/wow/agent/userprofiling/ProfileActor/logout
```
**Ex:**
```
https://ideabiz.lk/apicall/wow/agent/userprofiling/ProfileActor/logout?_dynSessConf="session_number"
```
##### Output 
**On Success:**
```
“success”:“Logout successfully” 
```
Code will be returned

### 3.	User Login/ Registration
```
https://ideabiz.lk/apicall/wow/agent/userprofiling/ProfileActor/loginUser
```

**Required Parameters:**

+	login="email/mobile"
+	password
+	email/mobileNumber 

If this is a registration (if you know specifically), 

**Required Parameters:**

+	confirmPassword

*Optional Parameters*
+	realmId
+	dateOfBirth
+	firstName
+	gender
+	lastName
+	member
+	middleName
+	address1
+	address2
+	address3
+	city
+	companyName
+	country
+	postalCode
+	faxNumber

If you are unaware whether this a login or a registration, passing the following required parameters would ensure System either logged you in or register in the system successfully, 

+	login="email/mobile"
+	password
+	email/mobileNumber
+	confirmPassword

Ex:

```
https://ideabiz.lk/apicall/wow/agent/userprofiling/ProfileActor/loginUser?_dynSessConf=<session_number>&login=<email/mobile>&password=<password>&email=<email>&confirmPassword=<password>
```

##### Output
**On Success:**
```
“success”:“Login successful” 
```
code will returned with the logged in user's profile information. 

Ex:
```
{
 
  "success": "Login successful",
  "profile": {"dataSource": {
    "middleName": null,
    "lastName": "last name",
    "firstName": "amanapa"
  }}
}

```

**On Failure:**
formErrors will encounter.

Ex:
```
{
 
  "formError": true,
  "formExceptions": [{
    "localizedMessage": "You are already logged in, amanapa@gmail.com.",
    "errorCode": "invalidLoginAlreadyLoggedIn"
  }]
}

```
### 4.	Add Item To Order
```
https://ideabiz.lk/apicall/wow/agent/commerce/order/purchase/CartModifierActor/addItemToOrder
```
**Required Parameters:**
+	catalogRefIds="sku id"
+	productId="product id"
+	quantity="quantity of the item"

Ex:
```
https://ideabiz.lk/apicall/wow/agent/commerce/order/purchase/CartModifierActor/addItemToOrder?_dynSessConf=<session_number>& catalogRefIds=<sku id>& productId=<product id>& quantity=<quantity of the item>
```

##### Output

**On Success:**
```
“success":"added to the cart successfully"
```
code will returned with the current order summary.

Ex:
```
{
 
  "orderSummary": {
    "id": "o7730006",
    "priceInfo": {
      "amount": 1560,
      "total": 1560,
      "shipping": 0,
      "currencyCode": "LKR",
      "tax": 0,
      "rawSubtotal": 1560,
      "discountAmount": 0
    },
    "commerceItems": [{
      "id": "ci407000007",
      "productDisplayName": "Jaggery cake",
      "priceInfo": {
        "amount": 1560,
        "currencyCode": "LKR",
        "currentPriceDetailsSorted": [{
          "amount": 1560,
          "currencyCode": "LKR",
          "quantity": 2,
          "detailedUnitPrice": 780
        }]
      },
      "quantity": 2,
      "catalogRefId": "sku31440826",
      "productId": "339"
    }],
    "totalCommerceItemCount": 2
  },
  "success": "added to the cart successfully"
}
```
*On Failure:*\
formErrors will encounter.

Ex:
```
{
 
  "formError": true,
  "formExceptions": [{
    "localizedMessage": "The quantity entered is invalid.",
    "errorCode": "invalidQuantity"
  }],
  "concurrentUpdate": false
}
```
### 5.	Checkout Items
```
https://ideabiz.lk/apicall/wow/agent/commerce/order/purchase/CartModifierActor/secureCheckout
```

Parameters:
N/A

Ex:
```
https://ideabiz.lk/apicall/wow/agent/commerce/order/purchase/CartModifierActor/secureCheckout?_dynSessConf=<session_number>
```
##### Output
**On Success:**
```
"success": "Items are proceeded to delivery/pickup successfully"
```
code will be returned with the current order summary.

Ex:
```
{
 
  "orderSummary": {
    "id": "o7730006",
    "priceInfo": {
      "amount": 1560,
      "total": 1560,
      "shipping": 0,
      "currencyCode": "LKR",
      "tax": 0,
      "rawSubtotal": 1560,
      "discountAmount": 0
    },
    "commerceItems": [{
      "id": "ci407000007",
      "productDisplayName": "Jaggery cake",
      "priceInfo": {
        "amount": 1560,
        "currencyCode": "LKR",
        "currentPriceDetailsSorted": [{
          "amount": 1560,
          "currencyCode": "LKR",
          "quantity": 2,
          "detailedUnitPrice": 780
        }]
      },
      "quantity": 2,
      "catalogRefId": "sku31440826",
      "productId": "339"
    }],
    "totalCommerceItemCount": 2
  },
  "success": "Items are proceeded to delivery/pickup successfully"
}
```
**On Failure:**
formErrors will encounter.

Ex:
```
{
 
  "formError": true,
  "formExceptions": [{
    "localizedMessage": "The quantity entered is invalid.",
    "errorCode": "invalidQuantity"
  }],
  "concurrentUpdate": false
}

```
### 6.	Deliver item to the given address
```
https://ideabiz.lk/apicall/wow/agent/commerce/order/purchase/ShippingGroupActor/shipToNewAddress
```
**Required Parameters:**
+	firstName 
+	lastName 
+	address1 
+	city
+	newShipToAddressName=<name for the delivery address>
+	postalCode
+	country


**Optional Parameters**
+	addressPhoneNumber="phone number of the recipient's"

Ex:
```
https://ideabiz.lk/apicall/wow/agent/commerce/order/purchase/ShippingGroupActor/shipToNewAddress?_dynSessConf=<session_number>& firstName=<firstName>&lastName=<lastNmae>&address1=<deliver_address>&city=<city>& newShipToAddressName=<name_for_the_delivery>&postalCode=<postalCode>&country=<country>
```
##### Output

**On Success**
```
"success": "Delivery details successfully added to the order"
```
code will be returned with the current order summary.

Ex:
```
{
 
  "orderSummary": {
    "id": "o7730006",
    "priceInfo": {
      "amount": 3120,
      "total": 3120,
      "shipping": 0,
      "currencyCode": "LKR",
      "tax": 0,
      "rawSubtotal": 3120,
      "discountAmount": 0
    },
    "commerceItems": [{
      "id": "ci407000007",
      "productDisplayName": "Jaggery cake",
      "priceInfo": {
        "amount": 3120,
        "currencyCode": "LKR",
        "currentPriceDetailsSorted": [{
          "amount": 3120,
          "currencyCode": "LKR",
          "quantity": 4,
          "detailedUnitPrice": 780
        }]
      },
      "quantity": 4,
      "catalogRefId": "sku31440826",
      "productId": "339"
    }],
    "totalCommerceItemCount": 4
  },
  "success": "Delivery details successfully added to the order"
}
```
**On Failure:**
formErrors will encounter.
On instance where the items are instore pickup only cannot deliver,
“unsatisfiedState”: “Item is instore pickup only. Cannot deliver the item.” Code will be returned.

Ex:
```
{
 
  "formError": true,
  "formExceptions": [
    {
      "localizedMessage": "The First Name property is required for your address.",
      "errorCode": "missingRequiredAddressProperty"
    },
    {
      "localizedMessage": "The Last Name property is required for your address.",
      "errorCode": "missingRequiredAddressProperty"
    },
    {
      "localizedMessage": "The Street Address property is required for your address.",
      "errorCode": "missingRequiredAddressProperty"
    },
    {
      "localizedMessage": "The City property is required for your address.",
      "errorCode": "missingRequiredAddressProperty"
    }
  ]
}
```
### 7.	Apply payments as COD
```
https://ideabiz.lk/apicall/wow/agent/commerce/order/purchase/PaymentGroupActor/applyPaymentsForCOD
```
**Parameters:**
N/A
Ex:
```
https://ideabiz.lk/apicall/wow/agent/commerce/order/purchase/PaymentGroupActor/applyPaymentsForCOD?_dynSessConf=<session_number>
```
##### Output

**On Success:**
"success": "Payment group applied successfully"
code will be returned
On Failure:
formErrors will encounter.

Ex:
```
{
 
  "formError": true,
  "formExceptions": [
    {
      "localizedMessage": "The country is missing in the shipping address",
      "errorCode": "MissingShippingCountry"
    },
    {
      "localizedMessage": "The city is missing in the shipping address",
      "errorCode": "MissingShippingCity"
    },
    {
      "localizedMessage": "There are no items to purchase in the order.",
      "errorCode": "NoCommerceItemsInOrder"
    },
    {
      "localizedMessage": "The state is missing in the shipping address",
      "errorCode": "MissingShippingState"
    },
    {
      "localizedMessage": "The first name is missing in the shipping address",
      "errorCode": "MissingShippingFirstName"
    },
    {
      "localizedMessage": "The address is missing in the shipping address",
      "errorCode": "MissingShippingAddress"
    },
    {
      "localizedMessage": "The postal code is missing in the shipping address",
      "errorCode": "MissingShippingPostalCode"
    },
    {
      "localizedMessage": "The last name is missing in the shipping address",
      "errorCode": "MissingShippingLastName"
    }
  ],
  "concurrentUpdate": false
}
```

### 8.	Order Confirmation
```
https://ideabiz.lk/apicall/wow/agent/commerce/order/purchase/ConfirmOrderActor/confirmOrder
```
Parameters:
N/A

Ex:
```
https://ideabiz.lk/apicall/wow/agent/commerce/order/purchase/ConfirmOrderActor/confirmOrder?_dynSessConf=<session_number>
```
##### Output

**On Success:**
Email will be sent.
Summary of the order will be returned

Ex:
```
{
  "securityStatus": 4,
  "order": {
    "lastModifiedTime": 1442849522974,
    "shippingGroupCount": 1,
    "paymentGroupCount": 1,
    "shippingGroups": [{
      "specialInstructions": {},
      "handlingInstructions": [],
      "state": 0,
      "locationId": null,
      "shippingMethod": "Ground",
      "id": "sg8070023",
      "trackingNumber": null,
      "priceInfo": {
        "amount": 0,
        "currencyCode": "LKR",
        "amountIsFinal": false,
        "discounted": false,
        "rawShipping": 0
      },
      "description": "sg8070023",
      "actualShipDate": null,
      "submittedDate": null,
      "shipOnDate": null,
      "shippingAddress": {
        "middleName": null,
        "lastName": null,
        "ownerId": null,
        "state": null,
        "address1": null,
"stateDetail": null
    }],
    "commerceItems": [],
    "id": "o7740007",
    "siteId": "siteMall",
    "taxPriceInfo": {
      "amount": 0,
      "currencyCode": "LKR",
      "countyTax": 0,
      "amountIsFinal": false,
      "countryTax": 0,
      "discounted": false,
      "stateTax": 0,
      "cityTax": 0,
      "districtTax": 0
    },
    "priceInfo": {
      "amount": 0,
      "total": 0,
      "shipping": 0,
      "currencyCode": "LKR",
      "tax": 0,
      "amountIsFinal": false,
      "discounted": false,
      "manualAdjustmentTotal": 0,
      "rawSubtotal": 0,
      "discountAmount": 0
    },
    "paymentGroups": [{
      "amount": 0,
      "currencyCode": null,
      "expirationMonth": null,
      "expirationYear": null,
      "paymentId": "pg8000163",
      "creditCardNumber": null,
      "expirationDayOfMonth": null,
      "billingAddress": {
        "middleName": null,
        "lastName": null,
        "ownerId": null,
        "state": null,
        "address1": null,
        "address2": null,
        "address3": null,
        "companyName": null,
        "suffix": null,
        "city": null,
        "country": null,
"prefix": null,
        "firstName": null,
        "jobTitle": null
      },
      "creditCardType": null,
      "orderId": null
    }],
    "profileId": "14290002",
    "creationTime": 1442849374796,
    "relationships": [],
    "totalCommerceItemCount": 0
  },
  "email": "amanapa@gmail.com",
  "orderSummary": {
    "id": "o7740007",
    "priceInfo": {
      "amount": 0,
      "total": 0,
      "shipping": 0,
      "currencyCode": "LKR",
      "tax": 0,
      "rawSubtotal": 0,
      "discountAmount": 0
    },
    "commerceItems": [],
    "totalCommerceItemCount": 0
  }
}
```
**On failure:**
formErrors will encounter.



By executing the APIs in the order given, an order will be placed for the user as a Cash On Delivery payment order.


#### NOTE:
We can use “GET” request to send the request for “Get Session Confirmation Number” API.
For other API’s it will be good to use a “POST” request, rather than a “GET” request.







