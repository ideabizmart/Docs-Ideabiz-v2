

* Go to **My Subscription**
 * Select Your Application in the drop down list **Application with Subscription**
 * Under the **Keys-Production** generate your keys
 	
        * Consumer Key & Consumer Secret - Using these credentials you can create the Access token which you need to pass for API calls to be authorized. 

        * Access Token - This will be a Unique ID for your Application which will provide acsess to API's

        * Refresh token - Token which can be used after current accses token expires,(in 1h by default) to get a new valid accses token.

* Once you have received the **Access Token** which will expire in 1 hour. Inorder to get the new Access Token Click **Create Refresh Token** or Visit [Ideabiz Tools](https://ideabiz.lk/tools) 


* Create **Authorization Code** with Base64 Encode using Consumer key & Consumer Secret as follows
 	* Visit [Base64 Encode](https://www.base64encode.org)
 	* Add Consumer Key:Consumer Secret and encode 
 		 (Eg - yYHg657Hbshsnadnsh:uahH&GASJ87380208Hgah)
 	* The encoded key will be your Authentication Code for the API calls




[Next Step](http://docs.ideabiz.lk/Getting_Started/Token_Manegment)