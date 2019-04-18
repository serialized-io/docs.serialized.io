# List Aggregated Projections

{% api-method method="get" host="https://api.serialized.io" path="/projections/aggregated" %}
{% api-method-summary %}
List all aggregated projections
{% endapi-method-summary %}

{% api-method-description %}
List all aggregated projections
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="projectionName" type="string" required=true %}
The projection name
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="sort" type="number" required=false %}
Sort string. Any combination of the following fields: projectionId, reference, createdAt, updatedAt. Add '+' and '-' prefixes to indicate ascending/descending sort order. Ascending order is default.
{% endapi-method-parameter %}

{% api-method-parameter name="skip" type="number" required=false %}
Number of entries to skip
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="number" required=false %}
Max number of entries to include in response. Default is 100.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Aggregated Projections successfully received
{% endapi-method-response-example-description %}

```javascript
{
  "projections": [
    {
      "projectionId": "22c3780f-6dcb-440f-8532-6693be83f21c",
      "createdAt": 1523518143967,
      "updatedAt": 1523518144467,
      "data": {}
    }
  ],
  "hasMore": false,
  "totalCount": 1
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  https://api.serialized.io/projections/aggregated
```
{% endtab %}

{% tab title="Java" %}
```java
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;

Client client = ClientBuilder.newClient();
URI apiRoot = URI.create("https://api.serialized.io");

Map response = client.target(apiRoot)
    .path("projections")
    .path("aggregated")
    .request()
    .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
    .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
    .get(Map.class);
```
{% endtab %}
{% endtabs %}



