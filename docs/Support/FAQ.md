/*
Title: FAQ
Sort: 2
*/

1.  Unable to login to ideabiz.

  		  Enable cookies in the browser.

2. How to verify REST-API calls?

  		  Use a rest client e.g.: postman to verify API calls
3. How to refresh Access token?

        To request an access token send the refresh token ( ```base64Encode(<consumer key> : <consumer secret> )```) along with the server call. By doing this a new access token along with a refresh token will be send to the client.
        [Visit ideabiz docs!](http://docs.ideabiz.lk/en/Authorization/Authorization_v1)

        >Note: Based 64 encode key can be generated online.[Visit base64encode]( https://www.base64encode.org/)

4. What are Consumer key, Consumer secret and Access token?

        * Consumer key is essentially the API key associated with the application. This 		key is what identifies the client.
        * Consumer secret is the client password that is used to authenticate with the 			authentication server.
        * Access token is what is issued to the client once the client successfully authenticates itself (using the consumer key & secret). This access token defines the privileges of the client (what data the client can and cannot access). Now every time the client wants to access the subscribed application services, the access token secret is sent with the access token.
    <br><br>
    
5. Some of the transactions are not executed.

          If the number of transactions per application created by user is greater than the count of transactions of all the subscribed APIs, then random set of transactions will be dropped.

6. Does ideabiz support unlimited transactions for customer?

        Ideabiz does not support unlimited transactions for customers.

7. What is the use of key word, in a subscribed API?

        It can be used to provide distinct services from a subscribed API.

8. What is the difference between **Production** access token and **base64Encode** access token?

        Production access token is used for subscribed APIs.
        base64Encode token is the refresh token used to request a new access token.

9. What is the use of **Ideabiz - Token Generate**?

          It is used to generate a new access token and Refresh token. The genrator simply 			calls the rest API.
          [Visit ideabiz docs!](https://ideabiz.lk/tools/)

10.    How to allow a specific domain?

      		Enter the domain required in the Allowed Domains field. For example to allow a specific WAN enter WAN ip address [Visit wanip](http://wanip.info/)

11.    What is **Tier** and **tpm**?

    		Tier represents the number of transactions and its unit is transactions per minute (tpm)

12.    How to verify user created application is approved by ideabiz?

    		Check application status. I.e. Active. [Visit ideabiz docs!](http://docs.ideabiz.lk/en/Getting_Started/View_Applications)

13.    How to verify if the subscribed services are approved by ideabiz?

    		The state of subscribed service should change from value **Subscription** to a value according to the agreement.
    		[Visit ideabiz docs!](http://docs.ideabiz.lk/en/Getting_Started/View_Subscriptions)

14.    What is the number of application an application can subscribe?

   			 An application can subscribe minimum 1 and to any number of applications.

15.    How to delete a created application?

    		Go to **My Applications**. In the table below, delete application using the column **Actions**.

16.    How to remove a subscribed application?

   		 To remove subscribed APIs [Visit ideabiz docs!](https://www.ideabiz.lk/store/site/pages/subscriptions.jag)

17.    Once an application is created, how to change the number of Tier or update app information.

    		An application cannot be updated from the Ideabiz UI. However tier (tpm) can be changed by an email request to ideabiz.

18.    What is header enrichment?

	  	  It is a mechanism used to identify user phone number preserving his privacy. The phone number related to header enrichment is resolved in server side.
	  	  [Visit ideabiz docs!](http://docs.ideabiz.lk/en/APIs/Header_Enrichment)

19.    Why do I get exception **Access Token Expired**?

	   		 Once the application has subscribed to APIs user can generate **Access Token** by specifying **Token Validity** time. When the 	specified time is reached and thereafter when an API request send with the same token **Access Token Expired** is thrown.
	    	The exception can be handled by using the refresh token in two ways.
	    		*  Use a scheduler action to request a new access token before the token expires.
	    		*  Once the exception is thrown request for a new access token.
    
20.    How to specify access token validity time through an API call?

    		The token validity time cannot be specified. By default it is set to 1hour.

21.    Why do I get **The subscription to the API is inactive Access failure for API**, status code **900909**. ?

  			  Once the application is subscribed to an API, it has to be approved from Ideabiz. Refer [Question 13]. The exception is due to pending approval.

22.    How to verify status code in the response (ex: 900901,900909,etc)?

    		The Wso2 API manager (version 1.7) specific response code can be referred through wso2 documentation.
    		[Visit WSO2 API Manager 1.7.0. error handling!]( https://docs.wso2.com/display/AM170/Error+Handling)

23.    What is the current version of Wso2 API manager used?

   		 Version 1.7.0 [Visit WSO2 API Manager 1.7.0. !]( 1.7.0 https://docs.wso2.com/display/AM170/About+API+Manager)

24.   What is **MSISDN** number?

    		It is used to identify a mobile phone number internationally.
 
25.    Getting inacvive token error

            recreate new token and send api call

26.     Getting inactive subscription error
            contact ideabiz team for get subscrription activated

27.     Getting application not found error in Payment, SMS or subscription API
            contact ideabiz team for compleate your api configurations

28.     Getting number not whitelisted error
            You need to subscribe specific number in your app. for that use any subscription method

29.     How long token validiti period
            Token keep only valid for 1hour

30.     How app charge rentals
            Application need to handle daily or monthly charging cycles with payment API

31.     What should need to do when user remove subscription from app
            You need to call Subscription API and send Unsubscription API call

32.     How many subscription can have for each app
            Any numbers

33.     How you get reports for API usage
            Request via ideabiz support contact. team will get back to you with neccesary reports

34.     Can i change password
            No. you need to contact support team

35.     Default API response timeout time
            Our proxy maximum response time is 15sec

36.     Setting your SMS, USSD, Subscription Admin API endpoints
            You need to host that endpoint on 80,443, 8080 or 8280 ports. otherwise team need two week to configure firewall rules

37.     if connection failed or proxy timeout
            Please retry if its sms or subscription API call
            for payment, please ask team to enable retry payment. so you can send same charging request and it will avoid harging user for twice

38.     What is callback url in Application Tab
            Currently we are not using it

39.     Sending sms to any operator
            yes, we can allow you to send to any local number or forign numbers
            For localnumbers, you can use any  mask like "MyCompany". but forign, you need to provide sim under your company

40.     Recieing sms 
            You can recieve sms to shortnumber for dialog network (eg : 87711)
            if you need recieve sms from other operator, you need get sim under yournam and transfer it to ideabiz 






