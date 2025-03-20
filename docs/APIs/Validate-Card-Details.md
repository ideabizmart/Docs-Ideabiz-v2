
###### URL                &lt;protocol&gt;://&lt;host&gt;:&lt;port&gt;/CEPM/api/rest/v1/card/ validate

###### METHOD        POST

**REQUEST**** PARAMS**

| Name | Type | Description | Data Type | Availability | Comment |
| --- | --- | --- | --- | --- | --- |
| Username | Header | Username to login to API | String | Required |   |
| Password | Header | Password to login to API | String | Required |   |
| cardUID | Body | Card UID | &lt;String&gt; | Required |   |
| cardDID | Body | Card DID | &lt;String&gt; | Required |   |
| unitCode | Body | Unit Code | &lt;String&gt; | Required |   |
| serviceCode | Body | Service Code | &lt;String&gt; | Required |   |

**RESPONSE**** PARAMS**

| Name | Type | Description | Data Type | Availability | Comment |
| --- | --- | --- | --- | --- | --- |
| status | Body | Status of the transaction | &lt;String&gt; | Required |   |
| statusCode | Body | Status Code of the transaction | &lt;String&gt; | Required |   |
| statusMsg | Body | Status Messages of the transaction | List&lt;String&gt; | Required |   |
| cardUID | Body | Valid Card UID or not | &lt;int&gt; | Required | Valid - 1Invalid - 0 |
| cardDID  | Body | Valid Card DID or not | &lt;int&gt; | Required | Valid - 1Invalid - 0 |
| unitCode | Body | Valid Unit code or not | &lt;int&gt; | Required | Valid - 1Invalid - 0 |
| serviceCode | Body | Valid Service code or not | &lt;int&gt; | Required | Valid - 1Invalid - 0 |

**SAMPLE REQUEST**

**Header**

POST /api/rest/v1/card/validate HTTP/1.1

User-Agent: &lt;agent&gt;

Host: &lt;HOST&gt;

Username: &lt;Username&gt;

Password: &lt;Password&gt;

**Body**

{

        &quot;cardUID&quot;:&quot;777000000006&quot;,

        &quot;cardDID&quot;:&quot;777000000086&quot;,

        &quot;unitCode&quot;:&quot;UT00421&quot;,

        &quot;serviceCode&quot;:&quot;Service001&quot;

}

**SAMPLE RESPONSE**

**Body**

{

  &quot;status&quot;: &quot;OK&quot;,

  &quot;statusCode&quot;: &quot;200&quot;,

  &quot;statusMsg&quot;: [

    &quot;Success&quot;

  ],

  &quot;cardUID&quot;: 1,

  &quot;cardDID&quot;: 1,

  &quot;unitCode&quot;: 1,

  &quot;serviceCode&quot;: 1

}