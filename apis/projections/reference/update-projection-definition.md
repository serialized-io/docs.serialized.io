# Update Projection Definition

{% api-method method="put" host="https://api.serialized.io" path="/projections/definitions/{projectionName}" %}
{% api-method-summary %}
Update projection definition
{% endapi-method-summary %}

{% api-method-description %}
Update projection definition
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="aggregated" type="boolean" %}
If true, creates an aggregated Projection. Default: `false`
{% endapi-method-parameter %}

{% api-method-parameter type="string" name="idField" %}
Field in the Projection to use as id field in queries. Defaults to the aggregate id.
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
Projection definition successfully updated
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

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i https://api.serialized.io/projections/definitions/orders \
  --header "Content-Type: application/json" \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  -X PUT \
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
    .path("orders")
    .request()
    .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
    .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
    .put(Entity.json(projectionDefinition));
```
{% endtab %}
{% endtabs %}

