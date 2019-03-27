# Delete all Aggregates by type

{% api-method method="delete" host="https://api.serialized.io" path="/aggregates/{aggregateType}" %}
{% api-method-summary %}
Delete all aggregates by type
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
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Events successfully stored.
{% endapi-method-response-example-description %}

```text

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
todo
```
{% endtab %}
{% endtabs %}

