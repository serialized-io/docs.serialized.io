# List scheduled reactions

{% api-method method="get" host="https://api.serialized.io" path="/reactions/scheduled" %}
{% api-method-summary %}
List scheduled reactions
{% endapi-method-summary %}

{% api-method-description %}
List all reactions scheduled for triggering in the future .
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Serialized-Access-Key" type="string" required=true %}
Serialized access key
{% endapi-method-parameter %}

{% api-method-parameter name="Serialized-Secret-Access-Key" type="string" required=true %}
Serialized secret key
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="skip" type="number" required=false %}
Number of entries to skip.
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="number" required=false %}
Max number of entries to include in response. Default is 10.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
List successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{
  "reactions" : [ {
    "reactionId" : "f9dfc984-bf19-4cc5-8e83-96643b3e74e1",
    "reactionName" : "payment-processed-email-reaction",
    "aggregateType" : "payment",
    "aggregateId" : "b39bcc9b-581b-4457-b0e5-006458ac70ed",
    "eventId" : "4bf26a74-3918-4085-b90f-5f3ea510bf53",
    "createdAt" : 1586372740000,
    "triggerAt" : 1586977540271
  } ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  https://api.serialized.io/reactions/scheduled
```
{% endtab %}

{% tab title="Java" %}
```java
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;

Client client = ClientBuilder.newClient();
URI apiRoot = URI.create("https://api.serialized.io");

Map response = client.target(apiRoot)
    .path("reactions")
    .path("scheduled")
    .request()
    .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
    .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
    .get(Map.class);
```
{% endtab %}

{% tab title="C\#" %}
```csharp
using RestSharp;
using System;

var client = new RestClient("https://api.serialized.io");

var request = new RestRequest("reactions/scheduled", Method.GET)
   .AddHeader("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
   .AddHeader("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>");

var response = client.Execute<Dictionary<string, Object>>(request);
```
{% endtab %}

{% tab title="Node" %}
```javascript
const axios = require("axios");

const client = axios.create({
  baseURL: "https://api.serialized.io",
  headers: {"Serialized-Access-Key": "<YOUR_ACCESS_KEY>"},
  headers: {"Serialized-Secret-Access-Key": "<YOUR_SECRET_ACCESS_KEY>"}
});

client.get("reactions/scheduled")
    .then(function (response) {
      // Handle response
    })
    .catch(function (error) {
      // Handle error
    });
```
{% endtab %}
{% endtabs %}

