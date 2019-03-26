---
description: >-
  The Event Reaction API provides support for connecting your events to
  side-effect behavior such as sending e-mails, calling your backend or any
  other external service.
---

# Reaction

Reactions are configured to react to a specific event and trigger an action whenever that event is processed by the event reactor. An example could be sending a confirmation email to a customer when an order has been successfully placed.

It is not possible for a Reaction to trigger on an event that has happened in the past. Reactions are always configured to listen to events from the moment when the Reaction is configured.

## Reaction definitions

Reaction definitions dictates how and when side-effects should trigger.

```text
https://api.serialized.io/reactions/definitions
```

## Actions

When a Reaction is triggered it will execute its configured Action.

### HTTP Post

The action type \(`HTTP_POST`\) requires the field `targetUri` to be populated with the address that should receive the POST request. Optionally the field `httpHeaders` can be set, i.e. to handle Basic Authentication etc.

### Slack

The action type \(`SLACK_POST`\) requires the field `targetUri` to be populated with a valid [Slack webhook URI](https://api.slack.com/incoming-webhooks). The message text is specified in the field `body`. Simple templating is supported and event data can be accessed using dot \(.\) notation.

Example of a Reaction posting a notification to Slack:

{% code-tabs %}
{% code-tabs-item title="Slack reaction example" %}
```javascript
{
  "reactionName": "on-order-placed-slack-notifier",
  "feedName": "order",
  "reactOnEventType": "OrderPlacedEvent",
  "action": {
    "actionType": "SLACK_POST",
    "targetUri": "https://hooks.slack.com/services/T0000/B0000/XXXX",
    "body": "A new order with number ${event.data.orderNumber} was placed by ${event.data.customer.email}!"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

