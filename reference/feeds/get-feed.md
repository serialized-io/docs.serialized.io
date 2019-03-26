# Get feed of events

{% api-method method="get" host="https://api.serialized.io" path="/feeds/{name}" %}
{% api-method-summary %}
Get feed of events
{% endapi-method-summary %}

{% api-method-description %}
Get all events for all aggregates given a type \(feed name\). The payload is returned with the event batches in insertion order, each with a unique sequence number.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="name" type="string" required=true %}
The feed name
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
{% api-method-parameter name="since" type="number" required=false %}
Sequence number to start feeding from
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="number" required=false %}
Limit of number of event batches in response. Default is 1000.
{% endapi-method-parameter %}

{% api-method-parameter name="from" type="number" required=false %}
ISO 8601 date-time string to start from, eg. 2017-07-21T17:32:28. Must be used in combination with 'to' parameter.
{% endapi-method-parameter %}

{% api-method-parameter name="to" type="number" required=false %}
ISO 8601 data-time string to stop at, eg. 2017-07-21T17:32:28. Must be used in combination with 'from' parameter.
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

