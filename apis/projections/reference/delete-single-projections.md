# Delete/recreate Single Projections

{% api-method method="delete" host="https://api.serialized.io" path="/projections/single/{projectionName}" %}
{% api-method-summary %}
Delete/recreate single projections
{% endapi-method-summary %}

{% api-method-description %}
  
This call deletes all existing projections and starts a rebuild from the beginning of the event history. Keep in mind that this might take a while.
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
The projection name
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Projections successfully deleted.
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Projection type not found
{% endapi-method-response-example-description %}

```text

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
  -X DELETE https://api.serialized.io/projections/single/orders
```
{% endtab %}

{% tab title="Java" %}
```java
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;

Client client = ClientBuilder.newClient();
URI apiRoot = URI.create("https://api.serialized.io");

Response response = client.target(apiRoot)
    .path("projections")
    .path("single")
    .path("orders")
    .request()
    .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
    .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
    .delete();
```
{% endtab %}

{% tab title="C\#" %}
```csharp
using RestSharp;
using System;

var client = new RestClient("https://api.serialized.io");

var request = new RestRequest("projections/single/{projectionName}", Method.DELETE)
   .AddUrlSegment("projectionName", "orders")
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

client.delete("projections/single/orders")
    .then(function (response) {
      // Handle response
    })
    .catch(function (error) {
      // Handle error
    });
```
{% endtab %}
{% endtabs %}

