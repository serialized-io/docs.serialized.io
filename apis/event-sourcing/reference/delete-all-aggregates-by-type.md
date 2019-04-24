# Delete all Aggregates by type

{% api-method method="delete" host="https://api.serialized.io" path="/aggregates/{aggregateType}" %}
{% api-method-summary %}
Delete all aggregates by type
{% endapi-method-summary %}

{% api-method-description %}
Permanently deletes all aggregates, including all events for a given aggregate type.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="aggregateType" type="string" required=true %}
The aggregate type
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
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
Aggregate type successfully deleted
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

This example requests a delete token for deleting all aggregates for a given type

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  -X DELETE https://api.serialized.io/aggregates/order
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
    .request()
    .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
    .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
    .delete(Map.class);
    
String deleteToken = (String) response.get("deleteToken");
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

client.delete("aggregates/order")
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

#### Permanently deleting the aggregate type using the delete token

This example permanently deletes an aggregate type using a delete token that was returned in the response to the preceding delete request.

{% hint style="danger" %}
This is a permanent request that cannot be undone. If you need to be able to restore this data in any way you must back it up on your side before executing the deletion.
{% endhint %}

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  -X DELETE https://api.serialized.io/aggregates/order?deleteToken=12c3780f-2dcb-340f-5532-5693be83f21c
```
{% endtab %}

{% tab title="Java" %}
```java
Response delete = client.target(apiRoot)
        .path("aggregates")
        .path("order")
        .queryParam("deleteToken", deleteToken)
        .request()
        .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
        .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
        .delete();
```
{% endtab %}

{% tab title="Node" %}
```javascript
const requestConfig = {
  params: {
    deleteToken: deleteToken 
  }
};

client.delete("aggregates/order", requestConfig)
    .then(function (response) {
      // Handle response
    })
    .catch(function (error) {
      // Handle error
    });
```
{% endtab %}
{% endtabs %}

