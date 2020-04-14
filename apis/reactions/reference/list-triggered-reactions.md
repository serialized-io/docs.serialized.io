# List triggered reactions

{% api-method method="get" host="https://api.serialized.io" path="/reactions/triggered" %}
{% api-method-summary %}
List triggered reactions
{% endapi-method-summary %}

{% api-method-description %}
List all reactions that have been executed already.
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
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
List successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{
  "reactions" : [ {
    "reactionId" : "141c606a-9db1-4d3b-9b5c-0d6bc03a3405",
    "reactionName" : "payment-processed-email-reaction",
    "aggregateType" : "payment",
    "aggregateId" : "b39bcc9b-581b-4457-b0e5-006458ac70ed",
    "eventId" : "4bf26a74-3918-4085-b90f-5f3ea510bf53",
    "createdAt" : 1586372740000,
    "finishedAt" : 1586372740756
  } ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

