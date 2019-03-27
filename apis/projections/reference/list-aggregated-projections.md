# List Aggregated Projections

{% api-method method="get" host="https://api.serialized.io" path="/projections/aggregated" %}
{% api-method-summary %}
List all aggregated projections
{% endapi-method-summary %}

{% api-method-description %}
List all aggregated projections
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="projectionName" type="string" required=true %}
The projection name
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Serialized-Access-Key" type="string" required=true %}
Access key for the project
{% endapi-method-parameter %}

{% api-method-parameter name="Serialized-Secret-Access-Key" type="string" required=true %}
Secret access key for the project
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="sort" type="number" required=false %}
Sort string. Any combination of the following fields: projectionId, reference, createdAt, updatedAt. Add '+' and '-' prefixes to indicate ascending/descending sort order. Ascending order is default.
{% endapi-method-parameter %}

{% api-method-parameter name="skip" type="number" required=false %}
Number of entries to skip
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="number" required=false %}
Max number of entries to include in response. Default is 100.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

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



