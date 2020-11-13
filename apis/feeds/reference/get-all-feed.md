# Get events from all feeds

{% api-method method="get" host="https://api.serialized.io" path="/feeds/\_all" %}
{% api-method-summary %}
Get events from all feeds
{% endapi-method-summary %}

{% api-method-description %}
Get all events for all aggregates for all types \(feed names\). The payload is returned with the event batches in insertion order, each with a unique sequence number.
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
{% api-method-parameter name="partitionNumber" type="number" required=false %}
The partition number to request.
{% endapi-method-parameter %}

{% api-method-parameter name="partitionCount" type="number" required=false %}
The expected total number of partitions, i.e. the total number of consumers feeding in parallel.
{% endapi-method-parameter %}

{% api-method-parameter name="since" type="number" required=false %}
Sequence number to start feeding from \(exclusive\).
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="number" required=false %}
Limit of number of event batches in response. Default is 1000.
{% endapi-method-parameter %}

{% api-method-parameter name="from" type="number" required=false %}
ISO 8601 date-time string to start from, eg. 2017-07-21T17:32:28. Must be used in combination with 'to' parameter.
{% endapi-method-parameter %}

{% api-method-parameter name="to" type="number" required=false %}
ISO 8601 data-time string to stop at, eg. 2017-07-21T17:32:28. Must be used in combination with 'from' parameter.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Feed successfully received
{% endapi-method-response-example-description %}

```javascript
{
  "entries": [
    {
      "sequenceNumber": 12314,
      "aggregateId": "22c3780f-6dcb-440f-8532-6693be83f21c",
      "timestamp": 1503386583474,
      "feedName": "order",
      "events": [
        {
          "eventId": "f2c8bfc1-c702-4f1a-b295-ef113ed7c8be",
          "eventType": "PaymentProcessed",
          "data": {
            "paymentMethod": "CARD",
            "amount": 1000,
            "currency": "SEK"
          },
          "encryptedData": "string"
        }
      ]
    }
  ],
  "hasMore": false,
  "currentSequenceNumber": 123456
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  https://api.serialized.io/feeds/_all
```
{% endtab %}

{% tab title="Java" %}
```java
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;
import javax.ws.rs.core.Response;

Client client = ClientBuilder.newClient();
URI apiRoot = URI.create("https://api.serialized.io");

Map response = client.target(apiRoot)
   .path("feeds")
   .path("_all")
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

var request = new RestRequest("feeds/_all", Method.GET)
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

client.get("feeds/_all")
    .then(function (response) {
      // Handle response
    })
    .catch(function (error) {
      // Handle error
    });
```
{% endtab %}
{% endtabs %}

