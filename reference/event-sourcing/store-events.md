# Store events

{% api-method method="post" host="https://api.serialized.io" path="/aggregates/{aggregateType}/events" %}
{% api-method-summary %}
Store events
{% endapi-method-summary %}

{% api-method-description %}
Stores all events in the request atomically. All events must refer to the same aggregate id.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="aggregateType" type="string" required=true %}
The aggregate type
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="aggregateId" type="string" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="events" type="array" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="expectedVersion" type="number" required=false %}

{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Events successfully stored.
{% endapi-method-response-example-description %}

```javascript

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid aggregate type name
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=409 %}
{% api-method-response-example-description %}
Conflict due to expected version mismatch
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}
Invalid request body
{% endapi-method-response-example-description %}

{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i https://api.serialized.io/aggregates/order/events \
  --header "Content-Type: application/json" \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  --data '
{
  "aggregateId": "2c3cf88c-ee88-427e-818a-ab0267511c84",
  "events": [
    {
      "eventId": "f2c8bfc1-c702-4f1a-b295-ef113ed7c8be",
      "eventType": "PaymentProcessed",
      "data": {
        "paymentMethod": "CARD",
        "amount": 1000,
        "currency": "SEK"
      },
      "encryptedData": "string"
    }
  ],
  "expectedVersion": 1
}
'
```
{% endtab %}
{% endtabs %}



