# List scheduled reactions

{% api-method method="get" host="https://api.serialized.io" path="/reactions/scheduled" %}
{% api-method-summary %}
List scheduled reactions
{% endapi-method-summary %}

{% api-method-description %}
List all reactions scheduled for triggering in the future .
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
    "reactionId" : "f9dfc984-bf19-4cc5-8e83-96643b3e74e1",
    "reactionName" : "payment-processed-email-reaction",
    "aggregateType" : "payment",
    "aggregateId" : "b39bcc9b-581b-4457-b0e5-006458ac70ed",
    "eventId" : "4bf26a74-3918-4085-b90f-5f3ea510bf53",
    "createdAt" : 1586372740000,
    "triggerAt" : 1586977540271
  } ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

