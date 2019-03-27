# Reacting to events

## Reacting to events

Reactions can be used to trigger external services when certain events occur.

The following examples shows the basic APIs for working with reactions.

### Create event reaction definition  <a id="create-event-reaction-definition"></a>

As an example, we are going to set up a reaction that posts notifications every time an order is placed. We need to specify the feed name and the event type to react to.

We use a `POST` command to configure our reaction:

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i https://api.serialized.io/reactions/definitions \
  --header "Content-Type: application/json" \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  --data '
  {
    "reactionName": "new-order-notifier",
    "feedName": "order",
    "reactOnEventType": "OrderPlacedEvent",
    "action": {
      "actionType": "HTTP_POST",
      "targetUri": "https://some-server.com"
    }
  }
  '
```
{% endtab %}
{% endtabs %}

```bash
HTTP/1.1 200 Created
Content-Length: 0
```

Next time an `OrderPlacedEvent` happens you will get a POST request to your `some-server.com` looking something like this:

```javascript
{  
   "metadata":{  
      "aggregateId":"723ecfce-14e9-4889-98d5-a3d0ad54912f",
      "timestamp":1505376578800,
      "sequenceNumber":1
   },
   "event":{  
      "eventId":"127b80b5-4a05-4774-b870-1c9a2e2a27a3",
      "eventType":"OrderPlacedEvent",
      "data":{  
         "customerId":"some-test-id-1",
         "orderAmount":12345
      }
   }
}
```

