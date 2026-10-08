# Contact Developer Support

The Ideabiz Developer Support team is dedicated to assisting developers, partners, and businesses with onboarding, API troubleshooting, and production inquiries.

---

## Support Channels & Hours

- **Email:** [support@ideabiz.lk](mailto:support@ideabiz.lk)
- **Direct Phone:** `+94 767 222 161`
- **Operating Hours:** Monday to Friday, 8:30 AM – 5:00 PM (Sri Lanka Time / IST, UTC+5:30)

---

## Quick Self-Check Before Raising a Ticket

Before contacting support, check if your issue is caused by one of these common setup issues:
1. **Did your token expire?** Access tokens expire after 1 hour. Check if you received error code `900903` and need to [Refresh Your Token](../Getting_Started/Token_Manegment.md).
2. **Is your application approved?** Check [My Subscriptions](https://www.ideabiz.lk) to confirm that your app and API subscriptions are in `Approved` status.
3. **Are you testing Header Enrichment over Wi-Fi?** Header Enrichment only works on mobile cellular data.
4. **Is your phone number format valid?** Numbers should typically be formatted as `947XXXXXXXX` without spaces, hyphens, or `+` signs (unless specifically required by the endpoint).

---

## Support Request Email Template

To help our technical team resolve your issue as fast as possible, please copy and fill out the template below when emailing `support@ideabiz.lk`:

```text
Subject: [Support Request] [Your App Name] - [Short Issue Description]

Hello Ideabiz Support Team,

We are experiencing an issue with the following Ideabiz API integration:

1. Application Name: [Your Registered App Name on Ideabiz]
2. API Being Called: [e.g., SMS API v3 / Balance Check v3 / Payment API]
3. Environment: [Sandbox / Production]
4. Endpoint URL: [e.g., https://ideabiz.lk/apicall/smsmessaging/v3/outbound/87798/requests]
5. Approximate Time of Incident: [Date & Time with Timezone]

--- Details ---
- Request Headers:
  Content-Type: application/json
  Authorization: Bearer [First 10 characters of your token, do not share full token publicly]

- Request Body:
  [Paste your JSON or Form data here]

- Response Status Code:
  [e.g., HTTP 400 / HTTP 401 / HTTP 500]

- Response Body Received:
  [Paste the exact error message or JSON returned by the server]

- Description of Expected Behavior vs. Actual Behavior:
  [Briefly explain what you were trying to accomplish and what went wrong]

Thank you,
[Your Name / Company Name]
[Contact Phone Number]
```