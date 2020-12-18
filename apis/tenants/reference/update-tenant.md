# Update Tenant

{% api-method method="put" host="https://api.serialized.io" path="/tenants/{tenantId}" %}
{% api-method-summary %}
Update Tenant
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to change the tenant reference.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="tenantId" type="string" %}
ID of the Tenant to update
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
{% api-method-parameter name="reference" type="string" required=true %}

The new reference
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}

{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Tenant reference successfully changed
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Could not find the given Tenant.
{% endapi-method-response-example-description %}

```javascript

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i https://api.serialized.io/tenants/e9ef574f-4563-4d56-ad9e-0a2d5ce42004 \
  --header "Content-Type: application/json" \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  --data '
  {  
     "reference": "A new reference"
  }
  '
```
{% endtab %}

{% tab title="Java" %}
```java
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;

Client client = ClientBuilder.newClient();
URI apiRoot = URI.create("https://api.serialized.io");

Response response = client.target(apiRoot)
  .path("tenants")
  .path("e9ef574f-4563-4d56-ad9e-0a2d5ce42004")
  .request()
  .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
  .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
  .put(json(Map.of("reference", "A new reference")));
```
{% endtab %}

{% tab title="C\#" %}
```csharp
using System;
using System.Collections.Generic;
using RestSharp;

var tenant = new Dictionary<string, Object>
{
    { "reference", "A new reference" }
};

var client = new RestClient("https://api.serialized.io");

var request = new RestRequest("tenants/{tenantId}", Method.PUT)
   .AddUrlSegment("tenantId", "e9ef574f-4563-4d56-ad9e-0a2d5ce42004")
   .AddHeader("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
   .AddHeader("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>");
   .AddJsonBody(tenant);

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
  reference: "A new reference"
};

client.put("tenants", definition)
    .then(function (response) {
      // Handle response
    })
    .catch(function (error) {
      // Handle error
    });
```
{% endtab %}
{% endtabs %}

