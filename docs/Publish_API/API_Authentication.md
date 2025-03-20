## Authenticate API call from Ideabiz

When authentication API call, Ideabiz use `Authorization` header. If each and every API call authorized, it will pass to the original endpoint. You can use header `X-JWT-Assertion` to identify each request. this header will append by Ideabiz to all api call request.

Other than that, you also can have any headers except `Authorization` 

### IP Validation

    Source IP : 202.69.200.34

### Reading API call information
All request contain header  `X-JWT-Assertion`

it looks like

    eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0=.eyJpc3MiOiJ3c28yLm9yZy9wcm9kdWN0cy9hbSIsImV4cCI6MTQzMzY1MzA2MzcwNSwiaHR0cDovL3dzbzIub3JnL2NsYWltcy9zdWJzY3JpYmVyIjoiYWRtaW4iLCJodHRwOi8vd3NvMi5vcmcvY2xhaW1zL2FwcGxpY2F0aW9uaWQiOiI0MiIsImh0dHA6Ly93c28yLm9yZy9jbGFpbXMvYXBwbGljYXRpb25uYW1lIjoiQXBwbGljYXRpb24gMyIsImh0dHA6Ly93c28yLm9yZy9jbGFpbXMvYXBwbGljYXRpb250aWVyIjoiVW5saW1pdGVkIiwiaHR0cDovL3dzbzIub3JnL2NsYWltcy9hcGljb250ZXh0IjoiL3Ntc21lc3NhZ2luZyIsImh0dHA6Ly93c28yLm9yZy9jbGFpbXMvdmVyc2lvbiI6IjEiLCJodHRwOi8vd3NvMi5vcmcvY2xhaW1zL3RpZXIiOiI2MDAwVFBNIiwiaHR0cDovL3dzbzIub3JnL2NsYWltcy9rZXl0eXBlIjoiUFJPRFVDVElPTiIsImh0dHA6Ly93c28yLm9yZy9jbGFpbXMvdXNlcnR5cGUiOiJBUFBMSUNBVElPTiIsImh0dHA6Ly93c28yLm9yZy9jbGFpbXMvZW5kdXNlciI6ImFub255bW91cyIsImh0dHA6Ly93c28yLm9yZy9jbGFpbXMvZW5kdXNlclRlbmFudElkIjoiLTEyMzQiLCJodHRwOi8vd3NvMi5vcmcvY2xhaW1zL2VtYWlsYWRkcmVzcyI6ImluZm9AaWRlYWJpei5sayIsImh0dHA6Ly93c28yLm9yZy9jbGFpbXMvZ2l2ZW5uYW1lIjoiRGlhbG9nIiwiaHR0cDovL3dzbzIub3JnL2NsYWltcy9sYXN0bmFtZSI6IklkZWFCaXoiLCJodHRwOi8vd3NvMi5vcmcvY2xhaW1zL3JvbGUiOiJhZG1pbixJbnRlcm5hbC9zdWJzY3JpYmVyIn0=.
    

Split this text by `"."`. and get 2nd part. then decode it using base 64

Then you can get JSON object like this

    {
      "iss": "wso2.org/products/am",
      "exp": 1433653063705,
      "http://wso2.org/claims/subscriber": "admin",
      "http://wso2.org/claims/applicationid": "42",
      "http://wso2.org/claims/applicationname": "Application 3",
      "http://wso2.org/claims/applicationtier": "Unlimited",
      "http://wso2.org/claims/apicontext": "/smsmessaging",
      "http://wso2.org/claims/version": "1",
      "http://wso2.org/claims/tier": "6000TPM",
      "http://wso2.org/claims/keytype": "PRODUCTION",
      "http://wso2.org/claims/usertype": "APPLICATION",
      "http://wso2.org/claims/enduser": "anonymous",
      "http://wso2.org/claims/enduserTenantId": "-1234",
      "http://wso2.org/claims/emailaddress": "info@ideabiz.lk",
      "http://wso2.org/claims/givenname": "Dialog",
      "http://wso2.org/claims/lastname": "IdeaBiz",
      "http://wso2.org/claims/role": "admin,Internal/subscriber"
    }


So you can use this  json object to authenticate API call that coming from Ideabiz to your API. and also you can have to verify source using above source IP.
Use `applicationId` and `applicationname` to identify each and apps


