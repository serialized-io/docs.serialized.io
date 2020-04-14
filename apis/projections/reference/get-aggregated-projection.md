# Get Aggregated Projection

{% api-method method="get" host="https://api.serialized.io" path="/projections/aggregated/{projectionName}" %}
{% api-method-summary %}
Get Aggregated Projection
{% endapi-method-summary %}

{% api-method-description %}
Retrieves an aggregated Projection
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

{% api-method-path-parameters %}
{% api-method-parameter name="projectionName" type="string" required=true %}
The Projection name
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Aggregated Projection successfully retrieved
{% endapi-method-response-example-description %}

```javascript
{
  "projectionId": "22c3780f-6dcb-440f-8532-6693be83f21c",
  "createdAt": 1523518143967,
  "updatedAt": 1523518144467,
  "data": {}
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Projection not found
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  https://api.serialized.io/projections/aggregated/order-totals
```
{% endtab %}

{% tab title="Java" %}
```java
import com.google.common.collect.ImmutableMap;
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;

Client client = ClientBuilder.newClient();
URI apiRoot = URI.create("https://api.serialized.io");

Map response = client.target(apiRoot)
  .path("projections")
  .path("aggregated")
  .path("order-totals")
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

var request = new RestRequest("projections/aggregated/{projectionName}", Method.GET)
   .AddUrlSegment("projectionName", "order-totals")
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

client.get("projections/aggregated/order-totals")
    .then(function (response) {
      // Handle response
    })
    .catch(function (error) {
      // Handle error
    });
```
{% endtab %}
{% endtabs %}

