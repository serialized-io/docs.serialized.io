# Store Events

{% api-method method="post" host="https://api.serialized.io" path="/aggregates/{aggregateType}/{aggregateId}/events" %}
{% api-method-summary %}
Store Events
{% endapi-method-summary %}

{% api-method-description %}
Stores all events in the request atomically. All events must refer to the same aggregate id.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="aggregateType" type="string" required=true %}
The aggregate type.
{% endapi-method-parameter %}

{% api-method-parameter name="aggregateId" type="string" required=true %}
Aggregate id. Must be UUID.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Serialized-Access-Key" type="string" required=true %}
Serialized access key
{% endapi-method-parameter %}

{% api-method-parameter name="Serialized-Secret-Access-Key" type="string" required=true %}
Serialized secret key
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="events" type="array" required=true %}
Array of events
{% endapi-method-parameter %}

{% api-method-parameter name="expectedVersion" type="number" required=false %}
Expected version of the aggregate for optimistic concurrency control.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Events successfully stored.
{% endapi-method-response-example-description %}

```javascript

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid aggregate type name
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=409 %}
{% api-method-response-example-description %}
Conflict due to expected version mismatch
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}
Invalid request body
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i https://api.serialized.io/aggregates/order/2c3cf88c-ee88-427e-818a-ab0267511c84/events \
  --header "Content-Type: application/json" \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  --data '
{
  "events": [
    {
      "eventType": "PaymentProcessed",
      "data": {
        "paymentMethod": "CARD",
        "amount": 1000,
        "currency": "SEK"
      }
    }
  ],
  "expectedVersion": 0
}
'
```
{% endtab %}

{% tab title="Java" %}
```java
import com.google.common.collect.ImmutableList;
import com.google.common.collect.ImmutableMap;
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;

Client client = ClientBuilder.newClient();
URI apiRoot = URI.create("https://api.serialized.io");

Map eventBatch = ImmutableMap.of(
    "events", ImmutableList.of(
        ImmutableMap.of(
            "eventType", "PaymentProcessed",
            "data", ImmutableMap.of(
                "paymentMethod", "CARD",
                "amount", 1000,
                "currency", "SEK"
            )
        )
    ),
    "expectedVersion", "0"
);

Response response = client.target(apiRoot)
    .path("aggregates")
    .path("order")
    .path("3070b6fb-f31b-4a8e-bc03-e22d38f4076e")
    .path("events")
    .request(MediaType.APPLICATION_JSON_TYPE)
    .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
    .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
    .post(Entity.json(eventBatch));
```
{% endtab %}

{% tab title="C\#" %}
```csharp
using System;
using System.Collections.Generic;
using RestSharp;

var eventBatch = new Dictionary<string, Object>
{
    { "events", new List<Dictionary<string, Object>>
        {
            new Dictionary<string, Object>
            {
                { "eventType", "PaymentProcessed" },
                { "data", new Dictionary<string, Object>
                    {
                        { "paymentMethod", "CARD" },
                        { "amount", 1000 },
                        { "currency", "SEK" }
                    } 
                }
            }
        }
    }
};

var postRequest = new RestRequest("aggregates/payments/{aggregateId}/events", Method.POST)
   .AddUrlSegment("aggregateId", "99415be8-6819-4470-860c-c2933558d8d2")
   .AddHeader("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
   .AddHeader("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>");
   .AddJsonBody(eventBatch);

var response = client.Execute(postRequest);
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

const eventBatch = {
  events: [
    {
      eventType: "PaymentProcessed",
      data: {
        paymentMethod: "CARD",
        amount: 1000,
        currency: "SEK"
      }
    }
  ]
};

client.post("aggregates/order/99415be8-6819-4470-860c-c2933558d8d3/events", eventBatch)
    .then(function (response) {
      // Handle response
    })
    .catch(function (error) {
      // Handle error
    });
```
{% endtab %}
{% endtabs %}

