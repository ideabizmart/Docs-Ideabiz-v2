

Once you provide your URL to us, we will enable header enrichment for your IP based on your requirement.

> [!IMPORTANT]
> **Key Operational Requirements:**
> 1. **Dialog Mobile Data Only:** The visitor **must** browse using Dialog mobile cellular data (3G/4G/5G). It **will not work over Wi-Fi** or fixed broadband connections.
> 2. **URL Whitelisting:** You must provide your website URL to the Ideabiz team beforehand so Header Enrichment can be provisioned for your domain/IP.

##  Encrypted MSISDN
This MSISDN is a unique identification of specific MSISDN. and its an alpha numeric string. but you can use it for charging and sms APIs

when you're sending this MSISDN to us, please do as below



You will recieve 'msisdn' header to your application (in base64 format)

```
msisdn: dmwdovap66bFrvw=
```
Please base64 decode it, and you will get the MSISDN in the below format, proceed with the next step
```
msisdn: vl^]£÷¬á®À­ÿ
```


Please URL encode it, where the output value will look like below

```
vl%1D%A3%F7%AC%E1%AE%C0%AD%FF
```

### In API Request Body

Please append `etel:+9477-` before the MSISDN in the API request body.

Your request MSISDN in body should look like this

```
etel:+9477-vl%1D%A3%F7%AC%E1%AE%C0%AD%FF
```

### In API Request URL

When you're appending the MSISDN to URL, Please URLEncode it "again"

First append urlencoded `etel:` to the prefix. (Dont append `+`), Then URL encode

```
urlencode('etel:9477-vl%1D%A3%F7%AC%E1%AE%C0%AD%FF')
```

Finally, the MSISDN in the API Request URL should look like below
```
etel%3A9477-vl%251D%25A3%25F7%25AC%25E1%25AE%25C0%25AD%25FF

```


## Key Terminology (Jargon Buster)

- **MSISDN**: The telecom term for a mobile telephone number.
- **Encrypted MSISDN**: For user privacy and security regulations, Dialog does not expose the customer's raw, plain phone number. Instead, a temporary encrypted token is provided. This token is accepted by Ideabiz Payment and SMS APIs.
- **HTTP Header**: Background metadata sent by the browser or network to your server with every web request (like the return address on an envelope).
- **Base64 & URL Encoding**: Standard methods to safely transmit special or binary characters across the internet without corruption.

---


## Reading header

### PHP

```
$msisdn = $_SERVER['HTTP_MSISDN'];
$urlEncodedMsisdn = urlencode("9477-".base64_decode($msisdn));

```

### JAVA
```
//import java.net.URLEncoder;
//import javax.servlet.http.HttpServletRequest;

byte[] basedmsisdn = Base64.decodeBase64(request.getHeader("msisdn"));
String b64decriptedmsisdn = "";
b64decriptedmsisdn = new String(basedmsisdn,StandardCharsets.ISO_8859_1);

msisdn = "9477-" +b64decriptedmsisdn;
String urlEncodedValue = URLEncoder.encode(msisdn, "ISO-8859-1");

```

## Dialog Mobile IP Block

```
175.157.0.0/16
182.161.0.0/19
122.255.44.0/22 
```

## Testing Header Enrichment

Please host bellow PHP file on your server and open it via HE enabled device

```
<?php
echo getenv('REMOTE_ADDR')."<br>";

if(!$_SERVER['HTTP_MSISDN']){
	echo "NO MSISDN HEADER";
	exit;
}
echo $_SERVER['HTTP_MSISDN']."<br>";
var_dump ($_SERVER['HTTP_MSISDN']);
echo "<br>";
echo urlencode ($_SERVER['HTTP_MSISDN'])."<br>";

?>

```
## How to Test Header Enrichment

Use this test script to verify your setup before integrating with full payment or SMS flows.

### Testing Checklist
- [ ] Upload the script below as `test_he.php` to your whitelisted web server.
- [ ] Take a smartphone equipped with an active **Dialog SIM card**.
- [ ] Turn **OFF Wi-Fi**.
- [ ] Turn **ON Mobile Data**.
- [ ] Open the mobile browser and visit `https://yourdomain.com/test_he.php`.
