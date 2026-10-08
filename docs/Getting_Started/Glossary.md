# Ideabiz & Telecom Jargon Buster (Glossary)

> [!NOTE]
> New to telecommunications or API integrations? This reference guide explains all common telecom, payment, and authentication terms in simple, plain English.

---

## Mobile & Telecom Terms

| Term | Full Name | Plain English Explanation | Example |
| :--- | :--- | :--- | :--- |
| **MSISDN** | Mobile Station International Subscriber Directory Number | A mobile phone number formatted with its international country calling code. | `94771234567` *(Sri Lanka: 94 + 77...)* |
| **Encrypted MSISDN** | Encrypted Mobile Number | An anonymous token representing a user's phone number, created to protect user privacy during mobile web browsing. | `etel:+9477-vl%1D...` |
| **Shortcode / Port** | Application Port | A short 4-to-6 digit phone number assigned to your business application by Dialog for sending and receiving SMS. | `87798` |
| **Sender Mask** | Alphanumeric Sender ID | An approved brand name that appears on the user's phone screen instead of a numeric phone number. | `MYBRAND` |
| **MO** | Mobile Originated | A message or session started by the **customer** from their mobile handset (e.g., customer texts your shortcode). | Customer sends "HELP" to 87798 |
| **MT** | Mobile Terminated | A message sent from **your application** ending at the customer's mobile phone (e.g., an OTP or alert). | App sends OTP to customer |
| **USSD** | Unstructured Supplementary Service Data | Real-time interactive text menus triggered on basic and smartphones by dialing numbers with `*` and `#`. | Dialing `#777#` |
| **MCC / MNC** | Mobile Country Code / Mobile Network Code | International standards identifying a country and its cellular carrier. | MCC `413` (Sri Lanka), MNC `02` (Dialog) |
| **HE** | Header Enrichment | A mobile network technology where Dialog automatically identifies a user's cellular connection when they visit your site. | 1-click mobile subscriptions |

---

## Security & Authentication Terms

| Term | What is it? | Plain English Explanation |
| :--- | :--- | :--- |
| **OAuth 2.0** | Authentication Standard | The industry-standard protocol Ideabiz uses to verify that your app has permission to call APIs. |
| **Consumer Key** | Client ID | Your application's unique public username on the Ideabiz portal. |
| **Consumer Secret** | Client Secret | Your application's confidential master password. **Never share this or commit it to public code.** |
| **Basic Auth** | Basic Authorization Header | A single string created by combining `ConsumerKey:ConsumerSecret` into Base64 format. Used only to obtain tokens. |
| **Access Token** | Bearer Token | A temporary passport valid for **1 hour (3,600s)**. Must be sent in the header of every API call. |
| **Refresh Token** | Token Renewer | A special companion key used to get a new Access Token without having to enter your username and password again. |

---

## Web & API Terms

| Term | What is it? | Plain English Explanation |
| :--- | :--- | :--- |
| **Endpoint** | API URL | The web address your code calls to perform an action (e.g., `https://ideabiz.lk/apicall/smsmessaging/...`). |
| **HTTP Method** | Action Type | The verb indicating what you want to do: <br>• `GET`: Read data (e.g., check balance) <br>• `POST`: Create or trigger an action (e.g., send SMS, charge money) |
| **Webhook / Callback** | Reverse API Call | A URL on **your** server that Ideabiz calls when an event happens (e.g., when an SMS is delivered or when a user pays). |
| **Payload / Body** | Request Data | The JSON or form data sent along with an HTTP request containing the details (e.g., message text, phone number, amount). |
| **Rate Limiting / TPS** | Transactions Per Second | The maximum number of API calls your application is allowed to make per second before the server throttles requests (HTTP 503). |

---

## Payment & Wallet Terms

| Term | Plain English Explanation |
| :--- | :--- |
| **eZ Cash** | Dialog's mobile money wallet service allowing users to pay, transfer money, and make merchant purchases from their phone. |
| **AOTC** | **Agent Over The Counter:** An eZ Cash transaction scenario where an authorized merchant or agent initiates a transaction on behalf of an end-customer. |
| **Prepaid** | A phone plan where the customer pays for airtime credit before using services. |
| **Postpaid** | A phone plan where the customer receives a monthly bill and is given a designated credit limit. |
