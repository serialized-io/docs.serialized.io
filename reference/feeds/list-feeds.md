# List feeds

{% api-method method="get" host="https://api.serialized.io" path="/feeds" %}
{% api-method-summary %}
List feeds
{% endapi-method-summary %}

{% api-method-description %}
Overview showing number of batches, aggregates and events per aggregate type.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Success
{% endapi-method-response-example-description %}

```text

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

