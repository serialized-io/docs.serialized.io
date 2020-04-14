# Add Tenant

{% api-method method="post" host="https://api.serialized.io" path="/tenants" %}
{% api-method-summary %}
Add Tenant
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to create a Tenant.
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
{% api-method-parameter name="tenantId" type="string" required=true %}
Unique tenant id
{% endapi-method-parameter %}

{% api-method-parameter name="reference" type="string" required=false %}
Tenant reference
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Tenant successfully created
{% endapi-method-response-example-description %}

```javascript

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=204 %}
{% api-method-response-example-description %}
Tenant already created
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid Tenant id or reference.
{% endapi-method-response-example-description %}

```javascript

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i https://api.serialized.io/tenants \
  --header "Content-Type: application/json" \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  --data '
  {  
     "tenantId": "e9ef574f-4563-4d56-ad9e-0a2d5ce42004",
     "reference": "Acme Inc"     
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

Map<String, Object> tenant = ImmutableMap.of(
    "tenantId", "e9ef574f-4563-4d56-ad9e-0a2d5ce42004",
    "reference", "Acme Inc"
);

Response response = client.target(apiRoot)
    .path("tenants")
    .request()
    .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
    .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
    .post(Entity.json(tenant));
```
{% endtab %}

{% tab title="C\#" %}
```csharp
using System;
using System.Collections.Generic;
using RestSharp;

var tenant = new Dictionary<string, Object>
{
    { "tenantId", "e9ef574f-4563-4d56-ad9e-0a2d5ce42004" },
    { "reference", "Acme Inc" }
};

var request = new RestRequest("tenants", Method.POST)
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
  tenantId: "e9ef574f-4563-4d56-ad9e-0a2d5ce42004",
  reference: "Acme Inc"
};

client.post("tenants", definition)
    .then(function (response) {
      // Handle response
    })
    .catch(function (error) {
      // Handle error
    });

```
{% endtab %}
{% endtabs %}

