---
description: >-
  By loading all events for a single aggregate we can derive the current state
  of the Aggregate.
---

# Load an Aggregate

{% api-method method="get" host="https://api.serialized.io" path="/aggregates/{aggregateType}/{aggregateId}" %}
{% api-method-summary %}
Load an Aggregate
{% endapi-method-summary %}

{% api-method-description %}
Before appending events to your aggregate you typically load \(hydrate\) it by loading all previous events for that particular `aggregateId` and fast-forward it into its current state.   
  
To limit the size of the response the optional query parameters `since` and `limit` can be used. Note that the parameters represents _changes_ to the aggregate which does not necessarily correspond to the number of events that has been appended to it. The default `limit` is set to `1000`.  
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="aggregateType" type="string" required=true %}
The aggregate type
{% endapi-method-parameter %}

{% api-method-parameter name="aggregateId" type="string" required=true %}
The aggregate id
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="since" type="number" required=false %}
Version number to start reading from.
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="number" required=false %}
Version limit. Default is 1000.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Aggregate successfully loaded
{% endapi-method-response-example-description %}

```javascript
{
  "aggregateId": "22c3780f-6dcb-440f-8532-6693be83f21c",
  "aggregateVersion": 1,
  "aggregateType": "payment",
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
  ],
  "hasMore": false
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid aggregate type name or aggregate id
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
If the aggregate does not exist
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  https://api.serialized.io/aggregates/order/99415be8-6819-4470-860c-c2933558d8d3
```
{% endtab %}

{% tab title="Java" %}
```java
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;
import javax.ws.rs.core.UriBuilder;

Client client = ClientBuilder.newClient();
URI apiRoot = URI.create("https://api.serialized.io");
    
Map aggregateResponse = client.target(apiRoot)
    .path("aggregates")
    .path("order")
    .path("99415be8-6819-4470-860c-c2933558d8d3")
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

var request = new RestRequest("aggregates/order/{aggregateId}", Method.GET)
   .AddUrlSegment("aggregateId", "99415be8-6819-4470-860c-c2933558d8d3")
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

client.get("aggregates/order/99415be8-6819-4470-860c-c2933558d8d3")
    .then(function (response) {
      // Handle response
    })
    .catch(function (error) {
      // Handle error
    });

```
{% endtab %}
{% endtabs %}

