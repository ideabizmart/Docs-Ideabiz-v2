<h1 class="title">1. SUBSCRIPTION API </h1>
<p class="introduction">To handle application registration and unregistration process.</p>
<h2 class="apiTitle">1.1. Registration Service </h2>

<ul>
<li><b>DESCRIPTION : </b> To register new application</li>
<li><b>URL:</b> https://ideabiz.lk/apicall/dapsubscription/v1.0/registration</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
   <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationName</td>
       <td>Application unique name</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>callbackUrl</td>
       <td>Callback url for alert generation</td>
       <td>string</td>
       <td>required</td>
   </tr>
</tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S101</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>App registration success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>appId</li>
                       <li>appName</li>
                       <li>status
                           <ul>
                               <li>REGISTERED/UNREGISTERED</li>
                           </ul>
                       </li>
                       <li>callbackUrl</li>
                       <li>createdDate</li>
                   </ul>
               </td>
          
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E101</td>
               <td> &ltparameter&gt not found or empty in the request</td>
           </tr>
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationName":"app",
           "callbackUrl":"sgtv"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S101",
           "statusDetail":"Application successfully registered",
           "responseData": {
                  "appId": "kv9ygf9p",
                  "appName": "app",
                  "status": "REGISTERED",
                  "callbackUrl": "128.217.137.199/callback",
                  "createdDate": "2017-06-30",
           }
        }
```
   </pre>
</li>
</ul>

<hr>


<h2 class="apiTitle">1.2. Application Update Service </h2>

<ul>
<li><b>DESCRIPTION : </b> To update  an application</li>
<li><b>URL:</b> https://ideabiz.lk/apicall/dapsubscription/v1.0/update</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
   <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>applicationName</td>
       <td>Application unique name</td>
       <td>string</td>
       <td>optional</td>
   </tr>
   <tr>
       <td>callbackUrl</td>
       <td>Callback url for alert generation</td>
       <td>string</td>
       <td>optional</td>
   </tr>
</tbody>
</table>

</li>

<li><b>SUCCESS RESPONSE:</b>
     <table>
         <tbody>
            <tr>
                <th>Status Code</th>
                <td>S102</td>
            <tr>
            <tr>
                <th>StatusDetail</th>
                <td>Application update success</td>
            <tr>
            <tr>
                <th>responseData</th>
                <td>
                    <ul>
                        <li>appId</li>
                        <li>appName</li>
                        <li>status
                            <ul>
                                <li>REGISTERED/UNREGISTERED</li>
                            </ul>
                        </li>
                        <li>callbackUrl</li>
                        <li>createdDate</li>
                    </ul>
                </td>        
         </tbody>
     </table>
 </li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E102</td>
               <td>applicationId  not found  or empty in the request</td>
           </tr>
           <tr>
               <td>E103</td>
               <td>Application for application id : &ltapplicationId&gt not found</td>
           </tr>
           <tr>
               <td>E104</td>
               <td>Application for application id : &ltapplicationId&gt already unregistered</td>
           </tr>
           <tr>
               <td>E108</td>
               <td>applicationName and callbackUrl both not found in the request</td>
           </tr>                       
          
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER</b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"kv9ygf9p",
           "applicationName":"app",
           "callbackUrl":"128.217.137.199/callback"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S102",
           "statusDetail":"Application update success",
           "responseData": {
                  "appId": "kv9ygf9p",
                  "appName": "app",
                  "status": "REGISTERED",
                  "callbackUrl": "128.217.137.199/callback",
                  "createdDate": "2017-06-30",
           }
        }
```
   </pre>
</li>
</ul>

<hr>

<h2 class="apiTitle">1.3. Un-registration Service</h2>

<ul>
<li><b>DESCRIPTION : </b> To unregister registered application. All routes and vehicles will be disabled after unregistration</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapsubscription/v1.0/unregistration</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
    <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
</tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S110</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Application successfully unregistered</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>appId</li>
                       <li>appName</li>
                       <li>status
                           <ul>
                               <li>REGISTERED/UNREGISTERED</li>
                           </ul>
                       </li>
                       <li>callbackUrl</li>
                       <li>createdDate</li>
                   </ul>
               </td>   
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E103</td>
               <td> Application for application id : &ltapplicationId&gt not found</td>
           </tr>
           <tr>
               <td>E104</td>
               <td>Application for application id : &ltapplicationId&gt already unregistered</td>
           </tr>
           <tr>
               <td>E105</td>
               <td> Application for application id : &ltapplicationId&gt have started schedules</td>
           </tr>
           <tr>
               <td>E110</td>
               <td>Application Id not found or empty  in the request</td>
           </tr>
          
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"kv9ygf9p"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S110",
           "statusDetail":"Application successfully unregistered",
           "responseData": {
                  "appId": "kv9ygf9p",
                  "appName": "app",
                  "status": "UNREGISTERED",
                  "callbackUrl": "128.217.137.199/callback",
                  "createdDate": "2017-06-30",
           }
        }
```
   </pre>
</li>
</ul>

<hr>

<h1 class="title">2. VEHICLE API </h1>
<p class="introduction">To handle vehicle registration and vehicle location.</p>
<h2 class="apiTitle">2.1. Vehicle Create Service </h2>

<ul>
<li><b>DESCRIPTION : </b>To add a new vehicle to application</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapvehicle/v1.0/create</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
    <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>vehicleName</td>
       <td>Unique name for the vehicle within application</td>
       <td>string</td>
       <td>required</td>
   </tr>
 
</tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S201</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Vehicle create success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>appId</li>
                       <li>vehicleId
                           <ul>
                                <li>System generted unique id for the vehicle</li>
                           </ul>
                       </li>
                       <li>vehicleName</li>
                       <li>createdDate</li>
                   </ul>
               </td>      
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E201</td>
               <td>Application id or vehicle name not found or empty  in the request</td>
           </tr>
           <tr>
               <td>E202</td>
               <td>Vehicle name : &ltvehicleName&gt should be less than 10 characters</td>
           </tr>                       
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "vehicleName":"v1"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S201",
           "statusDetail":"Vehicle create success",
           "responseData": {
                  "appId": "zfpqb6e9",
                  "vehicleId": "hq37tjsq",
                  "vehicleName": "v1",
                  "createdDate": "2017-06-30",
           }
        }
```
   </pre>
</li>
</ul>

<hr>


<h2 class="apiTitle">2.2. Vehicle Location Service </h2>

<ul>
<li><b>DESCRIPTION : </b> To update vehicle latest location</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapvehicle/v1.0/update_location</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
    <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>vehicleId</td>
       <td>Vehicle unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>lat</td>
       <td>Latitude</td>
       <td>float</td>
       <td>required</td>
   </tr>
   <tr>
       <td>lng</td>
       <td>Longitude</td>
       <td>float</td>
       <td>required</td>
   </tr>
   <tr>
       <td>datetime</td>
       <td>Date time string (Y-m-d H:i:s)</td>
       <td>String</td>
       <td>required</td>
   </tr>
</tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S210</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>New location added successfully</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>0 => routeId</li>
                       <li>1 => pointId</li>
                       <li>2 => distance from beginning</li>
                       <li>3 => time from beginning</li>
                       <li>4 => avg speed</li>
                       <li>5 => lat</li>
                       <li>6 => lng</li>
                       <li>7=>invalid lat</li>
                       <li>8=>invalid lng</li>
                       <li>9=>time from beginning to invalid point</li>
                       <li>10=>total average speed</li>                
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E210</td>
               <td>&ltparameter&gt not found or empty in the request</td>
           </tr>
           <tr>
               <td>E211</td>
               <td>Error in date time format : &lt timeString &gt</td>
           </tr>
           <tr>
               <td>E212</td>
               <td>No running schedule is configured for the vehicle : &lt vehicleId &gt</td>
           </tr> 
           <tr>
               <td>E260</td>
               <td>No vehicle found for application id : &ltapplicationId&gt vehicle id : &ltvehicleId&gt</td>
           </tr>
           <tr>
               <td>E312</td>
               <td>Error in format lat : &ltlat&gt lng : &ltlng&gt</td>
           </tr>
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "vehicleId":"hq37tjsq",
           "lat":"7.783495",
           "lng":"80.87294",
           "datetime":"2017-01-01 12:12:12"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"E212",
           "statusDetail":"No running schedule is configured for the vehicle : hq37tjsq",
           "responseData": []
        }
```
   </pre>
</li>
</ul>

<hr>

<h2 class="apiTitle">2.3. Vehicle Rename Service </h2>

<ul>
<li><b>DESCRIPTION : </b> To change vehicle name</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapvehicle/v1.0/rename</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
   <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>vehicleId</td>
       <td>Vehicle unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>VehicleName</td>
       <td>New vehicle name to update</td>
       <td>string</td>
       <td>required</td>
   </tr>
</tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S220    </td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Vehicle rename success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>appId</li>
                       <li>vehicleId</li>
                       <li>vehicleName</li>
                       <li>createdDate</li>                
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E220</td>
               <td>&ltparameter&gt not found or empty in the request</td>
           </tr>
           <tr>
               <td>E260</td>
               <td>No vehicle found for application id : &ltapplicationId&gt vehicle id : &ltvehicleId&gt</td>
           </tr>
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "vehicleId":"wix4k35g",
           "vehicleName":"v10"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S220",
           "statusDetail":"Vehicle rename success",
           "responseData": {
                      "appId": "zfpqb6e9",
                   "vehicleId": "wix4k35g",
                   "vehicleName": "v10",
                   "createdDate": "2017-07-02"
           }
       }
```
   </pre>
</li>
</ul>

<hr>


<h2 class="apiTitle">2.4. Vehicle View Service </h2>

<ul>
<li><b>DESCRIPTION : </b> To view vehicle status</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapvehicle/v1.0/get</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
   <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>vehicleId</td>
       <td>Vehicle unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
</tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S230</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Vehicle get success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>appId</li>
                       <li>vehicleId</li>
                       <li>vehicleName</li>
                       <li>createdDate</li>                
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E230</td>
               <td>&ltparameter&gt not found or empty in the request</td>
           </tr>
           <tr>
               <td>E260</td>
               <td>No vehicle found for application id : &ltapplicationId&gt vehicle id : &ltvehicleId&gt</td>
           </tr>
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "vehicleId":"wix4k35g"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S230",
           "statusDetail":"Vehicle get success",
           "responseData": {
                      "appId": "zfpqb6e9",
                   "vehicleId": "wix4k35g",
                   "vehicleName": "v10",
                   "createdDate": "2017-07-02"
           }
       }
```
   </pre>
</li>
</ul>

<hr>


<h2 class="apiTitle">2.5. Vehicle Remove Service</h2>

<ul>
<li><b>DESCRIPTION : </b>To remove a vehicle</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapvehicle/v1.0/remove</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
  <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>     
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>vehicleId</td>
       <td>Vehicle unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
</tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S240</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Vehicle remove success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>appId</li>
                       <li>vehicleId</li>
                       <li>vehicleName</li>
                       <li>createdDate</li>                
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E240</td>
               <td>&ltparameter&gt not found or empty in the request</td>
           </tr>
           <tr>
               <td>E250</td>
               <td>There are pending or running schedules for the vehicleId : &ltvehicleId&gt</td>
           </tr>
           <tr>
               <td>E260</td>
               <td>No vehicle found for application id : &ltapplicationId&gt vehicle id : &ltvehicleId&gt</td>
           </tr>
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "vehicleId":"wix4k35g"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S240",
           "statusDetail":"Vehicle remove success",
           "responseData": {
                      "appId": "zfpqb6e9",
                   "vehicleId": "wix4k35g",
                   "vehicleName": "v10",
                   "createdDate": "2017-07-02"
           }
       }
```
   </pre>
</li>
</ul>

<hr>

<h1 class="title">3.ROUTE  API
</h1>
<p class="introduction">To handle vehicle registration and vehicle location.</p>
<h2 class="apiTitle">3.1. Route Create Service</h2>

<ul>
<li><b>DESCRIPTION : </b>To create and maintain routes (including adding pickup points )</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapvehicle/v1.0/create</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
 <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>routeName</td>
       <td>Unique name for the route</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>jsonPath</td>
       <td>Path point array in json format</td>
       <td>String '[[lat,lng],[lat,lng],..]’</td>
       <td>required</td>
       </tr>
    </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S301</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Route create success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>rote
                           <ul>
                               <li>appId</li>
                               <li>routeId</li>
                               <li>routeName</li>
                               <li>createdPath</li>
                           </ul>
                       </li>
                       <li>resampledPath
                           <ul>
                               <li>[[lat,lng],[lat,lng],...]</li>
                           </ul>
                       </li>               
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E301</td>
               <td>&ltparameter&gt not found or empty in the request</td>
           </tr>
           <tr>
               <td>E302</td>
               <td>Error in jsonPath format : &ltjsonPath string&gt</td>
           </tr>
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "routeName":"r1",
           "jsonPath":"[[7.100812, 79.914290],[7.100280, 79.914185],[7.099752, 79.914105],[7.098979, 79.914212]]"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S301",
           "statusDetail":"Route created success",
           "responseData": {
                      "route": {
                          "appId":"zfpqb6e9",
                          "routeId":"qqdkmyfd",
                          "routeName":"r1",
                          "createdDate":"2017-06-30"
                      },
                      "resampledPath":[[7.100812,79.91429],[7.1007679216833,79.914281300323],[7.0990811646065,79.91419940325],[7.0990365813163,79.914204900307],...]    
           }
       }
```
   </pre>
</li>
</ul>

<hr>

<h2 class="apiTitle">3.2. Route Get Service</h2>

<ul>
<li><b>DESCRIPTION : </b>to get route details for a given application id and route</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/daproute/v1.0/get</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
  <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>routeId</td>
       <td>Route unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
    </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S330</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Route get success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>appId</li>
                       <li>routeId</li>               
                       <li>routeName</li>               
                       <li>createdDate</li>               
                       <li>points</li>               
                       <li>pickupPoints</li>               
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E315</td>
               <td>No route found for application id : &ltapplicationId&gt and route id : &ltrouteId&gt</td>
           </tr>
           <tr>
               <td>E330</td>
               <td>&ltparameter&gt not found or empty in the request</td>
           </tr>
          
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "routeId":"qqdkmyfd"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S330",
           "statusDetail":"Route get success",
           "responseData": {
                   "appId":"zfpqb6e9",
                      "routeId":"qqdkmyfd",
                      "routeName":"r1",
                      "createdDate":"2017-06-30",
                      "points":  [
                               {
                                   "name": 1,
                                   "lat": 7.100812,
                                   "lng": 79.91429,
                                   "dis": 0,
                                   "dist": 0
                               },
                               {
                                   "name": 2,
                                   "lat": 7.1007679216833,
                                   "lng": 79.914281300323,
                                   "dis": 4.9369,
                                   "dist": 4.9369
                               },
                                  ....           
                               {
                                   "name": 41,
                                   "lat": 7.0990811646065,
                                   "lng": 79.91419940325,
                                   "dis": 5.1091,
                                   "dist": 194.9848
                               }
                                ],
                               "pickupPoints": [
                                   {
                                       "p_name": 30,
                                       "pu_id": "30ciw",
                                       "lat": 7.0995269975049,
                                       "lng": 79.914144432624,
                                       "dis": 5.1091,
                                       "Dist": 145.0134
                                   }
                               ]
                           }
                       }
```
   </pre>
</li>
</ul>

<hr>

<h2 class="apiTitle">3.3. Route Update Service</h2>

<ul>
<li><b>DESCRIPTION : </b>to get route details for a given application id and route</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/daproute/v1.0/update</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
  <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>routeId</td>
       <td>Route unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>routeName</td>
       <td>Unique name for the route</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>jsonPath</td>
       <td>Path point array in json format</td>
       <td>String '[[lat,lng],[lat,lng],..]'</td>
       <td>required</td>
   </tr>
    </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S360</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Route update success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>route
                           <ul>
                               <li>appId</li>
                               <li>routeId</li>
                               <li>routeName</li>
                               <li>createdDate</li>
                           </ul>
                       </li>
                       <li>resampledPath
                           <ul>
                               <li>[[lat,lng],[lat,lng],...]</li>
                           </ul>
                       </li>                              
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E315</td>
               <td>No route found for application id : &ltapplicationId&gt and route id : &ltrouteId&gt</td>
           </tr>
           <tr>
               <td>E360</td>
               <td>&ltparameter&gt not found or empty in the request</td>
           </tr>
           <tr>
               <td>E361</td>
               <td>Error in json path format &ltjsonPath&gt </td>
           </tr>
          
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "routeName":"r1",
           "jsonPath":"[[7.100812, 79.914290],[7.100280, 79.914185],[7.099752, 79.914105],[7.098979, 79.914212]]"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S360",
           "statusDetail":"Route update success",
           "responseData": {
                   "route":{
                           "appId":"zfpqb6e9",
                           "routeId":"qqdkmyfd",
                           "routeName":"r1",
                           "createdDate":"2017-06-30",
                           },
                   "resampledPath":[[7.100812,79.91429],[7.1007679216833,79.914281300323],[7.0990811646065,79.91419940325],[7.0990365813163,79.914204900307],...]
           }
```
   </pre>
</li>
</ul>

<hr>



<h2 class="apiTitle">3.4. Add Pickup Point Service</h2>

<ul>
<li><b>DESCRIPTION : </b>Add and manage pickup points for a given route</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/daproute/v1.0/pickup_point/add</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
  <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>routeId</td>
       <td>Route unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>pickupPointName</td>
       <td>Unique name for the pickup point</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>lat</td>
       <td>Pick up point latitude</td>
       <td>float</td>
       <td>required</td>
   </tr>
   <tr>
       <td>lng</td>
       <td>Pick up point longitude</td>
       <td>float</td>
       <td>required</td>
   </tr>    
   </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S310</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Add new pickup point success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>p_name
                           <ul>
                               <li>estimated point name</li>
                           </ul>
                       </li>
                       <li>pu_id
                           <ul>
                               <li>system generated point id</li>
                           </ul>
                       </li> 
                       <li>lat</li>
                       <li>lng</li>
                       <li>lng
                           <ul>
                               <li>distance from the previous point</li>
                           </ul>
                       </li>
                       <li>dist
                           <ul>
                               <li>total distance from the beginning</li>
                           </ul>
                       </li>
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E310</td>
               <td>&ltparameter&gt not found or empty in the request</td>
           </tr>
           <tr>
               <td>E311</td>
               <td>No approximation point found for lat : &ltlat&gt lng : &ltlng&gt</td>
           </tr>
            <tr>
                <td>E312</td>
                <td>Error in format lat : &ltlat&gt lng : &ltlng&gt</td>
            </tr> 
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "pickupPointName":"pickup1",
           "routeId":"qqdkmyfd",
           "lat":"7.099525",
           "lng":"79.914151"
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S310",
           "statusDetail":"Add new pickup point success",
           "responseData": {
                           "p_name":30,
                           "pu_id":"30ciw",
                           "lat":7.0995269975049,
                           "lng":79.914144432624,
                           "dis":5.1091,
                           "dist":145.0134
           }
```
   </pre>
</li>
</ul>

<hr>

<h2 class="apiTitle">3.5. Remove Pickup Point Service</h2>

<ul>
<li><b>DESCRIPTION : </b>remove pickup point in a given route</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/daproute/v1.0/pickup_point/remove</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
   <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>      
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>routeId</td>
       <td>Route unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>pickupPointId</td>
       <td>Pickup point unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>    
   </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S340</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Remove pickup point success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>routeId</li>
                       <li>pointId</li>
                       <li>latitude</li>
                       <li>longitude</li>
                       <li>distance_from_previous_node</li>
                       <li>distance_from_the_beginning</li>
                       <li>pickup_point_id</li>
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E315</td>
               <td>No route found for application id : &ltapplicationId&gt route id : &ltrouteId&gt</td>
           </tr>
           <tr>
               <td>E340</td>
               <td>&ltparameter&gt not found or empty in the request</td>
           </tr>
            <tr>
                <td>E312</td>
                <td>No pickup point found for application id : &ltapplicationId&gt routeId : &ltrouteId&gt pickup point id : &ltpickupPointId&gt </td>
            </tr> 
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "routeId":"qqdkmyfd",
           "pickupPointId":"40hfv"
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S340",
           "statusDetail":"Remove pickup point success",
           "responseData": {
                           "routeId":"qqdkmyfd",
                           "pointId":40,
                           "latitude":7.0990811646065,
                           "longitude":79.91419940325,
                           "distance_from_previous_node":5.1091,
                           "distance_from_the_beginning":194.9848,
                           "pickup_point_id":"40hfv"
           }
```
   </pre>
</li>
</ul>

<hr>



<h2 class="apiTitle">3.6. Route Remove Service</h2>

<ul>
<li><b>DESCRIPTION : </b> to get route details for a given application id and route</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/daproute/v1.0/remove</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
   <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>routeId</td>
       <td>Route unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>   
   </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S350</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Route remove success.</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>appId</li>
                       <li>routeId</li>
                       <li>routeName</li>
                       <li>createdDate</li>
                       <li>points</li>
                       <li>pickupPoints</li>
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E315</td>
               <td>No route found for application id : &ltapplicationId&gt route id : &ltrouteId&gt</td>
           </tr>
           <tr>
               <td>E350</td>
               <td>&ltparameter&gt not found in the request</td>
           </tr>
            <tr>
                <td>E370</td>
                <td>Route for routeId : &ltrouteId&gt already removed</td>
            </tr> 
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "routeId":"qqdkmyfd"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S350",
           "statusDetail":"Route remove success",
           "responseData": {
                           "appId":"zfpqb6e9",
                           "routeId":"qqdkmyfd",
                           "routeName":"r1",
                           "createdDate":"2017-06-30",
                           "points": [
                                      {
                                          "name": 1,
                                          "lat": 7.100812,
                                          "lng": 79.91429,
                                          "dis": 0,
                                          "dist": 0
                                      },
                                      {
                                          "name": 2,
                                          "lat": 7.1007679216833,
                                          "lng": 79.914281300323,
                                          "dis": 4.9369,
                                          "dist": 4.9369
                                      },
                                  ....           
                                      {
                                          "name": 41,
                                          "lat": 7.0990811646065,
                                          "lng": 79.91419940325,
                                          "dis": 5.1091,
                                          "dist": 194.9848
                                      }
                                  ],
                            "pickupPoints": [
                                      {
                                          "p_name": 30,
                                          "pu_id": "30ciw",
                                          "lat": 7.0995269975049,
                                          "lng": 79.914144432624,
                                          "dis": 5.1091,
                                          "Dist": 145.0134
                                      }
                                  ]
                           }
            }
```
   </pre>
</li>
</ul>

<hr>
<h1 class="title">4. SCHEDULE API </h1>
<p class="introduction">To create and manage schedules</p>
<h2 class="apiTitle">4.1.Schedule Create Service</h2>

<ul>
<li><b>DESCRIPTION : </b>Create new schedule</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapschedule/v1.0/create</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
  <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>routeId</td>
       <td>Route unique id</td>
       <td>string</td>
       <td>optional</td>
   </tr>   
   <tr>
       <td>vehicleId</td>
       <td>Vehicle unique id</td>
       <td>string</td>
       <td>required</td>
   </tr> 
   <tr>
       <td>date</td>
       <td>Schedule date. Format Y-m-d</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>alertTypes</td>
       <td>Alert type details
           <ul>
               <li>CONVERGENCE_TIME</li>
               <li>CONVERGENCE_DISTANCE_LOS</li>
               <li>CONVERGEN7CE_DISTANCE_PATH</li>
               <li>DIVERGENCE_PATH</li>
               <li>DIVERGENCE_POINT</li>
               <li>DIVERGENCE_REGION</li>
           </ul>
       </td>
       <td>string</td>
       <td>required</td>
   </tr>
   </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S401</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Schedule create success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>appId</li>
                       <li>routeId</li>
                       <li>vehicleId</li>
                       <li>scheduleId</li>
                       <li>startTime</li>
                       <li>endTime</li>
                       <li>status
                           <ul>
                               <li>1 => PENDING</li>
                               <li>2 => STARTED</li>
                               <li>3 => ENDED</li>
                           </ul>
		</li>
		<li>alertTypes</li>
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E260</td>
               <td>No vehicle found for application id : &ltapplicationId&gt vehicle id : &ltvehicleId&gt</td>
           </tr>
           <tr>
               <td>E315</td>
               <td>No route found for application id : &ltapplicationId&gt route id : &ltrouteId&gt</td>
           </tr>
           <tr>
               <td>E401</td>
               <td>&ltparameter&gt not found or empty in the request</td>
           </tr>
            <tr>
                <td>E402</td>
                <td>Error in date format : &lt date string &gt</td>
            </tr> 
            <tr>
                <td>E403</td>
                <td>Alert type : &ltalert type&gt not valid without routeId</td>
            </tr> 
            <tr>
                <td>E404</td>
                <td>&ltvalueName&gt value is not found for alert type : &ltalertType&gt</td>
            </tr> 
            <tr>
                <td>E405</td>
                <td>&ltvalueName&gt value is not &ltdataType&gt for the alert type : &ltalertType&gt </td>
            </tr> 
            <tr>
                <td>E406</td>
                <td>Alert type does not match</td>
            </tr> 
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "vehicleId":"hq37tjsq",
           "routeId":"qqdkmyfd",
           "date":"2017-07-26"
           "alertTypes":  [
                           {
                             "type" :"CONVERGENCE_TIME",
                             "time" : 5
                           },
                         {
                            “type”:”CONVERGENCE_DISTANCE”,
                            “distance”:500
                         }
                         ]
        }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S401",
           "statusDetail":"Schedule create success",
           "responseData": {
                           "appId":"zfpqb6e9",
                           "routeId":"qqdkmyfd",
                           "vehicleId":"hq37tjsq",
                           "scheduleId":"uugxl9kj",
                           "scheduleDate":"2017-07-26",
                           "startTime": null,
                           "endTime":null,
                           "status":1,
                           "alertTypes":  [
                                               {
                                                 "type" :"CONVERGENCE_TIME",
                                                 "time" : 5
                                               },
                                             {
                                                “type”:”CONVERGENCE_DISTANCE”,
                                                “distance”:500
                                             }
                                         ]
                           }
            }
```
   </pre>
</li>
</ul>

<hr>

<h2 class="apiTitle">4.2. Schedule Update Service</h2>

<ul>
<li><b>DESCRIPTION : </b>update a schedule</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapschedule/v1.0/create/update</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
   <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>authorization</td>
               <td>Content-Type</td>
            </tr>
            <tr>
            <td>application/json</td>
            <td>Bearer [access token]</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>scheduleId</td>
       <td>Schedule unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>alertTypes</td>
       <td>Alert type details
           <ul>
               <li>CONVERGENCE_TIME</li>
               <li>CONVERGENCE_DISTANCE_LOS</li>
               <li>CONVERGEN7CE_DISTANCE_PATH</li>
               <li>DIVERGENCE_PATH</li>
               <li>DIVERGENCE_POINT</li>
               <li>DIVERGENCE_REGION</li>
           </ul>
       </td>
       <td>string</td>
       <td>required</td>
   </tr>
   </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S407</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Schedule update success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>appId</li>
                       <li>routeId</li>
                       <li>vehicleId</li>
                       <li>scheduleId</li>
		<li>scheduleDate</li>
                       <li>startTime</li>
                       <li>endTime</li>
                       <li>status
                           <ul>
                               <li>1 => PENDING</li>
                               <li>2 => STARTED</li>
                               <li>3 => ENDED</li>
                           </ul>
		</li>
		<li>alertTypes</li>
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
                 <td>E403</td>
                 <td>Alert type : &ltalert type&gt not valid without routeId</td>
             </tr> 
             <tr>
                 <td>E404</td>
                 <td>&ltvalueName&gt value is not found for alert type : &ltalertType&gt</td>
             </tr> 
             <tr>
                 <td>E405</td>
                 <td>&ltvalueName&gt value is not &ltdataType&gt for the alert type : &ltalertType&gt </td>
             </tr> 
             <tr>
                 <td>E406</td>
                 <td>Alert type does not match</td>
             </tr> 
           <tr>
               <td>E407</td>
               <td>&ltparameter&gt not found or empty in the request</td>
           </tr>
           <tr>
               <td>E415</td>
               <td>No Schedule found for application id : &ltapplicationId&gt schedule id : &ltscheduleId&gt ></td>
           </tr>
           <tr>
               <td>E420</td>
               <td>The schedule found for scheduleId : &ltscheduleId&gt is started or ended</td>
           </tr>
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```  
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```  
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```  
       {
           "applicationId":"zfpqb6e9",
           "vehicleId":"hq37tjsq"
           "alertTypes":  [
                           {
                             "type" :"CONVERGENCE_TIME",
                             "time" : 5
                           },
                         {
                            “type”:”CONVERGENCE_DISTANCE”,
                            “distance”:500
                         }
                         ]
        }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S407",
           "statusDetail":"Schedule create success",
           "responseData": {
                           "appId":"zfpqb6e9",
                           "routeId":"qqdkmyfd",
                           "vehicleId":"hq37tjsq",
                           "scheduleId":"uugxl9kj",
                           "scheduleDate":"2017-07-26",
                           "startTime": null,
                           "endTime":null,
                           "status":1,
                           "alertTypes":  [
                                               {
                                                 "type" :"CONVERGENCE_TIME",
                                                 "time" : 5
                                               },
                                             {
                                                “type”:”CONVERGENCE_DISTANCE”,
                                                “distance”:500
                                             }
                                         ]
                           }
            }
```
   </pre>
</li>
</ul>

<hr>


<h2 class="apiTitle">4.3. Schedule Start Service</h2>

<ul>
<li><b>DESCRIPTION : </b>Start schedule</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapschedule/v1.0/create/start</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
   <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>       
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>scheduleId</td>
       <td>Schedule unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S410</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Schedule start success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>appId</li>
                       <li>routeId</li>
                       <li>vehicleId</li>
                       <li>scheduleId</li>
                       <li>scheduleDate</li>
                       <li>startTime</li>
                       <li>endTime</li>
                       <li>status
                           <ul>
                               <li>1 => PENDING</li>
                               <li>2 => STARTED</li>
                               <li>3 => ENDED</li>
                           </ul>
		</li>
		<li>akertTypes</li>
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E410</td>
               <td>&ltparameter&gt not found or empty in the request</td>
           </tr>
           <tr>
               <td>E411</td>
               <td>Schedule : $ltscheduleId$gt is already started</td>
           </tr>
           <tr>
               <td>E412</td>
               <td>Schedule : $ltscheduleId$gt is already ended</td>
           </tr>
           <tr>
               <td>E415</td>
               <td>No schedule found for application id : $ltapplicationId&gt scheduleId : &ltscheduleId&gt</td>
           </tr>
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "scheduleId":"uugxl9kj"
        }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S410",
           "statusDetail":"Schedule start success",
           "responseData": {
                           "appId":"zfpqb6e9",
                           "routeId":"qqdkmyfd",
                           "vehicleId":"hq37tjsq",
                           "scheduleId":"uugxl9kj",
                           "scheduleDate":"2017-07-26",
                           "startTime":"2017-06-26 09:40:14",
                           "endTime":null,
                           "status":2,
                           "alertTypes":  [
                                               {
                                                 "type" :"CONVERGENCE_TIME",
                                                 "time" : 5
                                               },
                                             {
                                                “type”:”CONVERGENCE_DISTANCE”,
                                                “distance”:500
                                             }
                                         ]
                           }
            }
```
   </pre>
</li>
</ul>

<hr>


<h2 class="apiTitle">4.4. Schedule End Service</h2>

<ul>
<li><b>DESCRIPTION : </b>To end a schedule</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapschedule/v1.0/create/end</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
    <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>scheduleId</td>
       <td>Schedule unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S420</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Schedule end success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>appId</li>
                       <li>routeId</li>
                       <li>vehicleId</li>
                       <li>scheduleId</li>
                       <li>scheduleDate</li>
                       <li>startTime</li>
                       <li>endTime</li>
                       <li>status
                           <ul>
                               <li>1 => PENDING</li>
                               <li>2 => STARTED</li>
                               <li>3 => ENDED</li>
                           </ul>
                        </li>
                        <li>alertTypes</li>
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E411</td>
               <td>Schedule : $ltscheduleId$gt is already started</td>
           </tr>
           <tr>
               <td>E415</td>
               <td>No schedule found for application id : $ltapplicationId&gt scheduleId : &ltscheduleId&gt</td>
           </tr>
           <tr>
               <td>E420</td>
               <td>&ltparameter&gt not found or empty in the request</td>
           </tr>
           <tr>
               <td>E480</td>
               <td>Schedule : $ltscheduleId$gt is already ended</td>
           </tr>
         
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"ilul8s00",
           "scheduleId":"uuycw1ua"
        }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S420",
           "statusDetail":"Schedule end success",
           "responseData": {
                           "appId":"ilul8s00",
                           "routeId":"blrzqdmw",
                           "vehicleId":"6pm5wieh",
                           "scheduleId":"uuycw1ua",
                           "scheduleDate":"2017-06-26",
                           "startTime":"2017-06-26 09:40:14",
                           "endTime":"2017-06-26 11:48:33",
                           "status":3,
                           "alertTypes":  [
                                               {
                                                 "type" :"CONVERGENCE_TIME",
                                                 "time" : 5
                                               },
                                             {
                                                “type”:”CONVERGENCE_DISTANCE”,
                                                “distance”:500
                                             }
                                         ]
                           }
            }
```
   </pre>
</li>
</ul>

<hr>


<h2 class="apiTitle">4.5. Schedule Status Service</h2>

<ul>
<li><b>DESCRIPTION : </b>To end a schedule</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapschedule/v1.0/create/get</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
   <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>authorization</td>
               <td>Content-Type</td>
            </tr>
            <tr>
            <td>application/json</td>
            <td>Bearer [access token]</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>date</td>
       <td>Schedule date . Format Y-m-d</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>routeId</td>
       <td>Unique route id</td>
       <td>string</td>
       <td>optional</td>
   </tr>
   <tr>
       <td>vehicleId</td>
       <td>Unique vehicle id</td>
       <td>string</td>
       <td>optional</td>
   </tr>
   <tr>
       <td>scheduleId</td>
       <td>unique schedule id</td>
       <td>string</td>
       <td>optional</td>
   </tr>
   </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S430</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Schedule get success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>appId</li>
                       <li>routeId</li>
                       <li>vehicleId</li>
                       <li>scheduleId</li>
                       <li>scheduleDate</li>
                       <li>startTime</li>
                       <li>endTime</li>
                       <li>status
                           <ul>
                               <li>1 => PENDING</li>
                               <li>2 => STARTED</li>
                               <li>3 => ENDED</li>
                           </ul>
                        </li>
                       <li>alertTypes</li>
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E260</td>
               <td>No vehicle found for application id : &ltapplicationId&gt vehicle id : &ltvehicle Id&gt</td>
           </tr>
           <tr>
               <td>E315</td>
               <td>No route found for application id : &ltapplicationId&gt route id : &ltrouteId&gt</td>
           </tr>
           <tr>
               <td>E415</td>
               <td>No schedule found for application id : $ltapplicationId&gt scheduleId : &ltscheduleId&gt</td>
           </tr>
           <tr>
               <td>E430</td>
               <td>&ltparameter&gt not found or empty in the request</td>
           </tr>
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"ilul8s00",
           "date":"2017-07-26",
           "vehicleId":"6pm5wieh",
           "routeId":"blrzqdmw",
           "scheduleId":"uuycw1ua"
        }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S430",
           "statusDetail":"Schedule end success",
           "responseData": {
                           "appId":"ilul8s00",
                           "routeId":"blrzqdmw",
                           "vehicleId":"6pm5wieh",
                           "scheduleId":"uuycw1ua",
                           "scheduleDate":"2017-06-26",
                           "startTime":"2017-06-26 09:40:14",
                           "endTime":"2017-06-26 11:48:33",
                           "status":3,
                           "alertTypes":  [
                                               {
                                                 "type" :"CONVERGENCE_TIME",
                                                 "time" : 5
                                               },
                                             {
                                                “type”:”CONVERGENCE_DISTANCE”,
                                                “distance”:500
                                             }
                                         ]
                           }
            }
``` 
   </pre>
</li>
</ul>

<hr>

<h1 class="title">5. GEOFENCE API</h1>
<p class="introduction">To create and manage circular, path and polygon geofences.</p>

<h2 class="apiTitle">5.1. Circular Geofence Create Service</h2>

<ul>
<li><b>DESCRIPTION : </b>To create circular geofences</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapgeofence/v1.0/circular/create</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
   <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>     
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>geofenceName</td>
       <td>Name to identify the geofence</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>lat</td>
       <td>latitude</td>
       <td>float</td>
       <td>required</td>
   </tr>
   <tr>
       <td>lng</td>
       <td>longitude</td>
       <td>float</td>
       <td>required</td>
   </tr>
   <tr>
       <td>radius</td>
       <td>Radius of the circular geofence</td>
       <td>float</td>
       <td>required</td>
   </tr>
   </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S501</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Circular geofence create success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>applicationId</li>
                       <li>geofenceId</li>
                       <li>geofenceName</li>
                       <li>radius</li>
                       <li>lat</li>
                       <li>lng</li>
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E312</td>
               <td>Error in format lat : &lt lat &gt lng : &lt lng &gt </td>
           </tr> 
           <tr>
               <td>E501</td>
               <td>&ltparameter&gt not found or empty in the request</td>
           </tr>
           <tr>
                <td>E1001</td>
                <td>Error in format : Radius is not a float number &lt value &gt</td>
            </tr>
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "geofenceName":"cgf1",
           "lat":7.099081164,
           "lng":79.9141994,
           "Radius":"23.345"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
``` 
       {
           "statusCode":"S501",
           "statusDetail":"Add Circular Geofence Success",
           "responseData": {
                           "applicationId":"zfpqb6e9",
                           "geofenceId":"0ynnznwh",
                           "geofenceName":"cgf1",
                           "radius":23.345,
                           "lat":7.099081164,
                           "lng":79.9141994
                           }
       }
```
   </pre>
</li>
</ul>

<hr>






<h2 class="apiTitle">5.2. Circular Geofence Edit Service</h2>

<ul>
<li><b>DESCRIPTION : </b>To update circular geofences</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapgeofence/v1.0/circular/edit</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
  <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>      
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>geofenceId</td>
       <td>Unique id for the circular geofence </td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>lat</td>
       <td>latitude</td>
       <td>float</td>
       <td>required</td>
   </tr>
   <tr>
       <td>lng</td>
       <td>longitude</td>
       <td>float</td>
       <td>required</td>
   </tr>
   <tr>
       <td>radius</td>
       <td>Radius of the circular geofence</td>
       <td>float</td>
       <td>required</td>
   </tr>
   </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S502</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Circular geofence edit success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>applicationId</li>
                       <li>geofenceId</li>
                       <li>geofenceName</li>
                       <li>radius</li>
                       <li>lat</li>
                       <li>lng</li>
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
               <td>E312</td>
               <td>Error in format lat : &lt lat &gt lng : &lt lng &gt </td>
           </tr>
           <tr>
                <td>E502</td>
                <td>&ltparameter&gt not found or empty in the request</td>
            </tr>
            <tr>
               <td>E540</td>
               <td>No circular geofence found for application id : &ltapplicationId&gt geofence id :  &ltgeofenceId&gt </td>
           </tr> 
           <tr>
                <td>E1001</td>
                <td>Error in format : Radius is not a float number &lt value &gt</td>
            </tr>
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "geofenceId":"0ynnznwh",
           "lat":7.099081164,
           "lng":79.9141994,
           "Radius":"23.345"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S502",
           "statusDetail":"Update circular geofence success",
           "responseData": {
                           "applicationId":"zfpqb6e9",
                           "geofenceId":"0ynnznwh",
                           "geofenceName":"cgf1",
                           "radius":23.345,
                           "lat":7.099081164,
                           "lng":79.9141994
                           }
       }
```
   </pre>
</li>
</ul>

<hr>


<h2 class="apiTitle">5.3. Circular Geofence View Service</h2>

<ul>
<li><b>DESCRIPTION : </b> To view circular geofences</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapgeofence/v1.0/circular/get</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
  <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>geofenceId</td>
       <td>Unique id for the circular geofence </td>
       <td>string</td>
       <td>required</td>
   </tr>
   </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S504</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Get Circular Geofence Sucess</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>applicationId</li>
                       <li>geofenceId</li>
                       <li>geofenceName</li>
                       <li>radius</li>
                       <li>lat</li>
                       <li>lng</li>
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
                <td>E504</td>
                <td>&ltparameter&gt not found or empty in the request</td>
            </tr>
            <tr>
               <td>E540</td>
               <td>No circular geofence found for application id : &ltapplicationId&gt geofence id :  &ltgeofenceId&gt </td>
           </tr> 
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "geofenceId":"0ynnznwh"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S504",
           "statusDetail":"Get Circular Geofence Success",
           "responseData": {
                           "applicationId":"zfpqb6e9",
                           "geofenceId":"0ynnznwh",
                           "geofenceName":"cgf1",
                           "radius":23.345,
                           "lat":7.099081164,
                           "lng":79.9141994
                           }
       }
```
   </pre>
</li>
</ul>

<hr>


<h2 class="apiTitle">5.4. Circular Geofence Remove Service</h2>

<ul>
<li><b>DESCRIPTION : </b> To remove circular geofences. </li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapgeofence/v1.0/circular/remove</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
   <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>      
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>geofenceId</td>
       <td>Unique id for the circular geofence </td>
       <td>string</td>
       <td>required</td>
   </tr>
   </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S503</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Remove Circular Geofence Success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>applicationId</li>
                       <li>geofenceId</li>
                       <li>geofenceName</li>
                       <li>radius</li>
                       <li>lat</li>
                       <li>lng</li>
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
                <td>E503</td>
                <td>&ltparameter&gt not found or empty in the request</td>
            </tr>
            <tr>
               <td>E540</td>
               <td>No circular geofence found for application id : &ltapplicationId&gt geofence id :  &ltgeofenceId&gt </td>
           </tr> 
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "geofenceId":"0ynnznwh"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S503",
           "statusDetail":"Remove Circular Geofence Success",
           "responseData": {
                           "applicationId":"zfpqb6e9",
                           "geofenceId":"0ynnznwh",
                           "geofenceName":"cgf1",
                           "radius":23.345,
                           "lat":7.099081164,
                           "lng":79.9141994
                           }
       }
```
   </pre>
</li>
</ul>

<hr>


<h2 class="apiTitle">5.5. Polygon Geofence Create Service</h2>

<ul>
<li><b>DESCRIPTION : </b> To remove circular geofences. </li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapgeofence/v1.0/polygon/create</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
   <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>authorization</td>
               <td>Content-Type</td>
            </tr>
            <tr>
            <td>application/json</td>
            <td>Bearer [access token]</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>geofenceId</td>
       <td>Unique id for the circular geofence </td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>pointArray</td>
       <td>Polygon point array ( json string )</td>
       <td>string</td>
       <td>required</td>
   </tr>
   </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S510</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Polygon geofence create success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>applicationId</li>
                       <li>geofenceId</li>
                       <li>geofenceName</li>
                       <li>polygon
                           <ul>
                               <li>Array containing all points in polygon</li>
                           </ul>
                       </li>
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
	  <td>E312</td>
               <td>Error in format : &ltlat&gt lng : &ltlng&gt </td>
           </tr>
           <tr>
                <td>E510</td>
                <td>&ltparameter&gt not found or empty in the request</td>
            </tr>
            <tr>
               <td>E535</td>
               <td>Error in pointArray format &ltpointArray&gt</td>
           </tr> 
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "geofenceName":"poly1",
           "pointArray":"[[6.908377, 79.869157],[6.904119, 79.880633],[6.897827, 79.875394],[6.901664, 79.864887],[6.908377, 79.869157]]"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S510",
           "statusDetail":"Add Polygon Geofence Success",
           "responseData": {
                           "applicationId":"zfpqb6e9",
                           "geofenceId":"1s05anjl",
                           "geofenceName":"poly1",
                           "polygon": [
                                      [
                                          [
                                              79.869157,
                                              6.908377
                                          ],
                                          ...,
                                          [
                                              79.869157,
                                              6.908377
                                          ]
                                      ]
                                     ]
                           }
       }
```
   </pre>
</li>
</ul>

<hr>

<h2 class="apiTitle">5.6. Polygon Geofence Update Service</h2>

<ul>
<li><b>DESCRIPTION : </b> To update polygon geofences</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapgeofence/v1.0/polygon/update</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
   <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>geofenceId</td>
       <td>Unique id for the circular geofence </td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>geofenceName</td>
       <td>Name to identify the geofence</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>pointArray</td>
       <td>Polygon point array ( json string )</td>
       <td>string</td>
       <td>required</td>
   </tr>
   </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S511</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Polygon geofence update success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>applicationId</li>
                       <li>geofenceId</li>
                       <li>geofenceName</li>
                       <li>polygon
                           <ul>
                               <li>Array containing all points in polygon</li>
                           </ul>
                       </li>
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>	
              <td>E312</td>
               <td>Error in format : &ltlat&gt lng : &ltlng&gt </td>
           </tr>
           <tr>
                <td>E511</td>
                <td>&ltparameter&gt not found or empty in the request</td>
            </tr>
            <tr>
               <td>E535</td>
               <td>Error in pointArray format &ltpointArray&gt</td>
           </tr>
           <tr>
               <td>E540</td>
               <td>No polygon geofence found for application id : &ltapplicationId&gt geofence id : &ltgeofenceId&gt </td>
           </tr>
           <tr>
               <td>E550</td>
               <td>Both geofenceName and pointArray are not in the request</td>
           </tr>
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "geofenceId":"1s05anjl",
           "geofenceName":"poly-updated",
           "pointArray":"[[6.908377, 79.869157],[6.904119, 79.880633],[6.897827, 79.875394],[6.901664, 79.864887],[6.908377, 79.869157]]"
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S511",
           "statusDetail":"Update Polygon Geofence Success",
           "responseData": {
                           "applicationId":"zfpqb6e9",
                           "geofenceId":"1s05anjl",
                           "geofenceName":"poly-updated"
                           }
       }
```
</ul>

<hr>

<h2 class="apiTitle">5.7. Polygon Geofence View Service</h2>

<ul>
<li><b>DESCRIPTION : </b> To view polygon geofences</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapgeofence/v1.0/polygon/get</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
   <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>geofenceId</td>
       <td>Unique id for the circular geofence </td>
       <td>string</td>
       <td>optional</td>
   </tr>
   </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S512</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Polygon geofence get success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>applicationId</li>
                       <li>geofenceId</li>
                       <li>geofenceName</li>
                       <li>polygon
                           <ul>
                               <li>Array containing all points in polygon</li>
                           </ul>
                       </li>
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
                <td>E512</td>
                <td>&ltparameter&gt not found or empty in the request</td>
            </tr>
           <tr>
               <td>E540</td>
               <td>No polygon geofence found for application id : &ltapplicationId&gt geofence id : &ltgeofenceId&gt </td>
           </tr>
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "geofenceId":"1s05anjl",
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S512",
           "statusDetail":"Get Polygon Geofence Success",
           "responseData": {
                           "applicationId":"zfpqb6e9",
                           "geofenceId":"1s05anjl",
                           "geofenceName":"poly-updated"
                          
"polygon":[
                [  
                [
                            79.869157,  
                            6.908377 
                            ],
                            ... 
                            [  
                            79.869157,  6.908377 
                            ] 
                         ] 
                    ]
              ]                          
                          
         }
}
```
</ul>
<hr>

<h2 class="apiTitle">5.8. Polygon Geofence Remove Service</h2>

<ul>
<li><b>DESCRIPTION : </b> To remove polygon geofences</li>
<li><b>URL:</b>https://ideabiz.lk/apicall/dapgeofence/v1.0/polygon/remove</li>
<li><b>METHOD:</b> POST</li>
<li><b>Headers :</b>
   <table>
        <thead>
            <th>Key</th>
            <th>Value</th>
        </thead>
        <tbody>
            <tr>
               <td>accept</td>
               <td>application/json</td>
            </tr>
            <tr>
            <td>authorization</td>
            <td>Bearer [access token]</td>
            </tr>
            <tr>
           <td>Content-Type</td>
            <td>application/json</td>
            </tr>
        </tbody>
   </table>    
</li>
<li><b>DATA PARAMETERS (POST json parameters) : </b>
<table>
<thead>
    <th>Parameter Name</th>
    <th>Description</th>
    <th>Type</th>
    <th>Required / Optional</th>
</thead>
<tbody>
   <tr>
       <td>applicationId</td>
       <td>Application unique id</td>
       <td>string</td>
       <td>required</td>
   </tr>
   <tr>
       <td>geofenceId</td>
       <td>Unique id for the circular geofence </td>
       <td>string</td>
       <td>required</td>
   </tr>
   </tbody>
</table>

</li>
<li><b>SUCCESS RESPONSE:</b>
    <table>
        <tbody>
           <tr>
               <th>Status Code</th>
               <td>S513</td>
           <tr>
           <tr>
               <th>StatusDetail</th>
               <td>Remove Polygon Geofence success</td>
           <tr>
           <tr>
               <th>responseData</th>
               <td>
                   <ul>
                       <li>applicationId</li>
                       <li>geofenceId</li>
                       <li>geofenceName</li>
                       <li>polygon
                           <ul>
                               <li>Array containing all points in polygon</li>
                           </ul>
                       </li>
                   </ul>
               </td>
        </tbody>
    </table>
</li>
<li><b>ERROR RESPONSE:</b>
   <table>
       <thead>
           <th width="30%">Error code </th>
           <th width="70%">Error message </th>
       </thead>
       <tbody>
           <tr>
               <td>E130</td>
               <td>No application found for application id : &ltapplicationId&gt</td>
           </tr>
           <tr>
                <td>E513</td>
                <td>&ltparameter&gt not found or empty in the request</td>
            </tr>
           <tr>
               <td>E540</td>
               <td>No polygon geofence found for application id : &ltapplicationId&gt geofence id : &ltgeofenceId&gt </td>
           </tr>
       </tbody>
   </table>
</li>
<li>
<b>SAMPLE HEADER : </b>
<pre>
```
accept:application/json
authorization:Bearer 3efd841a652a45d978684ab9f357e95
Content-Type:application/json
```
</pre>
</li>
<li>
<b>SAMPLE REQUEST : </b>
   <pre>
```
       {
           "applicationId":"zfpqb6e9",
           "geofenceId":"1s05anjl",
       }
```
   </pre>
</li>
<li>
<b>SAMPLE RESPONSE : </b>
   <pre>
```
       {
           "statusCode":"S513",
           "statusDetail":"Remove Polygon Geofence Success",
           "responseData": {
                           "applicationId":"zfpqb6e9",
                           "geofenceId":"1s05anjl",
                           "geofenceName":"poly-updated",
                           
"polygon":[
                [  
                [
                            79.869157,  
                            6.908377 
                            ],
                            ... 
                            [  
                            79.869157,  6.908377 
                            ] 
                         ] 
                    ]
              ]
        }
 }
```
</ul>
