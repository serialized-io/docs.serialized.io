# Get Single Projection

{% api-method method="get" host="https://api.serialized.io" path="/projections/single/{projectionName}/{projectionId}" %}
{% api-method-summary %}
Get Single Projection
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="projectionName" type="string" required=true %}
The name of the Projection type
{% endapi-method-parameter %}

{% api-method-parameter name="projectionId" type="string" required=true %}
The id of the Projection instance
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="awaitCreation" type="integer" required=false %}
Max number of milliseconds to await the initial creation. Must be between 1 and 60000.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Single Projection successfully received
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
  https://api.serialized.io/projections/single/orders/84e3565e-cd61-44e7-9769-c4663588c4dd
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
   .path("single")
   .path("orders")
   .path("84e3565e-cd61-44e7-9769-c4663588c4dd")
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

var request = new RestRequest("projections/single/{projectionName}/{projectionId}", Method.GET)
   .AddUrlSegment("projectionName", "orders")
   .AddUrlSegment("projectionId", "84e3565e-cd61-44e7-9769-c4663588c4dd")
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

client.get("projections/single/orders/84e3565e-cd61-44e7-9769-c4663588c4dd")
    .then(function (response) {
      // Handle response
    })
    .catch(function (error) {
      // Handle error
    });
```
{% endtab %}
{% endtabs %}

