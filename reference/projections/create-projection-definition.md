# Create projection definition

{% api-method method="post" host="https://api.serialized.io" path="/projections/definitions" %}
{% api-method-summary %}
Create a new projection definition
{% endapi-method-summary %}

{% api-method-description %}
Create a new reaction definition
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
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
Projection definition successfully created
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

```
{% endtab %}
{% endtabs %}

