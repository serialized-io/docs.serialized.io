# Delete an Aggregate

{% api-method method="delete" host="https://api.serialized.io" path="/aggregates/{aggregateType}/{aggregateId}" %}
{% api-method-summary %}
Delete an aggregate
{% endapi-method-summary %}

{% api-method-description %}
Permanently delete an aggregate, including all events.
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
{% api-method-parameter name="deleteToken" type="string" required=false %}
Valid delete token. Will be included in the response to the first `DELETE` request
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Delete token created successfully
{% endapi-method-response-example-description %}

```javascript
{
  "deleteToken": "12c3780f-2dcb-340f-5532-5693be83f21c"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=204 %}
{% api-method-response-example-description %}
Aggregate successfully deleted
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Unknown delete token
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Example

This example requests a delete token for deleting a specific `order` aggregate

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  -X DELETE https://api.serialized.io/aggregates/order/d28bae0e-6f46-4c88-8935-b6e2c87ae4af
```
{% endtab %}

{% tab title="Java" %}
```java
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;

Client client = ClientBuilder.newClient();
URI apiRoot = URI.create("https://api.serialized.io");

Map response = client.target(apiRoot)
    .path("aggregates")
    .path("order")
    .path("99415be8-6819-4470-860c-c2933558d8d3")
    .request()
    .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
    .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
    .delete(Map.class);
    
String deleteToken = (String) response.get("deleteToken");
```
{% endtab %}

{% tab title="C\#" %}
```csharp
using RestSharp;
using System;

var client = new RestClient("https://api.serialized.io");

var request = new RestRequest("aggregates/order/{aggregateId}", Method.DELETE)
   .AddUrlSegment("aggregateId", "99415be8-6819-4470-860c-c2933558d8d3")
   .AddHeader("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
   .AddHeader("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>");

var response = client.Execute<Dictionary<string, Object>>(request);
var deleteToken = response.Data["deleteToken"]
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

client.delete("aggregates/order/99415be8-6819-4470-860c-c2933558d8d3")
    .then(function (response) {
      const deleteToken = response.data.deleteToken;
      // Use deleteToken
    })
    .catch(function (error) {
      // Handle error
    });
```
{% endtab %}
{% endtabs %}

#### Permanently deleting the aggregate using the delete token

This example permanently deletes an aggregate using a delete token that was returned in the response to the preceding delete request.

{% hint style="danger" %}
This is a permanent request that cannot be undone. If you need to be able to restore this data in any way you must back it up on your side before executing the deletion.
{% endhint %}

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  -X DELETE https://api.serialized.io/aggregates/order/d28bae0e-6f46-4c88-8935-b6e2c87ae4af?deleteToken=12c3780f-2dcb-340f-5532-5693be83f21c
```
{% endtab %}

{% tab title="Java" %}
```java
Response delete = client.target(apiRoot)
        .path("aggregates")
        .path("order")
        .path("99415be8-6819-4470-860c-c2933558d8d3")
        .queryParam("deleteToken", deleteToken)
        .request()
        .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
        .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
        .delete();
```
{% endtab %}

{% tab title="C\#" %}
```csharp
var request = new RestRequest("aggregates/order/{aggregateId}", Method.DELETE)
   .AddUrlSegment("aggregateId", "99415be8-6819-4470-860c-c2933558d8d3")
   .AddHeader("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
   .AddHeader("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
   .AddQueryParameter("deleteToken", deleteToken);

var response = client.Execute<Dictionary<string, Object>>(request);
```
{% endtab %}

{% tab title="Node" %}
```javascript
const requestConfig = {
  params: {
    deleteToken: deleteToken 
  }
};

client.delete("aggregates/order/99415be8-6819-4470-860c-c2933558d8d3", requestConfig)
    .then(function (response) {
      // Handle response
    })
    .catch(function (error) {
      // Handle error
    });
```
{% endtab %}
{% endtabs %}





