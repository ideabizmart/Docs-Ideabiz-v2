

* Support team will assist you to configure APIs as per your requirement

### SMS API- Required configurations
  
  You should decide and confirm the following

**A.**	Whether SMS service is for Dialog users only or to all networks?
			
 **B.**	Will you be you sending SMS through your application?
					
   If yes, do you want to send SMS via;
   
     - A Alphanumeric Mask** (eg: SMS is sent from your company name)
	 - A 5 digit numeric port (eg: SMS is sent from 87712 port)
	 - A dialog mobile number (eg: SMS goes from 0771234567) 
                            
**C.**	Do you want to receive SMS through your application?
					
 If Yes, do you want to receive incoming SMS from users via;
					
	- A 5 digit numeric port – (eg. Only a Dialog user can send SMS to 87712 port) 
	- A Dialog mobile number – (Any network user can send SMS & Keyword to 0771234567)
							
*Note : You must provide an end point (a URL in your application) to capture incoming SMS response

    **A mask is a personalized Sender Name: 
      eg: You can send SMS from the name of your company or application brand name instead of a port or number. 
                Conditions in selecting a Mask:

                  1.	Mask can be alphanumeric 
                  2.	limited to 9 characters
                  3.	Do not use special characters (~!@#$%^&*()_+`,.:;’”?><)
                  4.	Do not use brands or tradenames that do not belong to you


### Payment API-Required configurations 
	
   Confirm whether your application users;
			
            A. are only required to make one time payments? (User pays and buys some content – no recurring payments)
            
			B. register with your application and is expected to make several purchases time to time.
			
            C. are paying a periodic fee (User has agreed to paying weekly recurring charges)


### Subscription API-Required Configurations

 Confirm the subscription method to whitelist users to your application.
(Without whitelisting a user you cannot send sms, check balance, charge or initiate USSD menu to a user.)


 **A.**	Below subscription methods are available;
         
          I.	SMS Subscription- Where user sends a registration sms- decide on the subscription & unsubscription Keyword
         
          II.    USSD Subscription - Where user Dials a USSD port to register (eg : Eg: User Dials #234# and selects submenu ‘Register’) 
          
          III.	PIN Subscription - Where you have a mobile app (Andriod/IOS), and the user receives a One-Time-Pin (OTP) to verify to register.
          
          IV.	Web registration - Where you have website, and user clicks a Subscribe button to register. (Where user doesn’t enter mobile number, verify users number via encrypted header enrichment, OTP, or Mobile connect API)
          
          V.	Where you have your own method to  obtain user consent, yon can use subscription API and register the user

	**Note : You can have one or more of the above subscription methods


**B.**	Below unsubscription methods are available;
 
  Unsubscription may be user initiated or admin initiated
            
			I. User initiated -  when user requests to be unsubscribed, you need to trigger unsubscription method via sms, ussd or Subscription API (similar to mentioned above)
			
			II. Admin Initiated - When user contacts operator’s customer care agents, and requests for the service to be stopped, Admin may unsubscribe the user from your application. 
            
					* Email notifications- Once a user is unsubscribed by admin your operational contact will get an email with unsubscribe details. 
				
					* Admin API - If you wish to avoid having to manually update admin initiated unsubscriptions (notified via email) in your application, you can provide an Admin API to ideabiz team for configuration. 

	When a user needs basic info about your service or needs to unregister from any service by calling customer care, the  customer care interface, will trigger Admin API that is hosted at your endpoint


