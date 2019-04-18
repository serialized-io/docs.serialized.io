# Delete Tenant

{% api-method method="delete" host="https://api.serialized.io" path="/tenants/{tenantId}" %}
{% api-method-summary %}
Delete Tenant
{% endapi-method-summary %}

{% api-method-description %}
This endpoint permanently deletes a Tenant and all its data, including all Events, Projections, and pending Reactions.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="tenantId" type="string" %}
ID of the Tenant to delete
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Tenant successfully deleted.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Could not find the given Tenant.
{% endapi-method-response-example-description %}

```javascript

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

