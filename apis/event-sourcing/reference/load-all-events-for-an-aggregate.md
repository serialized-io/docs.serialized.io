# Load an Aggregate

{% api-method method="get" host="https://api.serialized.io" path="/aggregates/{aggregateType}/{aggregateId}" %}
{% api-method-summary %}
Load an Aggregate
{% endapi-method-summary %}

{% api-method-description %}
By loading all events for a single aggregate we can derive the current state.
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
{% api-method-parameter name="since" type="number" required=false %}
Version number to start from
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="number" required=false %}
Version limit. Default is 1000.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Aggregate successfully loaded
{% endapi-method-response-example-description %}

```javascript
{
  "aggregateId": "22c3780f-6dcb-440f-8532-6693be83f21c",
  "aggregateVersion": 1,
  "aggregateType": "payment",
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
  "hasMore": false
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid aggregate type name or aggregate id
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
If the aggregate does not exist
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
todo
```
{% endtab %}
{% endtabs %}

