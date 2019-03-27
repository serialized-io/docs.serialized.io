# Delete/recreate Single Projections

{% api-method method="delete" host="https://api.serialized.io" path="/projections/single/{projectionName}" %}
{% api-method-summary %}
This call deletes all existing projections and starts a rebuild from the beginning of the event history. Keep in mind that this might take a while.
{% endapi-method-summary %}

{% api-method-description %}
Delete/recreate single projections
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="projectionName" type="string" required=true %}
The projection name
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Projection definition successfully deleted.
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Projection definition not found
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

