

Once you provide your URL to us, we will enable header enrichment for your IP based on your requirement.

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


##  Plain MSISDN

Because of user privacy, we don't provide plain MSISDN for Header Enrichment feature

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
