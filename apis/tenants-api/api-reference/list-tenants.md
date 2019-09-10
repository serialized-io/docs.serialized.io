# List Tenants

{% api-method method="get" host="https://api.serialized.io/" path="tenants" %}
{% api-method-summary %}
List Tenants
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to list Tenants.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Tenants successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{
    "tenants": [
        "tenantId": "a8c929ac-b59d-429b-8570-99c9a84f6b2c",
        "tenantNumber": "1",
        "addedAt": 1568105144988,
        "reference": "Acme Corp.",
        "deleted": false
    ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



