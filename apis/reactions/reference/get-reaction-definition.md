# Get reaction definition

{% api-method method="get" host="https://api.serialized.io" path="/reactions/definitions/{reactionName}" %}
{% api-method-summary %}
Get reaction definition
{% endapi-method-summary %}

{% api-method-description %}
Get reaction definition
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="reactionName" type="string" required=true %}
The reaction name
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Success
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Reaction definition not found
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

