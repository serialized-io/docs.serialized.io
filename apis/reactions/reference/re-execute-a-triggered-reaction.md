# Re-execute a triggered reaction

{% api-method method="post" host="https://api.serialized.io" path="/reactions/triggered/:reactionId" %}
{% api-method-summary %}
Re-execute a triggered reaction
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to re-execute an already executed reaction.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Serialized-Access-Key" type="string" required=true %}
Serialized access key
{% endapi-method-parameter %}

{% api-method-parameter name="Serialized-Secret-Access-Key" type="string" required=true %}
Serialized secret key
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-path-parameters %}
{% api-method-parameter name="reactionId" type="string" required=true %}
ID of the reaction to re-execute.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Reaction successfully executed.
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

