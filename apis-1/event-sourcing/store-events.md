# Store events

{% api-method method="post" host="https://api.serialized.io" path="/aggregates/{aggregateType}/events" %}
{% api-method-summary %}
Store events
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

{% api-method-body-parameters %}
{% api-method-parameter name="aggregateId" type="string" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="events" type="array" required=true %}

{% endapi-method-parameter %}


{% api-method-parameter name="expectedVersion" type="number" required=false %}

{% endapi-method-parameter %}

{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Cake successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{
    "name": "Cake's name",
    "recipe": "Cake's recipe name",
    "cake": "Binary cake"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Could not find a cake matching this query.
{% endapi-method-response-example-description %}

```javascript
{
    "message": "Ain't no cake like that."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}









