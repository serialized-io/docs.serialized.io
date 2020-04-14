# Delete scheduled reaction

{% api-method method="delete" host="https://api.serialized.io" path="/reactions/scheduled/:reactionId" %}
{% api-method-summary %}
Delete scheduled reaction
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to delete a reaction that has not yet been executed.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="reactionId" type="string" required=true %}
ID of the scheduled reaction to delete.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Serialized-Access-Key" type="string" required=true %}
Serialized access key
{% endapi-method-parameter %}

{% api-method-parameter name="Serialized-Secret-Access-Key" type="string" required=true %}
Serialized secret key
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Reaction successfully deleted.
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

