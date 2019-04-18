# List reaction definitions

{% api-method method="get" host="https://api.serialized.io" path="/reactions/definitions" %}
{% api-method-summary %}
List reaction definitions
{% endapi-method-summary %}

{% api-method-description %}
List all reaction definitions
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Reaction definitions successfully received
{% endapi-method-response-example-description %}

```javascript
{
  "definitions": [
    {
      "reactionName": "payment-processed-email-reaction",
      "feedName": "payment",
      "reactOnEventType": "PaymentProcessed",
      "cancelOnEventTypes": [
        "OrderCanceledEvent"
      ],
      "triggerTimeField": "my.event.data.field",
      "offset": "PT1H",
      "action": {
        "actionType": "HTTP_POST"
      }
    }
  ]
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
  https://api.serialized.io/reactions/definitions
```
{% endtab %}

{% tab title="Java" %}
```java
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;

Client client = ClientBuilder.newClient();
URI apiRoot = URI.create("https://api.serialized.io");

Map response = client.target(apiRoot)
    .path("reactions")
    .path("definitions")
    .request()
    .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
    .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
    .get(Map.class);
```
{% endtab %}
{% endtabs %}

