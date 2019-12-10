---
description: >-
  To present your event data in multiple ways you can define projections that
  process your events and generate data structures that can be fetched, listed
  and searched for in the Serialized API.
---

# Projecting events

## Creating your first projection definition

The first part of projecting data is creating a projection definition. A definition defines what events should be processed and how they should be processed into a data structure that is more suitable for the client to query.

{% hint style="info" %}
It is a good idea to create multiple projection definitions for different use cases. Projections are cheap views and making them tailored for the client makes your data easy to use.
{% endhint %}

If you performed the first steps explained in [Working with aggregates](working-with-aggregates.md) you now have a stream of `order` events. To be able to query the current state of a particular order, without having to load all its events, we could configure a projection.

We use a `POST` command to create our projection definition:

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i https://api.serialized.io/projections/definitions \
  --header "Content-Type: application/json" \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  --data '
  {
  "projectionName": "orders",
  "feedName": "order",
  "handlers": [
    {
      "eventType": "OrderPlacedEvent",
      "functions": [
        {
          "function": "set",
          "targetSelector": "$.projection.status",
          "rawData": "PLACED"
        },
        {
          "function": "set",
          "targetSelector": "$.projection.orderAmount",
          "eventSelector": "$.event.orderAmount"
        }
      ]
    },
    {
      "eventType": "OrderPaidEvent",
      "functions": [
        {
          "function": "set",
          "targetSelector": "$.projection.status",
          "rawData": "PAID"
        }
      ]
    },
    {
      "eventType": "OrderShippedEvent",
      "functions": [
        {
          "function": "set",
          "targetSelector": "$.projection.status",
          "rawData": "SHIPPED"
        }
      ]
    },
    {
      "eventType": "OrderCancelledEvent",
      "functions": [
        {
          "function": "set",
          "targetSelector": "$.projection.status",
          "rawData": "CANCELLED"
        }
      ]
    }
  ]
}
'
```
{% endtab %}
{% endtabs %}

### Query our projection

Great! We can now access the projection and check the current state of our order!

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  https://api.serialized.io/projections/single/orders/723ecfce-14e9-4889-98d5-a3d0ad54912f
```

{% code title="Response" %}
```bash
HTTP/1.1 200 OK
Content-Type: application/json
ETag: "c604ac5bd1a51a798accc5717bd5109f"
Last-Modified: Mon, 18 Sep 2017 17:01:23 GMT
Vary: Accept-Encoding
Content-Length: 125

{
  "projectionId":"723ecfce-14e9-4889-98d5-a3d0ad54912f",
  "updatedAt":1505754083976,
  "data": {
    "orderAmount":"12345",
    "status":"PAID"
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

