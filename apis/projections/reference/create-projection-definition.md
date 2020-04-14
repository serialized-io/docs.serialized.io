# Create Projection Definition

{% api-method method="post" host="https://api.serialized.io" path="/projections/definitions" %}
{% api-method-summary %}
Create a new projection definition
{% endapi-method-summary %}

{% api-method-description %}
Create a new reaction definition
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

{% api-method-body-parameters %}
{% api-method-parameter name="idField" type="string" required=false %}
Field in the Projection to use as id field in queries. Defaults to the aggregate id.
{% endapi-method-parameter %}

{% api-method-parameter name="aggregated" type="boolean" required=false %}
If true, creates an aggregated Projection. Default: `false`
{% endapi-method-parameter %}

{% api-method-parameter name="projectionName" type="string" required=true %}
Name of the projection type
{% endapi-method-parameter %}

{% api-method-parameter name="feedName" type="string" required=true %}
Name of the feed
{% endapi-method-parameter %}

{% api-method-parameter name="handlers" type="array" required=true %}
Handlers for the reaction. See examples below.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Projection definition successfully created
{% endapi-method-response-example-description %}

```javascript

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=409 %}
{% api-method-response-example-description %}
If projection name is not unique
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}
If request body is invalid
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
curl -i https://api.serialized.io/projections/definitions \
  --header "Content-Type: application/json" \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  --data '
  {
  "projectionName": "orders",
  "feedName": "order",
  "handlers": [
    {
      "eventType": "OrderPlacedEvent",
      "functions": [
        {
          "function": "set",
          "targetSelector": "$.projection.status",
          "rawData": "PLACED"
        },
        {
          "function": "set",
          "targetSelector": "$.projection.orderAmount",
          "eventSelector": "$.event.orderAmount"
        }
      ]
    }
  ]
}
'
```
{% endtab %}

{% tab title="Java" %}
```java
import com.google.common.collect.ImmutableMap;
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;

Client client = ClientBuilder.newClient();
URI apiRoot = URI.create("https://api.serialized.io");

Map<String, Object> projectionDefinition = ImmutableMap.of(
    "projectionName", "orders",
    "feedName", "order",
    "handlers", ImmutableList.of(
        ImmutableMap.of(
            "eventType", "OrderPlacedEvent",
            "functions", ImmutableList.of(
                ImmutableMap.of(
                    "function", "set",
                    "targetSelector", "$.projection.status",
                    "rawData", "PLACED"
                ),
                ImmutableMap.of(
                    "function", "set",
                    "targetSelector", "$.projection.orderAmount",
                    "eventSelector", "$.event.orderAmount"
                )
            )

        )
    )
);

Response response = client.target(apiRoot)
    .path("projections")
    .path("definitions")
    .request()
    .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
    .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
    .post(Entity.json(projectionDefinition));
```
{% endtab %}

{% tab title="C\#" %}
```csharp
using System;
using System.Collections.Generic;
using RestSharp;

var definition = new Dictionary<string, Object>
{
    { "projectionName", "orders" },
    { "feedName", "order" },
    { "handlers", new List<Dictionary<string, Object>>
        {
            new Dictionary<string, Object>
            {
                {"eventType", "OrderPlacedEvent"},
                {"functions", new List<Dictionary<string, Object>>
                    {
                        new Dictionary<string, Object>
                        {
                            {"function", "set"},
                            {"targetSelector", "$.projection.status"},
                            { "rawData", "PLACED"}
                        },
                        new Dictionary<string, Object>
                        {
                            {"function", "set"},
                            {"targetSelector", "$.projection.orderAmount"},
                            { "eventSelector", "$.event.orderAmount"}
                        }
                    }
                }
            }
        }
    }
};

var request = new RestRequest("projections/definitions", Method.POST)
   .AddHeader("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
   .AddHeader("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>");
   .AddJsonBody(definition);

var response = client.Execute(request);
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

const definition = {
  projectionName: "orders",
  feedName: "order",
  handlers: [
    {
      eventType: "OrderPlacedEvent",
      functions: [
        {
          "function": "set",
          "targetSelector": "$.projection.status",
          "rawData": "PLACED"
        },
        {
          "function": "set",
          "targetSelector": "$.projection.orderAmount",
          "eventSelector": "$.event.orderAmount"
        }
      ]
    }
  ]
};

client.post("projections/definitions", definition)
    .then(function (response) {
      // Handle response
    })
    .catch(function (error) {
      // Handle error
    });
```
{% endtab %}
{% endtabs %}

