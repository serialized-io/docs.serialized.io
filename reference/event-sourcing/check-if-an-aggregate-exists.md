# Check if an aggregate exists

{% api-method method="head" host="https://api.serialized.io" path="/aggregates/{aggregateType}/{aggregateId}" %}
{% api-method-summary %}
Check if an aggregate exists
{% endapi-method-summary %}

{% api-method-description %}
Check if an aggregate exists without loading any events from the event store.
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
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Success
{% endapi-method-response-example-description %}

{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid aggregate type name or aggregate id
{% endapi-method-response-example-description %}
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
